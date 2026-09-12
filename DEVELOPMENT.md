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

Public CI requires the platform bundle URL and SHA-256 to be configured after
Core's first package release. Until then, the same checks run locally with the
bundle path above. Hosted checks never have production deployment credentials.

Each DSP keeps its own installed plugin code, SDK copies, settings, databases,
credentials and browser sessions. Downloading a release does not change them.
See `RELEASES.md` for independent releases and controlled activation.
