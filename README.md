# Unraid plugins by tabilzad

This repository is the [Community Applications](https://ca.unraid.net) feed for my Unraid
plugins. It holds the listing metadata only: one XML wrapper per plugin and the repository
profile. Each plugin's code, releases and issues live in its own repository.

| Plugin | Repository | Support | Listed from |
| --- | --- | --- | --- |
| Claude Code for Unraid | [claude-code-unraid](https://github.com/tabilzad/claude-code-unraid) | [Forum thread](https://forums.unraid.net/topic/201128-plugin-claude-code-for-unraid-claude-code-cli-on-the-host-with-sessions-that-survive-reboots-beta/) | this repository |
| Docker Batch Update | [unraid-docker-batch-update](https://github.com/tabilzad/unraid-docker-batch-update) | [Forum thread](https://forums.unraid.net/topic/200997-plugin-docker-batch-update-update-only-the-containers-you-pick-in-one-go/) | its own repository, until the registration moves (see below) |

To install one, search for it in the **Apps** tab of your Unraid server.

## Layout

| Path | Purpose |
| --- | --- |
| `ca_profile.xml` | The repository profile Community Applications shows. Required at the root, with a non-empty `Profile`. |
| `plugins/<name>.xml` | One wrapper per plugin. Its `PluginURL` must be byte-for-byte the `pluginURL` of that plugin's `.plg`; Community Applications reads the plg from there, so a new plugin release needs no change here. |
| `icon.png` | The repository icon. |
| `scripts/validate` | The checks the Validate workflow runs: well-formed XML, required fields, every `PluginURL` reachable and equal to the plg's own `pluginURL`, every icon a reachable PNG. |
| `LICENSE` | MIT. Community Applications requires an OSI-approved licence at the root. |

## Adding a plugin

1. Release the plugin from its own repository so that the `.plg` is reachable at a stable
   https URL (a GitHub release asset, or a raw file on the default branch).
2. Copy the wrapper the plugin's repository maintains (`ca/<name>.xml` in
   `claude-code-unraid`, `plugins/<name>.xml` in `unraid-docker-batch-update`) to
   `plugins/<name>.xml` here, unchanged. The plugin repository is the source; this
   repository mirrors it.
3. Add the plugin to the table above and mention it in `ca_profile.xml`.
4. Run `./scripts/validate` (needs `xmllint` and `curl`), commit, push. The Validate
   workflow runs the same checks on every push and once a week.
5. Open <https://ca.unraid.net/submit>, choose this repository and run **Scan**, so that
   Community Applications picks up the new wrapper. A plugin that is new to Community
   Applications is reviewed by the moderators before it appears.

## Registration

Community Applications keeps one feed repository per account. This repository takes over
from `unraid-docker-batch-update`, which was registered first and still carries its own
profile and wrapper. Community Applications refuses a submission that repeats a
`PluginURL` it already lists, so the Docker Batch Update wrapper stays out of this
repository until the registration has been moved here; the old repository keeps serving
that listing meanwhile. Once the move is done, copy `plugins/docker.batch.update.xml` from
the old repository into `plugins/` here, run Scan, and remove the profile and wrapper there.
