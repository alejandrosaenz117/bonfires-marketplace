# Privacy Policy: OSV Scanner Plugin

_Last updated: 2026-10-05_

The OSV Scanner plugin runs entirely on your machine. It has no backend, no accounts, and no analytics.

## What the plugin does with data

- **Local processing:** Claude Code starts the locally installed `osv-scanner` binary (`osv-scanner experimental-mcp`) over stdio. It reads lockfiles and dependency manifests in the path you choose. The triage command also searches your source code locally with Claude's search tools.
- **Data sent to a third party:** to find known vulnerabilities, `osv-scanner` sends package names, versions, and ecosystems (and commit hashes for Git-based dependencies) to the OSV API at `api.osv.dev`, operated by Google. Your source code, file contents, and file paths are not sent. See the [OSV Scanner documentation](https://google.github.io/osv-scanner/) and the [OSV.dev](https://osv.dev/) site for how that service handles requests.
- **Data we collect:** none. The plugin author receives no data from your use of the plugin.
- **Retention:** the plugin stores nothing. Scan results exist only in your Claude Code session, which is governed by your Claude settings and Anthropic's privacy policy.
- **Personal data and credentials:** the plugin does not target personal data and does not read or require credentials.

## Offline use

`osv-scanner` can scan against a locally downloaded vulnerability database so that no package data leaves your machine. See its documentation for the offline options.

## Contact

Questions about this policy: open an issue at https://github.com/alejandrosaenz117/bonfires-marketplace/issues
