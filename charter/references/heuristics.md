# Architecture heuristics

Defaults, not rules. Recommend each one; if the user pushes back, record their
choice and move on — a recorded disagreement is a successful outcome. Sources are
cited so users can evaluate the defaults themselves.

## Abstraction only at volatility points
A seam exists because something behind it is expected to change — the
model-provider wrapper is the canonical example. When proposing any layer, name
the volatility that justifies it. No stated volatility → no layer.
(Parnas, "On the Criteria to Be Used in Decomposing Systems into Modules," 1972:
modules should hide the decisions most likely to change.)

## Contract-first boundaries
Modules communicate through typed schemas or interfaces, never ad-hoc dicts or
implicit shapes. The contract is also the test surface: validate at every
boundary crossing. (Meyer, design by contract, *Object-Oriented Software
Construction*, 1988.)

## Deep modules
A good module has a simple interface hiding substantial implementation. Deletion
test for shallow ones: if removing the module and inlining its code costs
nothing, remove it. (Ousterhout, *A Philosophy of Software Design*, 2018.)

## Wrappers and adapters
- Hide: vendor SDKs, auth, retries, pagination, rate limits, vendor types.
- Expose: domain-shaped operations and domain types only.
- One adapter behind an interface = a hypothetical seam (fine if volatility is
  stated); two adapters = a real seam. Don't design for a third that nobody named.
(Cockburn, hexagonal architecture / ports & adapters, 2005.)

## Agent navigability
Agents build maintainably when the map is predictable:
- One home per artifact type — never two folders doing the same job.
- One file per concern; a file passing ~300 lines or mixing concerns is a split
  signal, not a hard limit.
- Names match the domain vocabulary the docs use.

## When NOT to abstract
- No interface with one implementation absent stated volatility.
- No config for a value that never changes.
- No speculative flexibility — needs that arrive can scaffold for themselves.

## ADR practice
Record only genuinely locked decisions — choices someone could plausibly undo by
accident. Formula: context / decision / consequences / alternatives considered /
escape hatch. The escape hatch ("write a new ADR and flag a human — do not
implement first") is the load-bearing feature: it converts silent drift into a
visible decision. (Nygard, "Documenting Architecture Decisions," 2011.)
