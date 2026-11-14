# Codex Game Studios

<!-- codex-game-studios:begin -->

Project-specific studio instructions for LunarFriends. See the upstream template
in .codex/STUDIO.md.

<!-- codex-game-studios:end -->

A game-development studio with 49 specialist roles and 74 reusable workflows.
Use the expertise needed for the task; do not activate the whole studio for a
small change. This repository supports Unity, Godot, and Unreal Engine.

## Start with the project

1. Read `project.yaml`, relevant `project.local.yaml` overrides, and the latest
   `production/session-state/active.md` if it exists. Confirm facts against files.
2. Read `.codex/docs/codex-runtime.md` before the first studio workflow or
   delegation. It defines the kit's runtime behavior and takes precedence over
   examples that assume other tools or repeated approval prompts.
3. Inspect the actual engine project before changing settings. Unity's
   `ProjectSettings/ProjectVersion.txt`, Godot's `project.godot`, and Unreal's
   `.uproject` files are evidence; the template's example versions are not.
4. Resolve settings with `bash .codex/hooks/yaml-helper.sh resolve_config`
   from this project root. Commands in skills are steps to run, not automatic
   shell interpolation. A failed helper is not an empty project.

## Technology Stack

- **Engine**: Unity 6000.5.6f1
- **Language**: C#
- **Render Pipeline**: Universal Render Pipeline (URP) 17.5.0.
- **Version Control**: Git; preserve existing branch and release conventions.
- **Build System**: Unity Build Pipeline.
- **Asset Pipeline**: Unity Asset Import Pipeline.

## Engine Version Reference

<!-- ENGINE-REFERENCE-IMPORT: setup-engine updates the explicit read below. -->

Read `docs/engine-reference/unity/VERSION.md` before engine-specific work.
The bundled snapshot covers Unity 6.3 LTS. Verify the actual editor version
and current official API documentation for Unity 6000.5.6f1.

## Workflows and roles

- Skills live in `.agents/skills/<name>/SKILL.md`. Invoke them as `$start`,
  `$setup-engine`, `$brainstorm`, `$dev-story`, `$code-review`, or `$help`.
  `/skills` is Codex's skill browser; studio skills do not add slash commands.
- Native agents live in `.codex/agents/*.toml`; their editable role guides are
  `.codex/roles/*.md`. Run `python3 tools/studio.py sync-agents` after editing a
  role guide. Agent models inherit the user's selected model.
- Use available Codex subagent tools for independent tasks with distinct file
  ownership. Give each agent its objective, inputs, allowed paths, and expected
  output. Wait for dependencies before integration. The parent owns completion.
- If native subagents or a named role are unavailable, read the role guide and
  perform the work in this session. Report this plainly; never invent a tool or
  claim an independent review occurred.
- Read `.codex/docs/coordination-rules.md` for larger multi-domain work and
  `.codex/docs/directory-structure.md` for engine-specific paths.

## Authorization and scope

An explicit request to implement, fix, configure, or run a workflow authorizes
ordinary reversible work required to complete it. Do not ask again before each
file edit. Resolve routine implementation details and keep working. Ask only
when a material decision is unresolved, the requested scope must expand, or a
destructive/external action lacks authorization. Existing approval carries
forward. User instructions override template preferences.

`modes.automation` tunes collaboration, not Codex permissions. The shipped
project uses `guided`; `collaborative` is available for users who want draft
approval at meaningful milestones. Never infer permission to publish, message
others, spend money, or weaken sandbox/approval policy from an automation mode.
Prepare a concrete result before asking for a necessary final approval.

## Coding and validation

- Follow existing architecture and naming. Keep gameplay tuning data separate
  from implementation when useful, and use frame-independent time calculations.
- Keep normal C# statements on one line; wrap only lines that are too long to
  read comfortably. Preserve the project's established brace style.
- Put Unity code under `Assets/` or appropriate `Packages/`; Unreal module code
  under `Source/<Module>/`; Godot may use `src/`. Preserve an existing layout.
- Make validation proportional to the change. Test meaningful behavior and
  regressions; avoid tests that only mirror trivial implementation.
- A parse/build check is not a playtest. Player-visible game changes need a real
  engine run and retained observation where available. State precisely when an
  engine, device, or tool is absent and what was not tested.
- For framework changes, run `python3 tools/studio.py validate` and relevant
  tests under `tools/tests/`. `studio-tests/` evaluates the studio; game tests
  remain in their engine's proper test directories.

## Conditional rules

Markdown in `.codex/rules/` is reference material, not automatically matched
Codex policy. Explicitly read the matching rule before changing that area:

