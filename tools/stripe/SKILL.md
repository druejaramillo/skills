---
name: stripe
description: Guidance for building Stripe payment flows with Go, templ, htmx, and Tailwind. Use when integrating Checkout Sessions, PaymentIntents, subscriptions, saved payment methods, or Stripe webhooks.
---

# Stripe Payment Systems (Go + templ + htmx + Tailwind)

Use Stripe for payment collection and billing. Keep the database and verified webhook events authoritative for application state. Verify the API shape and required parameters against the [Stripe documentation](https://docs.stripe.com) for the SDK version in use.

## Pick the Smallest Flow

| Need | Recommended primitives |
| --- | --- |
| Take a one-time payment | Checkout Session with `mode=payment` |
| Collect payment details for a later charge | Checkout Session with `mode=setup` and a SetupIntent |
| Save a method while taking a payment | Checkout Session with `payment_intent_data.setup_future_usage=off_session` |
| Sell a recurring plan | Checkout Session with `mode=subscription` plus webhooks |
| Charge an existing saved method | Off-session PaymentIntent, with a recovery path for authentication |

Start with Checkout unless the product needs a custom payment experience. Do not treat a success redirect as proof that a payment, subscription, or provisioning task is complete; process verified webhooks as well.

## Core Objects

- **Checkout Session**: Coordinates a hosted, embedded, or custom Checkout flow for payment, setup, or subscription modes.
- **PaymentIntent**: Represents a one-time payment attempt and its status.
- **SetupIntent**: Collects and verifies a payment method for later use.
- **PaymentMethod**: A customer's card, bank account, or other payment instrument.
- **Customer**: Owns reusable payment methods and subscriptions.
- **Subscription and Invoice**: Model recurring billing and payment attempts.

## Setup

Install the Stripe Go SDK version used by the project:

```bash
go get github.com/stripe/stripe-go/v84
```

Configure secrets outside source control:

- `STRIPE_SECRET_KEY`
- `STRIPE_PUBLISHABLE_KEY`
- `STRIPE_WEBHOOK_SECRET`

Keep the secret key and webhook secret on the server. Load Stripe.js from `https://js.stripe.com` rather than serving a copied bundle.

## One-Time Checkout

Create the Checkout Session on the server. Use a real price ID in production when possible so prices are managed in Stripe rather than duplicated in application code.

```go
sc := stripe.NewClient(os.Getenv("STRIPE_SECRET_KEY"))

params := &stripe.CheckoutSessionCreateParams{
    Mode: stripe.String(stripe.CheckoutSessionModePayment),
    LineItems: []*stripe.CheckoutSessionCreateLineItemParams{
        {
            Price:    stripe.String("price_..."),
            Quantity: stripe.Int64(1),
        },
    },
    SuccessURL: stripe.String("https://example.com/return?session_id={CHECKOUT_SESSION_ID}"),
    CancelURL:  stripe.String("https://example.com/checkout/cancelled"),
}

session, err := sc.V1CheckoutSessions.Create(ctx, params)
if err != nil {
    // Return a safe server error and record diagnostics.
}
```

Redirect to the returned Checkout URL for a hosted session. For embedded or custom flows, return only the necessary client secret or session data to the browser, then initialize Stripe.js with the publishable key.

On the return page, retrieve the session server-side and show a clear status. The return page is for user feedback; webhook processing remains the source of truth.

## Save Payment Methods and Charge Later

Use setup mode when there is no immediate charge:

```go
params := &stripe.CheckoutSessionCreateParams{
    Mode:     stripe.String(stripe.CheckoutSessionModeSetup),
    Currency: stripe.String(stripe.CurrencyUSD),
    Customer: stripe.String(customerID),
    SuccessURL: stripe.String(
        "https://example.com/success?session_id={CHECKOUT_SESSION_ID}",
    ),
}
```

For a one-time payment that should support a later off-session charge, set `payment_intent_data.setup_future_usage=off_session` and obtain the customer's informed consent. Saved-method display, removal, and redisplay policies affect active subscriptions and should be designed deliberately.

An off-session charge needs a Customer, a saved PaymentMethod, `off_session=true`, and `confirm=true`. Authentication can still be required, so preserve a user-visible way to update the payment method instead of repeatedly retrying the charge.

## Subscriptions

Use Checkout with `mode=subscription` for a fixed-price subscription. Store the Stripe customer, price, subscription, and invoice IDs needed by the application.

At minimum, handle these events:

- `checkout.session.completed`: associate the session with the local user or order.
- `invoice.paid`: provision or continue paid access.
- `invoice.payment_failed`: notify the customer and offer a payment-method recovery flow.
- `customer.subscription.deleted`: remove access at the appropriate end of service.

Model the product's own access rules explicitly. Stripe status alone does not determine whether an account should retain access during trials, grace periods, refunds, or manual overrides.

## Webhooks

Verify the signature against the raw request body before parsing or acknowledging an event. Store every processed event ID with a uniqueness constraint because events can be retried or delivered more than once.

```go
func handleStripeWebhook(w http.ResponseWriter, r *http.Request) {
    if r.Method != http.MethodPost {
        http.Error(w, "method not allowed", http.StatusMethodNotAllowed)
        return
    }

    payload, err := io.ReadAll(io.LimitReader(r.Body, 65536))
    if err != nil {
        http.Error(w, "read body failed", http.StatusBadRequest)
        return
    }

    event, err := webhook.ConstructEvent(
        payload,
        r.Header.Get("Stripe-Signature"),
        os.Getenv("STRIPE_WEBHOOK_SECRET"),
    )
    if err != nil {
        http.Error(w, "bad signature", http.StatusBadRequest)
        return
    }

    if err := processEventIdempotently(r.Context(), event); err != nil {
        http.Error(w, "event processing failed", http.StatusInternalServerError)
        return
    }

    w.WriteHeader(http.StatusOK)
}
```

The handler should complete quickly. If processing must be asynchronous, record the verified event transactionally before returning `2xx`, then process it with a retryable worker. Do not start untracked background work and acknowledge before the event is durable.

## htmx and Server-Rendered UI

- Create sessions in server handlers, never in browser JavaScript.
- Return a redirect, a server-rendered fragment, or narrowly scoped JSON such as `{ "clientSecret": "cs_..." }` as required by the chosen Stripe UI.
- Let Stripe Elements own payment fields. Use the Appearance API for the embedded component and Tailwind for application UI.
- Use `session_id={CHECKOUT_SESSION_ID}` in the return URL only as an identifier to retrieve session state server-side.

## Production Checklist

### Security

- Verify every webhook signature using the raw body.
- Limit webhook request-body size.
- Keep publishable, secret, and webhook keys separate.
- Never trust a client-provided amount, product, Stripe object ID, or payment status without server-side authorization and verification.

### Reliability

- Persist Stripe IDs alongside local order or account IDs.
- Deduplicate webhooks by `event.id`.
- Make provisioning and deprovisioning idempotent.
- Handle event ordering and delayed delivery.
- Test failed payments, cancellation, retries, and authentication-required recovery.

### Customer Experience and Compliance

- Explain redirects, pending states, and failed payment recovery clearly.
- Obtain the appropriate consent before saving payment methods or charging later.
- Review tax, refund, privacy, and consumer-disclosure obligations for the business and regions served.
