# DSP development

Requires Node 22 or 24 and npm. Obtain the verified platform package bundle from
Core's build/release artifacts, then run from this repository:

```sh
npm run bootstrap -- /absolute/build/platform-packages
npm run check
npm test
npm run build -- /absolute/build/dsp-candidate
npm run export -- /absolute/build/dsp-source
```

The bundle's installer verifies package contents and exact versions declared in
`package.json`. DSP builds use `tooling/frontend` and its own lockfile. They do
not read a Core dashboard checkout. The SDK packages sealed plugin backends and
frontends and embeds each plugin's SDK runtime. The DSP artifact contains runtime
code, dependency copies and separate sealed plugin packages. `release.json` records
its hashes. Builds are development candidates; `0.0.0` is an unreleased placeholder.

```sh
bin/dispatch create plugin sample-notes
bin/dispatch plugin generate sample-notes
bin/dispatch plugin check sample-notes
```

For API/dashboard preview, run Core's `bin/dispatch plugin dev` with the plugin's
absolute source path. Core's preview uses a sealed package and two synthetic DSPs.
Plugin code consumes the SDK; Core internals are not a runtime dependency.

`npm test` runs the standalone suites listed in `tooling/tests.json`. Integration
tests require an explicit Core source export installed as a test-only package;
Core's `tooling/integration-package.js` installs that fixture and its platform
dependencies into this project's `node_modules`. Then run `npm run test:integration`.

Public CI uses the reviewed dependency lock in `tooling/platform-dependencies.json`.
Before the first Core release, a separate job builds a bundle from an exact Core
commit. Production publication requires a published bundle URL and SHA-256.
Hosted checks never have production deployment credentials.

Each DSP keeps its own installed plugin code, SDK copies, settings, databases,
credentials and browser sessions. Downloading a release does not change them.
See `RELEASES.md` for independent releases and controlled activation.

## GitHub workflow

Use an isolated feature worktree from freshly fetched `origin/main`. Open a draft
PR after the first reviewed commit. Run the applicable local checks and wait for
GitHub checks on the exact PR commit before marking it ready. Report the PR link,
changes and verification in chat. Only merge after the owner explicitly approves;
recheck the approved head and required checks immediately before merging. Automatic
merge is disabled. Main requires PRs, up-to-date checks and resolved conversations.
The owner's chat approval is the human gate; GitHub does not interpret chat.

`tooling/workflow.py pr-details --repo OWNER/REPOSITORY --pr NUMBER` reports PR
facts. This helper is read-only; use normal git/gh commands for branches and PRs.
Keep multiline PR bodies in a file and pass `--body-file`.

The manual release workflow is a separate operation; never dispatch it as part of
ordinary development, merging, testing or retrying CI. See `RELEASES.md`.
