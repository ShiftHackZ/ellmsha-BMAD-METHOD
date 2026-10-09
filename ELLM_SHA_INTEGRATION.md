# eLLMsha BMAD integration

The upstream BMAD-METHOD repository remains the fork base. eLLMsha additions live in `ellmsha-skills/` and `ellmsha-reference/`, so upstream changes can be merged without replacing the integration layer.

`ellmsha-reference/` preserves the BMAD workflows used in eLLMsha v2. `ellmsha-skills/` exposes twelve self-contained memory workflows to eLLMsha's global skills catalog. Each installed skill includes its own optional templates and checklists, so the catalog can copy it independently. The old activation scripts and filesystem artifact discovery are not required.

## Artifact storage

The runtime supplies a stable `BMAD artifact prefix: bmad/<project-scope>/`. A child agent receives that prefix and the relevant artifact paths. If context does not include it, `tdai_context` resolves `bmad_prefix` from the absolute `workspace`; skills never derive scope independently from a display name.

Workflows list and read existing TencentDB L2 scenario documents, write full Markdown plus a searchable summary, then read the document back before claiming success. Related skills agree on `prd.md`, `architecture.md`, `epics-and-stories.md`, `sprint-status.md` and `stories/<story-id>.md`. Updates preserve IDs and progress. Source code edits still happen in the project repository; workflow artifacts do not.

The desktop startup imports these skill packs through `/api/skills/import`, which refreshes existing installed copies. AionUi's slash catalog merges selected global skills with native agent commands. The fork's `GuidPage` preserves the global skill selection while assistant details load.

## Acceptance scenario

Use an isolated empty test workspace to avoid changing a real project's plan.

1. Open a new Codex chat and invoke `/bmad-prd`: ask for a small fast-path PRD with a distinctive requirement, such as an offline inventory export in CSV. Confirm the artifact is written and read back under the injected prefix.
2. Open a fresh chat on the same workspace. Ask `/bmad-create-epics-and-stories` to continue from the existing PRD without pasting its content or path. Confirm it reads the original artifact and retains the distinctive requirement.
3. Ask a delegated agent to read one artifact and create a review at a separate path. Confirm the parent reads that saved review and keeps the same project prefix.
4. Verify no `_bmad-output`, `project-context.md`, sprint file or PRD appeared in the workspace. Inspect the TencentDB documents and summaries. Repeat a document update and confirm the path is reused.

Import/visibility tests do not prove model execution. Report the actual inference outcome separately when running this scenario.
