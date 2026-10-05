# Privacy Policy: OSV Scanner Plugin

_Last updated: 2026-10-05_

The OSV Scanner plugin runs entirely on your machine. It has no backend, no accounts, and no analytics.

## What the plugin does with data

- **Local processing:** Claude Code starts the locally installed `osv-scanner` binary (`osv-scanner experimental-mcp`) over stdio. It reads lockfiles and dependency manifests in the path you choose. The triage command also searches your source code locally with Claude's Glob and Grep tools.
- **Data sent to a third party:** the `osv-scanner` binary calls the OSV API at `api.osv.dev`, operated by Google, in two ways:
  - To find known vulnerabilities, it sends package identifiers from your lockfiles: package names, versions, and ecosystems, and for some dependency types, commit hashes.
  - To retrieve an advisory, it requests the vulnerability ID (for example, a CVE or GHSA identifier).
  Your source code, file contents, and file paths are not part of these requests. See the [OSV Scanner documentation](https://google.github.io/osv-scanner/) and [OSV.dev](https://osv.dev/) for how that service handles requests.
- **Data we collect:** none. The plugin author receives no data from your use of the plugin.
- **Storage:** the plugin writes nothing to disk. The `osv-scanner` process keeps fetched advisories in memory only while it runs.
- **Your Claude session:** scan results (including the paths of the lockfiles that were scanned) and any code matches found by the triage command become part of your Claude conversation. That data is handled under your Claude settings and Anthropic's privacy policy, not by this plugin.
- **Personal data and credentials:** the plugin does not target personal data and does not read or require credentials. The OSV API requires no API key.

## Contact

Questions about this policy: open an issue at https://github.com/alejandrosaenz117/bonfires-marketplace/issues
