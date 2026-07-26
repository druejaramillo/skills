# Refactor Candidates

After the agreed tests are green, look for behavior-preserving changes only:

- **Duplication** → Extract function/class
- **Long methods** → Break into private helpers (keep tests on public interface)
- **Shallow modules** → Combine or deepen
- **Feature envy** → Move logic to where data lives
- **Primitive obsession** → Introduce value objects
- **Existing code** the new code reveals as problematic

If a candidate changes public behavior, an agreed seam, or a compatibility contract, it is not a refactor. Record it as a separate proposal and return to planning. Do not fold broad review feedback into the current red-green-refactor slice.
