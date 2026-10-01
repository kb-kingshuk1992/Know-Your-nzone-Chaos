# Build the Mac app

Build on an Apple Silicon Mac running macOS 13 or later, or use the supplied GitHub Actions workflow. End users receive an app ZIP and need no Python or Node installation. This source is JavaScript and does not use Python for building.

## Developer build

Install Node.js 24 on the developer's Mac, then open Terminal in this extracted source folder:

```sh
npm install
MAC_RELEASE_MODE=development npm run package:mac
```

The output is `dist/Know_Your_Nzone_ManualToken_Mac_AppleSilicon_Development.zip`. It is built and locally signed on macOS for owner testing. Downloaded copies may still be blocked by Gatekeeper; this is not the signed production release. No launcher is included and no security checks are disabled.

## Signed production release

Install the organisation's Developer ID Application certificate in the Mac builder's keychain. Keep its private key and password out of source code and chat. Configure these environment variables locally or through GitHub Actions secrets:

- `MAC_SIGN_IDENTITY`: the full `Developer ID Application: ...` certificate identity.
- `APPLE_TEAM_ID`: Apple Developer team identifier.
- `APPLE_ID`: the Apple account used for notarization.
- `APPLE_APP_SPECIFIC_PASSWORD`: an Apple app-specific password for notarization.

On a local Mac, a preconfigured `NOTARY_KEYCHAIN_PROFILE` can replace the Apple ID and app-specific password variables. See Apple's `notarytool store-credentials` documentation.

Run `MAC_RELEASE_MODE=signed npm run package:mac`. The build signs the full app, notarizes it, staples Apple's ticket, verifies the signature and notarization ticket, and runs Gatekeeper assessment. If any step fails, it does not produce a signed release ZIP.

The output is `dist/Know_Your_Nzone_ManualToken_Mac_AppleSilicon_Signed.zip` plus its SHA256 checksum. Distribute this ZIP, not the development ZIP.

## GitHub Actions

Place `.github/workflows/build-macos.yml` at the repository root. Keep `Know_Your_Nzone_ManualToken_MacOS_Source.zip` at the repository root. Open Actions, select Build MacOS Manual Token, choose Run workflow and select `development` or `signed`.

For signed mode, configure the following repository Actions secrets through GitHub Settings. Do not commit certificates or passwords:

- `MAC_CERTIFICATE_BASE64`: the Developer ID Application P12 certificate exported as base64.
- `MAC_CERTIFICATE_PASSWORD`: its export password.
- `MAC_SIGN_IDENTITY`
- `APPLE_TEAM_ID`
- `APPLE_ID`
- `APPLE_APP_SPECIFIC_PASSWORD`

The workflow imports the certificate into an ephemeral runner keychain, performs the native build and removes the temporary keychain. It uploads a downloadable ZIP as an Actions artifact. It never signs a release with fake credentials or turns a development ZIP into a production ZIP by renaming it.

GitHub's `macos-15` runner is Apple Silicon arm64: https://docs.github.com/en/actions/reference/runners/github-hosted-runners
Electron signing guide: https://www.electronjs.org/docs/latest/tutorial/code-signing

The owner's first launch and live Mappls API acceptance check on the target MacBook are still required before wider distribution. Source checks are available in `tests.cjs` for a developer who chooses to run them; native package creation and signing do not prove live API connectivity.
