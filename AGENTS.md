# Dispatch DSP

This is the future `dispatch-dsp` repository. The current task is local only:
do not initialize Git, publish, or change production. In the development workspace,
read the workspace `AGENTS.md` and `dev/AGENTS.md` as well.

- `runtime/`: DSP supervisor, jobs, collector execution, scoped workers and local vault/session handling.
- `plugins/`: Paycom and future plugin source, including each plugin's frontend.
- `tooling/`: portable checks/builds and frontend compiler dependencies.
- `bin/`: DSP command entry points.
- `compatibility/`: retained legacy adapters.

The SDK source belongs to Core. Bootstrap from its verified package bundle;
never import a sibling Core checkout. Each DSP release contains its own copies
of the SDK/protocol/runtime support packages and sealed plugin packages.
Credentials, settings, databases and installed code belong to each DSP's private
runtime directory, never this repository. See `DEVELOPMENT.md` and `RELEASES.md`.
