# Go Testing Guidance

Use this when the implementation target is Go.

## Default Test Style

Prefer table-driven tests with subtests for related cases.

- Use one top-level `TestXxx` per behavior area.
- Use `t.Run(...)` for each case so failures are named clearly.
- Keep each subtest focused on one behavior variation.
- Name subtests by behavior or scenario, not by arbitrary numbers.

Example:

```go
func TestParseUserID(t *testing.T) {
	tests := []struct {
		name    string
		input   string
		want    int
		wantErr bool
	}{
		{name: "valid decimal", input: "42", want: 42},
		{name: "empty string", input: "", wantErr: true},
	}

	for _, tt := range tests {
		tt := tt
		t.Run(tt.name, func(t *testing.T) {
			got, err := ParseUserID(tt.input)
			if tt.wantErr {
				if err == nil {
					t.Fatal("expected error")
				}
				return
			}

			if err != nil {
				t.Fatalf("unexpected error: %v", err)
			}
			if got != tt.want {
				t.Fatalf("got %d, want %d", got, tt.want)
			}
		})
	}
}
```

## Best Practices

- Test through public behavior when possible. Prefer exported seams or package-level behavior over private helper tests.
- Keep tests deterministic. Avoid real time, random data, network calls, and shared global state unless the test is explicitly about those things.
- Keep tests narrow and fast. Prefer `go test ./path/to/pkg -run TestName` during red-green loops, then broaden later.
- Use `t.Helper()` in assertion helpers so failure lines point at the caller.
- Use `t.TempDir()` for filesystem tests instead of hand-managed temp paths.
- Use `t.Setenv()` for environment-dependent behavior instead of mutating process env manually.
- Compare errors intentionally. Check for `errors.Is` or `errors.As` when matching wrapped errors instead of string equality.
- Prefer simple direct assertions. Avoid assertion libraries unless the repo already uses one consistently.
- If comparing complex structs, keep expectations readable. Small explicit field checks are often better than giant opaque diffs.
- Use table-driven tests when multiple cases exercise the same behavior shape. Do not force tables for a single simple case.
- Use subtests to organize scenarios, not to hide unrelated behaviors inside one giant test.
- Use `t.Parallel()` only when the test and code under test are truly isolated. Do not add it by reflex.
- Avoid over-mocking. Prefer real collaborators when they are cheap and deterministic. Introduce fakes at package boundaries, not for every internal call.
- For HTTP handlers, prefer `httptest.NewRequest` and `httptest.NewRecorder`.
- For HTTP servers, prefer `httptest.NewServer` when the behavior is meaningfully exercised over HTTP.
- For concurrent code, make the synchronization explicit in the test. Avoid sleep-based timing tests when a channel, context, wait group, or hook can make the signal deterministic.
- Keep failure messages concrete: what input was used, what happened, what was expected.

## Subtest Gotchas

- Capture the loop variable before `t.Run` when needed:

```go
for _, tt := range tests {
	tt := tt
	t.Run(tt.name, func(t *testing.T) {
		...
	})
}
```

- This matters even more if the subtest uses `t.Parallel()`.
- Do not make a single table cover unrelated behavior categories. Split into multiple top-level tests when the setup or intent changes materially.

## TDD Guidance In Go

During phase `4`:

- Add one new subtest or one new focused test for the current TODO slice.
- Run the narrowest command that proves red, such as `go test ./path/to/pkg -run 'TestParseUserID/empty_string$'`.

During phase `5`:

- Make only the minimum code change needed for that new test or subtest.
- Re-run the same narrow command first.

During phase `6`:

- If refactoring, run the focused command, then the broader package command.

## Anti-Patterns

- One giant test with many unlabelled scenarios
- Tables whose fields mirror implementation details instead of behavior
- Comparing error strings when sentinel or typed errors exist
- Using sleeps to "wait" for concurrent behavior without a deterministic signal
- Mocking internal functions that could be exercised directly
- Marking everything `t.Parallel()` without proving isolation