| Area                       | Read                                                                 |
| -------------------------- | -------------------------------------------------------------------- |
| Gameplay, movement, combat | `.codex/rules/gameplay-code.md`                                      |
| Engine/core systems        | `.codex/rules/engine-code.md`                                        |
| AI or networking           | `.codex/rules/ai-code.md` or `.codex/rules/network-code.md`          |
| UI or shaders              | `.codex/rules/ui-code.md` or `.codex/rules/shader-code.md`           |
| Design or narrative        | `.codex/rules/design-docs.md` or `.codex/rules/narrative.md`         |
| Data, scenes, resources    | `.codex/rules/data-files.md` or `.codex/rules/scene-files.md`        |
| Game tests, prototypes     | `.codex/rules/test-standards.md` or `.codex/rules/prototype-code.md` |
| Studio skills, role memory | `.codex/rules/skill-authoring.md` or `.codex/rules/agent-memory.md`  |

Native command policies use `.rules` files. Codex's sandbox and approvals are
the access controls; Markdown instructions and hooks are not full boundaries.

## State and handoff

Keep useful progress in `production/session-state/active.md` when session state
is enabled. After context loss, reread it and verify against actual files.
Read `.codex/docs/context-management.md` when a task needs a longer checkpoint.
Helpers in `.codex/scripts/` emit observations, never verdicts. Interpret their
findings with effective project settings. Finish with what changed, what was
verified, and concrete remaining limitations.

## Unity Project Development Standards

These rules complement the Codex Game Studios instructions above and apply to project-owned Unity code and assets.

### 1. SOLID and architecture

- Apply all five SOLID principles pragmatically.
- Keep each class focused on one clear responsibility.
- Prefer composition over unnecessary inheritance.
- Separate gameplay logic, networking, configuration, and presentation when appropriate.
- Keep dependencies explicit and avoid circular references.
- Use interfaces and abstractions only when there is a concrete need.
- Avoid overengineering, oversized classes, and unnecessary architectural layers.
- Keep gameplay parameters configurable when useful.

### 2. C# coding conventions

- Prioritize readable and maintainable code over brevity.
- Use four spaces for indentation.
- Put opening braces on their own lines.
- Always use braces with `if`, `else`, `for`, `foreach`, `while`, and similar statements.
- Avoid ternary operators (`?:`).
- Avoid null-conditional operators (`?.`).
- Avoid null-coalescing operators (`??` and `??=`).
- Avoid nullable syntax (`?`) when explicit validation is sufficient.
- Avoid expression-bodied methods (`=>`).
- Prefer explicit conditions, traditional methods, and descriptive variable names.
- Avoid unnecessarily complex LINQ expressions and lambdas.
- Keep ordinary statements on one line when readable.
- Never use pseudocode, incomplete implementations, or placeholder logic as a finished solution.
- Do not use accented characters in identifiers, code comments, or technical strings.
- User-facing text may contain proper Spanish accents.

### 3. Mandatory XML documentation

- Every C# method must have an XML `/// <summary>` comment.
- This includes public, private, protected, and internal methods.
- Constructors must also have XML documentation.
- Unity lifecycle methods such as `Awake()`, `Start()`, `Update()`, and `OnDestroy()` must be documented.
- Each summary must briefly explain the method's responsibility.
- Use one short, clear sentence.
- Avoid unnecessary repetition of the method name.
- Do not use accented characters in code comments.

### 4. Unity project organization

- Respect the existing Unity folder structure.
- If the project uses numbered top-level asset folders, preserve that convention.
- Organize new project-owned files into descriptive PascalCase subfolders.
- Keep scripts, scenes, prefabs, materials, textures, audio, and other assets in their corresponding categories.
- Avoid unnecessary folders and duplicate structures.
- Preserve Unity `.meta` files and asset GUIDs.
- Move or rename Unity assets safely, preserving existing references.
- Do not reorganize Unity-generated or third-party package directories.
- Do not modify unrelated assets when implementing a feature.

### 5. Unity implementation

- Keep MonoBehaviour components focused on specific responsibilities.
- Avoid placing unrelated game systems inside a single MonoBehaviour.
- Cache references when appropriate instead of repeatedly searching for objects.
- Use frame-independent calculations for time-dependent gameplay.
- Respect the actual Unity version and installed packages.
- Verify package APIs instead of inventing methods or relying on incompatible examples.
- Consult the project's GDD and technical documentation when available.
- Do not introduce mechanics or architectural changes that contradict approved requirements.

### 6. Workflow and verification

- Prefer small, focused, and verifiable changes.
- Avoid unrelated refactoring.
- Use existing skills and tools only when they materially help the task.
- Prefer one agent for straightforward tasks; delegate only when justified.
- Keep validation proportional to the implementation.
- Distinguish compilation checks from actual gameplay testing.
- Do not claim tests passed unless they were executed successfully.
- Preserve unrelated Git changes.
- Do not create commits, push changes, or change branches without authorization.
- Report implemented changes, performed validations, and remaining limitations.
