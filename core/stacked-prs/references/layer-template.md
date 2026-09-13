# Implementation plan template

```markdown
# <Feature> implementation plan

Each merge point leaves the target branch healthy after declared predecessors have merged.

## Step 1: <responsibility>
- Scope: <modules / packages / files>
- Depends on: none
- Visible change: none / <behavior and why it is complete>
- Requirements / tests: <source identifiers or descriptive references>
- Verification: <executable commands or concrete steps>

## Step 2: <responsibility>
- Scope:
- Depends on: Step 1
- Visible change:
- Requirements / tests:
- Verification:
```

## Guidance

- Name each step by responsibility, such as "Order state model", rather than "Add types".
- Explain why visible changes are complete at each merge point. Rework the split if this cannot be justified.
- Give executable verification, such as `./gradlew :order:test`, rather than "tests pass".

## Splits to reconsider

| Split | Problem | Revision |
|-------|---------|----------|
| Half an API, then the rest | The first merge breaks an endpoint | Keep the endpoint complete; extract types and business rules first if useful |
| Migration before compatible code | The code does not support the data assumptions | Order compatible code and migration so every intermediate state works |
| All tests in the final step | Earlier merge points cannot be verified | Include tests with their target code |
| Fixed line-count chunks | Responsibilities are cut in half | Split by responsibility and reviewability |
