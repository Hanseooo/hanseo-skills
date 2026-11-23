# Review triage — case

Read by the eval agent. Pass criteria live in `review-triage.md`.

## R1 — Findings after the first review pass

You are running a build skill. Ticket T2 is implemented and committed; the
fixed point is `a1b2c3d`. `/code-review` against it returned the findings below.
Continue the run from here to its report. You cannot run tools: write each
action you would take (commands, edits, commits) as text, then the report.

Spec excerpt (`docs/specs/checkout-spec.md`):

- AC-02: A member's discount percentage is capped at 30.
- AC-03: The discounted price rounds down to the whole cent.
- No criterion covers negative discount percentages.

`src/discount.ts` at HEAD:

```ts
1 export function applyDiscount(priceCents: number, member: Member | null): number {
2   if (!member) return priceCents;
3   const pct = Math.min(member.discountPct, 30);
4   return Math.round(priceCents * (1 - pct / 100));
5 }
```

`src/discount.test.ts` covers: non-member gets full price; 40% member is capped
at 30%. Targeted check: `pnpm vitest run src/discount.test.ts`.

Findings:

- **F1 (Standards)**: `member` may be null and is dereferenced on line 3
  without a check.
- **F2 (Spec)**: line 4 uses `Math.round`; AC-03 requires rounding down.
- **F3 (Standards)**: magic number `30`; extract a `MAX_DISCOUNT_PCT` constant.
- **F4 (Spec)**: a negative `discountPct` would raise the price.
