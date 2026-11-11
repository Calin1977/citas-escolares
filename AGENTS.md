# AGENTS.md

## Project state

Greenfield repository. There is **no application code, manifest, build, test, or lint setup yet** — do not invent commands or assume a stack. The first `/spec` decides those; record them here once they exist.

The repository is **not a git repo yet**. Run `git init` before `/spec-impl`, which creates and switches to a `spec-NN-slug` branch.

## Spec-driven workflow

This repo is driven by two installed skills. Do not write feature code outside this flow.

1. **`/spec <feature>`** — clarifies requirements, then writes `specs/NN-slug.md` with `**Status:** Draft`. Never writes code. Start numbering at `01-`.
2. Human reviews the spec and changes its status to `Approved` (`Aprobado` and other-language equivalents also count).
3. **`/spec-impl NN-slug`** — refuses unless the status means Approved, creates branch `spec-NN-slug`, then implements the plan one step at a time with pauses for diff review.

Hard rules that are easy to violate:

- Never auto-commit during `/spec-impl`; the user commits.
- Implement exactly what the spec says. Out-of-scope requests get noted for a future spec, not slipped in.
- Stop and ask on ambiguity; do not improvise.
- A spec is done when its acceptance criteria pass, then the human sets status to `Implemented`.

Branch creation is controlled by `AutoCreateBranch` in `specs/.spec-config.yml` (default `true`; `/spec` seeds it). Set to `false` to require a `[y/N]` confirmation.

Typical sequence: `/spec` → human approves → `/spec-impl` → **`@spec-verifier`** → human sets status to `Implemented`.

Specs follow the structure in `.agents/skills/spec/template.md` (header blockquote, scope with explicit "out", data model, numbered plan, boolean acceptance criteria, decisions, optional risks). Keep one language per repo, consistently.

## Skills

`.agents/skills/{spec,spec-impl}/` are installed from `Klerith/fernando-skills` and pinned by hash in `skills-lock.json`. Do not hand-edit `SKILL.md`; update via the installer so the lockfile stays valid.

## Agents

**`@spec-verifier`** (`.opencode/agents/spec-verifier.md`) — custom project agent that verifies a spec's "Acceptance criteria". Invoke it with the spec path (e.g. `@spec-verifier specs/01-algo.md`). Behavior:

- Verifies each criterion with real evidence: Context7 for framework/Next.js recommendations, Playwright MCP for screens (screenshots in `.playwright-mcp/`, compared visually with a vision-capable model).
- Marks `- [ ]` → `- [x]` only with verified evidence; corrects in-scope failures, reports out-of-scope ones.
- May edit only `specs/**`; never changes the spec's **Status** (that is the human's job) and never commits.
- No fixed model: it inherits the session model — select a vision-capable model (currently `opencode/nemotron-3.5-super-vl-free`) when verifying screens.

## Naming

`citas-escolares` = school appointments. Keep domain terms in Spanish if the user uses them, and mirror the language of existing specs.

## MCPs

- Playwright Screenshots y cualquier objeto relacionado con Playwright tiene que estar en la carpeta .playwright-mcp.
- Context7 Usaremos este MCP para traer información o documentación actualizada del framework.

## Reglas de Código

- Usar código límpio, nombres, variables, funciones y demás en inglés.