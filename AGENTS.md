# Codex Project Instructions

## Project Mission

Build a reproducible research system for AI-assisted investment decisions.
The system may open a new paper position only when at least 95% of all
configured voters approve and every deterministic risk gate passes.

The authoritative product and research plan is:

- `docs/IMPLEMENTATION_PLAN.md`

Read that document before planning or implementing substantial work.

## Current Stage

The repository is in the design stage. Implement the phases in order:

1. Specification and schemas
2. Reproducible data snapshots
3. Independent multi-model voting
4. Consensus and deterministic risk gates
5. Paper trading and reports
6. Forward validation

Do not add live brokerage execution until the paper-trading and validation
requirements in the plan are met and the user explicitly requests it.

## Non-Negotiable Rules

- Default to paper trading. Never imply that returns are guaranteed.
- Use 20 configured voters for the MVP unless the plan is deliberately amended.
- An entry requires at least 19 approvals out of all 20 configured voters.
- Divide by configured voter count, not successful response count.
- Timeout, malformed output, missing response, and abstention are not approvals.
- Never rerun a vote merely to obtain a preferred decision.
- Keep votes independent until all initial votes have been recorded.
- Do not manufacture diversity by presenting one model as many independent models.
- A deterministic risk veto always overrides AI approval.
- Stop-losses, emergency exits, and actions that reduce risk must not wait for
  95% approval.
- Do not use data published after a decision timestamp.
- Do not simulate an order at a price that was already known only after the
  decision. Prefer the next tradable price and model costs conservatively.
- Record NO TRADE and RISK VETO outcomes, not only executed trades.
- Preserve raw model responses alongside normalized structured output.
- Never commit API keys, brokerage credentials, account identifiers, or secrets.

## Implementation Guidance

- Prefer Python with typed data models and explicit JSON Schema or equivalent.
- Keep market-data, screening, voting, consensus, risk, execution, and reporting
  modules separate.
- Make model providers replaceable behind a common interface.
- Make prompts, models, thresholds, costs, and risk limits versioned config.
- Use deterministic fixtures and provider mocks in tests. Unit tests must not
  require paid APIs or live market data.
- Add tests before or with consensus and risk behavior, especially:
  `18/20`, `19/20`, `20/20`, abstention, timeout, malformed output, duplicate
  vote, stale data, risk veto, and emergency exit.
- Every decision artifact should include a decision ID, as-of timestamp, config
  version, prompt version, model identity, code commit, and input hash.
- Prefer append-only run artifacts. Corrections should be traceable rather than
  silently overwriting prior decisions.

## Research Discipline

- Run 95% consensus and comparison cohorts from the same frozen inputs.
- At minimum compare single-model, simple majority, 75%, 95%, unanimity,
  rules-only, and buy-and-hold behavior.
- Include fees and slippage in reported results.
- Report sample size, drawdown, and uncertainty, not return alone.
- Treat small samples and correlated model errors as unresolved evidence.
- Preserve losing trades and failed runs.

## Scope Control

Before implementing a requested feature, check whether it changes:

- the definition of a voter;
- the 95% denominator;
- independence between votes;
- the point-in-time data contract;
- risk-veto precedence;
- paper versus live execution; or
- the comparability of experiment cohorts.

If it does, update `docs/IMPLEMENTATION_PLAN.md`, tests, and configuration
together so the research contract remains explicit.
