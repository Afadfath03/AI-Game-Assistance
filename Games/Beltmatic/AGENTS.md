I want to find the most optimal number combination to reach a target number.

Basic Rules:
1. Allowed mathematical operations are defined by the user (default: addition (+)). The complete set of recognized operations is: `+` (addition), `-` (subtraction), `×` (multiplication), `÷` (division), `^` (exponentiation).
2. Allowed base numbers are defined by the user (default: one (1)).

Operand Definition:
- Every literal number in the expression counts as one operand, regardless of its position.
- Symbols (=, +, -, ×, ÷, ^, and parentheses) do NOT count as operands.
- A base number may be reused any number of times unless the user states a usage limit.
- Optimality Criteria rule 1 (SMALLEST COUNT OF NUMBERS) takes absolute priority over rule 2 (Operation Hierarchy). The hierarchy only breaks ties between combinations that already share the same operand count.

Evaluation Order:
- Exponentiation (^) is evaluated first, then multiplication (×) and division (÷) at the same level (left to right), then addition (+) and subtraction (-) at the same level (left to right).
- Exponentiation is right-associative: 2^3^2 = 2^(3^2) = 512. Multiplication, division, addition, and subtraction are left-associative: 8 ÷ 2 ÷ 2 = 2, 8 - 2 - 2 = 4.
- Parentheses may be used to override this order.
- Use the `×` symbol (never the letter `x`) for multiplication, `^` for exponentiation, `÷` for division, and `-` for subtraction, in all output.
- When `-` or `÷` is allowed, intermediate results may be negative or fractional, but the final result MUST still equal the target exactly.

Optimality Criteria (Main Rules):
1. SMALLEST COUNT OF NUMBERS IS BEST: The combination using the fewest total operands is absolute top priority. (Note: For powers like A^B, both A and B count as separate operands).
2. Operation Hierarchy: Prioritize in this order: exponentiation (^) to grow values rapidly, then multiplication (×), then addition (+), then division (÷), then subtraction (-). Only operations the user allows may be used.
3. No Redundant Operations: Do not use power of 1 (N^1), multiplication or division by 1 (N × 1, N ÷ 1), or addition or subtraction of 0 (N + 0, N - 0) unless strictly required.
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
4. Provide additional combinations: BEST (OTHER FORM) and ALTERNATIVE
5. Comparison table (MINIMUM 3 DATA ROWS when available, MAXIMUM 5 DATA ROWS, excluding the header)

Table Format Rules:
- Columns MUST ONLY BE: Combination | Number Count | Explanation | Status
- The "Combination" column MUST ONLY shown raw value, for example 5×5×5+5 not (5×5)×5+5
- The "Status" column MUST ONLY contain `BEST`, `BEST (OTHER FORM)`, or `ALTERNATIVE`. Use `BEST` for the primary combination only.
- The "Explanation" column MUST ONLY contain step-by-step mathematical calculations without descriptive text. Separate steps with `<br>`.
  Example explanation format:
  = 5^3 + 4×2 = 125 + 8 = 133
- Every row other than `BEST` MUST have the same operand count as the best combination. Classify each row by comparing its set of numbers (counting repetitions) against the best combination:
  - Same numbers as the best combination, only written or ordered differently, same result → `BEST (OTHER FORM)`. Example: if the best combination is 5×5+5, then 5+5×5 is `BEST (OTHER FORM)`, not `ALTERNATIVE`.
  - Different numbers from the best combination → `ALTERNATIVE`. Example: if the best combination is 4×4, then 2^4 is `ALTERNATIVE`.
- Row Order: always order rows `BEST`, then `BEST (OTHER FORM)`, then `ALTERNATIVE`, then any remaining valid rows.
- Row Count: the table MUST contain at least 3 data rows when that many are available, and at most 5. Default to 1 `BEST (OTHER FORM)` row and 1 `ALTERNATIVE` row. If the table still has room below 5 rows and other valid combinations remain, keep adding rows in this order: further `ALTERNATIVE` rows, then further `BEST (OTHER FORM)` rows. Never add a row just to fill the table. When a row type is unavailable, omit it instead of substituting another type.
- Two rows MUST NOT be rearrangements of each other by commutativity or associativity (for example, 5×5+5 and 5+5×5 can never both appear as separate rows).
- If no additional row can be listed at all, state: "No other combination with the same operand count."

Clarification Rule:
If I have not explicitly mentioned the required parameters, ask me for clarification first before generating the answer:
- What is the target number?
- What is the allowed base number? (Ask if not provided)
- What operations are allowed? (Ask if not provided)
- Does the allowed base number range also restrict the exponent in a power such as A^B? (Ask only if the answer uses an exponent)

Worked Example (target = 12, base numbers = {2, 3}, operations = {+, ×, ^}):

**Allowed Operations:** + (addition), × (multiplication), ^ (exponentiation)
**Allowed Base Numbers:** 2, 3

**Best Combination:** 2^2×3
No 2-operand form reaches 12 (the 2-operand values available are 4, 5, 6, 8, 9, 27), so 3 operands is minimal. It also uses the highest-ranked allowed operations (^ then ×).

**Additional Combinations:** 3×2^2 (BEST (OTHER FORM), same numbers as the best), 3^2+3 (ALTERNATIVE, different numbers)

| Combination | Number Count | Explanation | Status |
|---|---|---|---|
| 2^2×3 | 3 | = 2^2×3<br>= 4×3<br>= 12 | BEST |
| 3×2^2 | 3 | = 3×2^2<br>= 3×4<br>= 12 | BEST (OTHER FORM) |
| 3^2+3 | 3 | = 3^2+3<br>= 9+3<br>= 12 | ALTERNATIVE |
