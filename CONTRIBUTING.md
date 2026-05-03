# Contributing

Thank you for your interest in contributing! Please read this guide before opening a pull request.

---

## Code of Conduct

This project follows the [Contributor Covenant Code of Conduct](https://github.com/daiso-tech/.github/blob/main/CODE_OF_CONDUCT.md). By participating, you are expected to uphold it. Please report unacceptable behavior to the project maintainers via a [GitHub Issue](https://github.com/daiso-tech/daiso-core/issues).

---

## Getting Started

**Prerequisites:**

- Node.js (see `.nvmrc`)
- Docker (required for Testcontainers-based tests)

**Setup:**

```bash
git clone https://github.com/daiso-tech/daiso-core.git
cd daiso-core
npm ci
```

> [!IMPORTANT] > **Note:** This project uses **npm** as its package manager. Do not use `yarn` or `pnpm`.

---

## Local Development Commands

| Command                 | Description                                                                                      |
| ----------------------- | ------------------------------------------------------------------------------------------------ |
| `npx changeset`         | For bumping version for relase is done via [changeset](https://github.com/changesets/changesets) |
| `npm run commit`        | Creates conventional commit via [commitizen](https://commitizen-tools.github.io/commitizen/)     |
| `npm run check`         | Type-check with TypeScript                                                                       |
| `npm run lint`          | Lint with ESLint                                                                                 |
| `npm run lint:fix`      | Lint and auto-fix                                                                                |
| `npm run format`        | Check formatting with Prettier                                                                   |
| `npm run format:fix`    | Auto-fix formatting                                                                              |
| `npm run test`          | Run all tests                                                                                    |
| `npm run check:all`     | Run all checks (type, lint, format, test, exports)                                               |
| `npm run check:all:fix` | Run all checks and auto-fix where possible                                                       |

The following `:changed` variants only run against files that have changed compared to the `main` branch, significantly reducing feedback time during local development:

| Command                         | Description                                       |
| ------------------------------- | ------------------------------------------------- |
| `npm run lint:changed`          | Lint only changed files                           |
| `npm run lint:fix:changed`      | Lint and auto-fix only changed files              |
| `npm run format:changed`        | Check formatting of only changed files            |
| `npm run format:fix:changed`    | Auto-fix formatting of only changed files         |
| `npm run test:changed`          | Run tests related to only changed files           |
| `npm run check:all:changed`     | Run all checks on only changed files              |
| `npm run check:all:fix:changed` | Run all checks and auto-fix on only changed files |

---

## Branch Naming Convention

Branch names must start with one of:

`feature/` · `enhancement/` · `bug/` · `fix/` · `refactor/` · `docs/` · `chore/`

Examples: `feature/add-redis-cache`, `fix/event-bus-memory-leak`, `docs/update-readme`

---

## Commit Messages

Use [Conventional Commits](https://www.conventionalcommits.org/) format:

```
<type>(<scope>): <short description>
```

Examples: `feat(cache): add Redis adapter`, `fix(event-bus): handle reconnection`, `docs: update contributing guide`

This project uses **[Commitizen](https://commitizen-tools.github.io/commitizen/)** to help write correctly formatted commits. Run the commit script instead of `git commit` directly:

```bash
npm run commit
```

This will launch an interactive prompt that guides you through the Conventional Commits format.

---

## Versioning & Changesets

This project uses **[@changesets/cli](https://github.com/changesets/changesets)** to manage versioning and changelogs and follows **[Semantic Versioning (semver)](https://semver.org/)**. If your PR changes public-facing behavior, add a changeset:

```bash
npx changeset
```

Follow the interactive prompts to select the affected packages and describe the change. Commit the generated `.changeset/*.md` file alongside your code changes.

> [!IMPORTANT]
> Contributors may only bump **patch** (`0.0.x`) or **minor** (`0.x.0`) versions. **Major** (`x.0.0`) version bumps are reserved for maintainers only.

---

## Pull Request Process

1. Before opening a PR, run `npm run check:all:fix:changed` to check and auto-fix only changed files. Running the full `check:all` suite locally is not required — it can take up to 28 minutes.
2. Keep PRs focused — one concern per PR.
3. Include a clear description of what changed and why.
4. Link any related issues.

---

## File and Folder Naming Convention

- All files and folders must be **kebab-case** (e.g., `my-file.ts`, `my-folder/`).
- An optional `_` prefix is allowed for internal files (e.g., `_internal-helper.ts`).

This avoids naming conflicts across case-sensitive (Linux) and case-insensitive (macOS, Windows) filesystems.
File names are enforced via `ls-lint` in CI and `lint-staged` on commit.

---

## CI / GitHub Actions

Every pull request automatically runs:

| Check                              | Tool                                         |
| ---------------------------------- | -------------------------------------------- |
| Linting                            | ESLint                                       |
| Testing                            | Vitest + Testcontainers                      |
| Type checking                      | TypeScript                                   |
| Formatting                         | Prettier (Markdown docs & JSON config files) |
| `package.json` exports field       | publint                                      |
| Entrypoint files listed in exports | custom script                                |
| Module exports files               | custom script                                |
| File name linting                  | ls-lint                                      |

All checks must pass before a PR can be merged.

---

## Code Quality

- **[ESLint SonarJS](https://github.com/SonarSource/eslint-plugin-sonarjs)** — detects code smells and enforces quality standards.
- **[eslint-plugin-boundaries](https://github.com/javierbrea/eslint-plugin-boundaries)** — enforces consistent folder structure and internal module dependency rules.
- **Prettier** — formats Markdown docs and JSON config files. Prettier is also configured via ESLint for source files, meaning a single `npm run lint` command handles both linting and source code formatting.

### VS Code Setup

The project includes a `.vscode/settings.json` that configures ESLint and Prettier for VS Code automatically. To take advantage of it, install the following extensions:

- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

---

## TypeScript Conventions

- Prefer `type` over `interface` for all type definitions.
- Use `interface` only when TypeScript emits circular dependency errors that cannot be resolved with `type`.
- Avoid heavy use of the `infer` keyword. Overuse of `infer` breaks TypeScript LSP features such as go-to-definition, find-all-references, and hover types, and adds unnecessary maintenance burden. Prefer explicit type definitions instead. If `infer` is necessary, add Vitest type tests to verify the inferred types behave correctly.

---

## Reporting Bugs & Requesting Features

Please open a [GitHub Issue](https://github.com/daiso-tech/daiso-core/issues) with:

- A clear title and description.
- Steps to reproduce (for bugs).
- Expected vs actual behavior (for bugs).
- Use case and motivation (for features).

---

## Architecture Overview

The library is primarily designed with **majestic monoliths** in mind — large, well-structured single-deployment applications. Because it is built on hexagonal architecture, it could theoretically be adapted for microservices in the future, but that is not its primary purpose or design goal.

The library is built around a few core principles:

### Vertical Slice Architecture

The codebase is organized by **component** (e.g. `src/cache/`, `src/event-bus/`). Within each component, code is further organized into layers:

- **contracts** — TypeScript types and interfaces defining the component's public API
- **adapters** — Concrete implementations of the contracts
- **derivables** — Higher-level abstractions derived from the contracts
- **middlewares** — Agnostic middleware functions for cross-cutting concerns; they are not tied to any specific component and can be applied to any function or method
- **plugins** — Reusable plugins built on top of middlewares
- **test-utilities** — Shared test helpers and reusable behavior test suites

### Hexagonal Architecture (Adapter Pattern)

Key components (cache, event bus, file storage, execution context, etc.) are defined as **TypeScript types**. Multiple concrete implementations can be swapped in without changing any core logic, enabling seamless integration with different backends and frameworks.

### Interface Segregation

A component is split into smaller, focused sub-types that each define a subset of the full contract. The sub-types are composed via extension to form the full type (e.g. `IReadableCache` defines read operations, and `ICache` extends `IReadableCache` to add write operations). A single concrete implementation satisfies all of them.

### Inversion of Control

Components depend on the **contracts (interfaces)** of other components, not their concrete implementations. This promotes testability, modularity, and flexible dependency wiring.

### Aspect-Oriented Programming

The library provides **agnostic middleware functions** that can be applied to any function or method. These middlewares act as plugins to implement cross-cutting concerns (retries, timeouts, caching, circuit breaking, etc.) without modifying core logic.

### Testing Approach

Majority of tests target **adapter behaviors** directly via shared, reusable test suites. This means the same tests can be run against any adapter implementation, ensuring all adapters conform to the expected contract.

Tests live **alongside the implementation code** rather than in a separate top-level `tests/` folder. This keeps related code and tests co-located for better cohesion and discoverability.

> [!NOTE]
> Some tests are inherently flaky because expiration behavior is tested against real time rather than mocked time. This is necessary for adapters that run outside of the JS runtime (e.g., MongoDB, Redis), where time mocking is not possible. If up to 10 tests fail because of expiration on your machine, this is acceptable — go ahead and open the PR for code review.

---

## Adding a New Component

When adding a new component (e.g. a new module like a job-scheduler), follow this process:

### 1. Design the contracts first

Before writing any implementation, define the **public API contracts** — both the component's own contract and the adapter contract(s) it relies on. These should be:

- **Well thought out** — consider the full range of use cases, edge cases, and how the component will compose with the rest of the library.
- **Well documented** — every type, method signature, and parameter must have TSDoc comments in the source code explaining its purpose, behavior, and any constraints.
- **Human-authored** — contracts must not be directly AI-generated. AI can be used for inspiration or as a sounding board, but the final design must reflect deliberate human judgment and understanding of the library's principles.

Investing time here pays off significantly — contracts that are unclear or poorly designed tend to cause rework in every adapter that implements them.

### 2. Implement concrete adapters

Once the contracts are stable enough, begin implementing concrete adapters. Some **iteration on the contracts is expected and healthy** at this stage — implementing contracts often reveals gaps or awkward constraints in the original design. Revisit and refine the contracts as needed.

### 3. Follow the vertical slice structure

Place all component code under `src/<component-name>/` and follow the established internal layer structure (contracts, adapters, derivables, middlewares, plugins, test-utilities).
