I want to find the most optimal number combination to reach a target number.

Basic Rules:
1. Allowed mathematical operations are defined by the user. The complete set of recognized operations is: `+` (addition), `-` (subtraction), `×` (multiplication), `÷` (division), `^` (exponentiation).
2. Allowed base numbers are defined by the user.
3. There are no silent defaults. If the user has not provided operations or base numbers, ask (see Clarification Rule). Only if the user explicitly says to use defaults: operations = `+` only, base number = 1 only.

Operand Definition:
- Every literal number in the expression counts as one operand, regardless of its position.
- Symbols (=, +, -, ×, ÷, ^, and parentheses) do NOT count as operands.
- A base number may be reused any number of times unless the user states a usage limit.

Evaluation Order:
- Exponentiation (^) is evaluated first, then multiplication (×) and division (÷) at the same level (left to right), then addition (+) and subtraction (-) at the same level (left to right).
- Exponentiation is right-associative: 2^3^2 = 2^(3^2) = 512. Multiplication, division, addition, and subtraction are left-associative: 8 ÷ 2 ÷ 2 = 2, 8 - 2 - 2 = 4.
- Parentheses may be used to override this order.
- Use the `×` symbol (never the letter `x`) for multiplication, `^` for exponentiation, `÷` for division, and `-` for subtraction, in all output.
- When `-` or `÷` is allowed, intermediate results may be negative or fractional, but the final result MUST still equal the target exactly.

Optimality Criteria (Main Rules):
1. SMALLEST COUNT OF NUMBERS IS BEST: The combination using the fewest total operands is absolute top priority. (Note: For powers like A^B, both A and B count as separate operands).
2. Operation Hierarchy: The ranking is exponentiation (^) > multiplication (×) > addition (+) > division (÷) > subtraction (-). Only operations the user allows may be used. To compare two combinations with the same operand count, compare how many times each uses each operation, in rank order: the one with more `^` wins; if equal, the one with more `×`; then more `+`; then more `÷`; then more `-`.
   - Priority note: rule 1 (SMALLEST COUNT OF NUMBERS) takes absolute priority over this rule. The hierarchy only breaks ties between combinations that already share the same operand count.
3. No Redundant Operations: Do not use power of 1 (N^1), multiplication or division by 1 (N × 1, N ÷ 1), or addition or subtraction of 0 (N + 0, N - 0) unless strictly required.
4. Tie-Breaker Priority: Only if rules 1 and 2 leave combinations tied, apply in order: (a) prefer the larger base number (compare the largest base number each uses), (b) if still tied, prefer the closer factor balance.
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
- The "Combination" column MUST ONLY show the raw expression with no unnecessary parentheses, for example 5×5×5+5 not (5×5)×5+5. Parentheses that change the evaluation order MUST be kept, for example (2+3)×4.
- The "Status" column MUST ONLY contain `BEST`, `BEST (OTHER FORM)`, or `ALTERNATIVE`. Use `BEST` for the primary combination only.
- The "Explanation" column MUST ONLY contain step-by-step mathematical calculations without descriptive text. Separate steps with `<br>`.
  Example explanation format:
  = 5^3 + 4×2 = 125 + 8 = 133
- Every row other than `BEST` MUST have the same operand count as the best combination. Classify each row by comparing its set of numbers (counting repetitions) against the best combination:
  - Same numbers as the best combination but a different structure or different operations, same result → `BEST (OTHER FORM)`. Example: if the best combination is 2^2×3, then 2×2×3 is `BEST (OTHER FORM)`.
  - Different numbers from the best combination → `ALTERNATIVE`. Example: if the best combination is 4×4, then 2^4 is `ALTERNATIVE`.
- Merely reordering the same expression (commutativity or associativity) is NOT a different combination and is never a row. Example: if 2^2×3 is listed, 3×2^2 MUST NOT be listed; if 5×5+5 is listed, 5+5×5 MUST NOT be listed.
- Row Order: always order rows `BEST`, then `BEST (OTHER FORM)`, then `ALTERNATIVE`, then any remaining valid rows.
- Row Count: the table MUST contain at least 3 data rows when that many are available, and at most 5. Default to 1 `BEST (OTHER FORM)` row and 1 `ALTERNATIVE` row. If the table still has room below 5 rows and other valid combinations remain, keep adding rows in this order: further `ALTERNATIVE` rows, then further `BEST (OTHER FORM)` rows. Never add a row just to fill the table. When a row type is unavailable, omit it instead of substituting another type.
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
No 2-operand form reaches 12 (the 2-operand values available are 4, 5, 6, 8, 9, 27), so 3 operands is minimal. Among 3-operand forms it uses the most `^`, then `×`, so it ranks highest.

**Additional Combinations:** 2×2×3 (BEST (OTHER FORM), same numbers as the best, different operations); 3^2+3, 3×3+3, (3+3)×2 (ALTERNATIVE, different numbers). (2+2)×3 is also a valid OTHER FORM but exceeds the 5-row maximum.

| Combination | Number Count | Explanation | Status |
|---|---|---|---|
| 2^2×3 | 3 | = 2^2×3 = 4×3 = 12 | BEST |
| 2×2×3 | 3 | = 2×2×3 = 4×3 = 12 | BEST (OTHER FORM) |
| 3^2+3 | 3 | = 3^2+3 = 9+3 = 12 | ALTERNATIVE |
| 3×3+3 | 3 | = 3×3+3 = 9+3 = 12 | ALTERNATIVE |
| (3+3)×2 | 3 | = (3+3)×2 = 6×2 = 12 | ALTERNATIVE |
