I want to find the most optimal number combination to reach a target number.

Basic Rules:
1. Allowed mathematical operations are defined by the user (default: addition (+)).
2. Allowed base numbers are defined by the user (default: one (1)).

Operand Definition:
- Every literal number in the expression counts as one operand, regardless of its position.
- Symbols (=, +, ×, ^, and parentheses) do NOT count as operands.
- A base number may be reused any number of times unless the user states a usage limit.
- Optimality Criteria rule 1 (SMALLEST COUNT OF NUMBERS) takes absolute priority over rule 2 (Operation Hierarchy). The hierarchy only breaks ties between combinations that already share the same operand count.

Evaluation Order:
- Exponentiation (^) is evaluated first, then multiplication (×), then addition (+).
- Exponentiation is right-associative: 2^3^2 = 2^(3^2) = 512.
- Parentheses may be used to override this order.
- Use the `×` symbol (never the letter `x`) for multiplication, and `^` for exponentiation, in all output.

Optimality Criteria (Main Rules):
1. SMALLEST COUNT OF NUMBERS IS BEST: The combination using the fewest total operands is absolute top priority. (Note: For powers like A^B, both A and B count as separate operands).
2. Operation Hierarchy: Prioritize exponentiation (^) first to grow values rapidly, followed by multiplication (×), then addition (+).
3. No Redundant Operations: Do not use power of 1 (N^1), multiplication by 1 (N × 1), or addition of 0 (N + 0) unless strictly required.
4. Tie-Breaker Priority: If multiple combinations share the exact same operand count, apply in order: (a) prefer the larger base number, (b) if still tied, prefer the closer factor balance.
5. Exact Target Match: The calculated result must strictly equal the target number without exceeding or rounding.
6. No Solution Rule: If no valid combination reaches the target, say so explicitly and name which operation or base number is missing. NEVER invent, round, or approximate a combination to fit the target. Then offer the closest reachable value as a reference.

Verification Rule:
- Show the final evaluated value on its own line inside the Explanation column.
- If the best combination uses 2 or more operands, add one line confirming that no shorter form reaches the target.
- If the best combination uses exactly 1 operand, no such confirmation is needed.

Required Response Format:
1. Show allowed operations
2. Show allowed base numbers
3. Provide the best combination
4. Provide alternative best combinations
5. Comparison table (MAXIMUM 5 DATA ROWS, excluding the header)

Table Format Rules:
- Columns MUST ONLY BE: Combination | Number Count | Explanation | Status
- The "Status" column MUST ONLY contain `BEST` or `ALTERNATIVE`.
- The "Explanation" column MUST ONLY contain step-by-step mathematical calculations without descriptive text. Separate steps with `<br>`.
  Example explanation format:
  = 5^3 + 4×2
  = 125 + 8
  = 133
- Alternative combinations MUST have the same operand count as the best combination and MUST be syntactically distinct from it and from each other (a different written expression is required, even if it evaluates to the same value). If no such alternative exists, state: "No alternative with the same operand count."

Clarification Rule:
If I have not explicitly mentioned the required parameters, ask me for clarification first before generating the answer:
- What is the target number?
- What is the allowed base number? (Ask if not provided)
- What operations are allowed? (Ask if not provided)
- Does the allowed base number range also restrict the exponent in a power such as A^B? (Ask only if the answer uses an exponent)

Worked Example (target = 16, base numbers = {2, 4}, operations = {^, ×}):

**Allowed Operations:** ^ (exponentiation), × (multiplication)
**Allowed Base Numbers:** 2, 4

**Best Combination:** 4×4
No 1-operand form reaches 16 (2 and 4 are the only single operands), so 2 operands is minimal.

**Alternative Best Combinations:** 2^4, 4^2

| Combination | Number Count | Explanation | Status |
|---|---|---|---|
| 4×4 | 2 | = 4×4<br>= 16 | BEST |
| 2^4 | 2 | = 2^4<br>= 16 | ALTERNATIVE |
| 4^2 | 2 | = 4^2<br>= 16 | ALTERNATIVE |
