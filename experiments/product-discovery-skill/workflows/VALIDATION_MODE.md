# Validation Mode

## Principle

Validation effort should scale with the cost of being wrong.

A useful mental model:

`validation intensity ∝ build cost × irreversibility × operational risk × uncertainty`

Do not calculate this mechanically. Use it to choose the appropriate experiment.

## Step 1: Identify the dominant risk

Classify the biggest uncertainty:
- **value risk** — users may not care;
- **payment risk** — users may care but not pay;
- **distribution risk** — customers may be expensive/impossible to reach;
- **technical risk** — the core behavior may not work reliably;
- **platform risk** — vendor/native features may erase the opportunity;
- **retention risk** — need may be too infrequent or weak;
- **trust/risk** — users may not permit the product to act on sensitive/high-stakes data.

## Step 2: Choose the cheapest experiment that hits that risk

Possible experiments:
- ship a tiny complete utility;
- interactive prototype;
- demo video;
- fake-door/landing-page test;
- free scanner/checker with paid artifact boundary;
- manual/concierge fulfillment;
- plugin/extension prototype;
- marketplace listing test;
- paid-ad smoke test;
- community post with real call-to-action;
- usability sessions;
- interviews anchored in recent behavior;
- technical spike;
- one integration only;
- pre-order or paid beta.

## Cheap product rule

If a meaningful product can be built and shipped in a few days with negligible downside, shipping can itself be the validation experiment.

Do not add ceremony that costs more than failure.

## Timing override

For a short opportunity window:
1. perform a lightweight duplicate/policy sanity check;
2. cap build time and spend;
3. ship the smallest compelling version;
4. exploit momentum immediately if it works;
5. stop if the window closes.

## Expensive product rule

For months-long, regulated, infrastructure-heavy, or high-support products, require stronger evidence before implementation.

## Success and kill criteria

Every meaningful validation experiment should define:
- signal that justifies the next investment;
- signal that should stop or narrow the thesis;
- maximum time/money budget before reevaluation.

Avoid vanity metrics when the actual uncertainty is willingness to pay or repeated behavior.
