I want to find the most optimal number combination to reach a target number.

Basic Rules:
1. Allowed mathematical operations are defined by the user (default: addition (+)).
2. Allowed base numbers are defined by the user (default: one (1)).

Optimality Criteria (Main Rules):
1. SMALLEST COUNT OF NUMBERS IS BEST: The combination using the fewest total operands is absolute top priority. (Note: For powers like A^B, both A and B count as separate operands).
2. Operation Hierarchy: Prioritize exponentiation (^) first to grow values rapidly, followed by multiplication (×), then addition (+).
3. No Redundant Operations: Do not use power of 1 (N^1), multiplication by 1 (N × 1), or addition of 0 (N + 0) unless strictly required.
4. Tie-Breaker Priority: If multiple combinations share the exact same operand count, prioritize the option using larger base numbers or closer factor balance.
5. Exact Target Match: The calculated result must strictly equal the target number without exceeding or rounding.

Required Response Format:
1. Show allowed operations
2. Show allowed base numbers
3. Provide the best combination
4. Provide alternative best combinations
5. Comparison table (MAXIMUM 5 ROWS)

Table Format Rules:
- Columns MUST ONLY BE: Combination | Number Count | Explanation | Status
- The "Explanation" column MUST ONLY contain step-by-step mathematical calculations without descriptive text.
  Example explanation format:
  = 5^3 + 4x2
  = 125 + 8
  = 133

Clarification Rule:
If I have not explicitly mentioned the required parameters, ask me for clarification first before generating the answer:
- What is the target number?
- What is the allowed base number? (Ask if not provided)
- What operations are allowed? (Ask if not provided)
