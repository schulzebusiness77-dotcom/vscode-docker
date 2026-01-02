# Copilot instructions — vscode-docker

This repository is the VS Code Docker extension (see `package.json` engines and metadata). Below are focused, actionable notes to help an AI coding agent be immediately productive in this codebase.

- **Big picture:** This is a VS Code extension repo with Docker-centric assets. Key artifacts live at the repository root: `Dockerfile`, `compose.yaml`, `compose.debug.yaml`, and VS Code task definitions in `.vscode/tasks.json`.
- **Runtime / tooling:** The project targets VS Code (see [package.json](package.json#L1-L40)) and integrates with Docker and the `ms-azuretools.vscode-containers` extension.

Workflows
- **Build container (VS Code):** run the `docker-build` task defined in [.vscode/tasks.json](.vscode/tasks.json#L1-L20). CLI equivalent: `docker build -f Dockerfile -t vscode-docker .` (inspect [Dockerfile](Dockerfile#L1)).
- **Run (release):** `docker-run: release` depends on `docker-build` and runs the built image (see [.vscode/tasks.json](.vscode/tasks.json#L1-L40)).
- **Run (debug):** `docker-run: debug` sets `DEBUG=*` and `NODE_ENV=development` and enables Node debugging via the task (see [.vscode/tasks.json](.vscode/tasks.json#L20-L40)). Use this task for iterative debugging.
- **Package extension:** packaging uses `vsce` — run `npm run package` or `vsce package` (see [package.json](package.json#L1-L40)).

Project-specific conventions and patterns
- Root-level docker/compose files are the single source of container-related configuration: [Dockerfile](Dockerfile#L1), [compose.yaml](compose.yaml#L1), [compose.debug.yaml](compose.debug.yaml#L1).
- Documentation and walkthroughs live under `resources/readme` and `resources/walkthroughs` (see [resources/walkthroughs/empty.md](resources/walkthroughs/empty.md)).
- `package.json` contains extension metadata and a `package` script. Note: `build`, `lint`, and `test` scripts are currently empty — CI steps may be external.

Integration points and external dependencies
- Requires Docker daemon for the VS Code tasks to succeed.
- Developer packaging depends on `@vscode/vsce` (devDependency) and the `ms-azuretools.vscode-containers` extension (see [package.json](package.json#L1-L40)).

Guidance for code changes
- When changing build or run behavior, update both the `Dockerfile` and `.vscode/tasks.json` to keep the VS Code tasks accurate.
- When changing extension metadata or dependencies, update `package.json` and the `README.md` to reflect new install/build instructions.
- Prefer minimal changes to global scripts since many scripts are intentionally empty; add scripts only if you also update README/automation that expects them.

Files to inspect first
- [Dockerfile](Dockerfile#L1)
- [.vscode/tasks.json](.vscode/tasks.json#L1-L40)
- [compose.yaml](compose.yaml#L1)
- [compose.debug.yaml](compose.debug.yaml#L1)
- [package.json](package.json#L1-L40)
- [resources/walkthroughs/empty.md](resources/walkthroughs/empty.md)
- [README.md](README.md)

If anything here is unclear or you want examples expanded (e.g., exact `docker run` flags used by tasks, or a sample `vsce package` CI job), tell me which part to expand and I will iterate.
