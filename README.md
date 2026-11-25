# Unraid plugins and Docker apps by tabilzad

This repository is the [Community Applications](https://ca.unraid.net) feed for my Unraid
plugins and Docker apps. It holds the listing metadata only: one XML file per plugin or
app and the repository profile. Each project's code, releases and issues live in its own
repository.

| Name | Kind | Repository | Support | Listed from |
| --- | --- | --- | --- | --- |
| Claude Code for Unraid | Plugin | [claude-code-unraid](https://github.com/tabilzad/claude-code-unraid) | [Forum thread](https://forums.unraid.net/topic/201128-plugin-claude-code-for-unraid-claude-code-cli-on-the-host-with-sessions-that-survive-reboots-beta/) | this repository |
| GPU Unlock | Plugin | [gpu-unlock-unraid](https://github.com/tabilzad/gpu-unlock-unraid) | [GitHub issues](https://github.com/tabilzad/gpu-unlock-unraid/issues) (a forum thread will follow) | this repository |
| Sectorama | Docker app | [sectorama](https://github.com/tabilzad/sectorama) | [GitHub issues](https://github.com/tabilzad/sectorama/issues) | this repository |
| Docker Batch Update | Plugin | [unraid-docker-batch-update](https://github.com/tabilzad/unraid-docker-batch-update) | [Forum thread](https://forums.unraid.net/topic/200997-plugin-docker-batch-update-update-only-the-containers-you-pick-in-one-go/) | its own repository, until the registration moves (see below) |

To install one, search for it in the **Apps** tab of your Unraid server.

## Layout

| Path | Purpose |
| --- | --- |
| `ca_profile.xml` | The repository profile Community Applications shows. Required at the root, with a non-empty `Profile`. |
| `plugins/<name>.xml` | One wrapper per plugin. Its `PluginURL` must be byte-for-byte the `pluginURL` of that plugin's `.plg`; Community Applications reads the plg from there, so a new plugin release needs no change here. |
| `templates/<name>.xml` | One template per Docker app: the image, the ports, paths and variables Unraid offers when the app is installed. Its `TemplateURL` is the file's own raw URL here. A new image under the same tag needs no change here. |
| `icon.png` | The repository icon. |
| `scripts/validate` | The checks the Validate workflow runs: well-formed XML, required fields, every `PluginURL` reachable and equal to the plg's own `pluginURL`, every template's image tag present on Docker Hub, every icon a reachable PNG. |
| `LICENSE` | MIT. Community Applications requires an OSI-approved licence at the root. |

## Adding a plugin

1. Release the plugin from its own repository so that the `.plg` is reachable at a stable
   https URL (a GitHub release asset, or a raw file on the default branch).
2. Copy the wrapper the plugin's repository maintains (`ca/<name>.xml` in
   `claude-code-unraid` and `gpu-unlock-unraid`, `plugins/<name>.xml` in
   `unraid-docker-batch-update`) to
   `plugins/<name>.xml` here, unchanged. The plugin repository is the source; this
   repository mirrors it.
3. Add the plugin to the table above and mention it in `ca_profile.xml`.
4. Run `./scripts/validate` (needs `xmllint` and `curl`), commit, push. The Validate
   workflow runs the same checks on every push and once a week.
5. Wait for the next catalog build (one runs every few hours), then open this repository
   under [My submissions](https://ca.unraid.net/submissions). A plugin that is new to
   Community Applications stays out of the catalog until an admin has approved its
   `PluginURL`: the repository page lists it under **Plugin approval cases**, where
   **Request approval** asks for the review. The submit form is of no use here; it
   refuses a repository that is already approved.

## Adding a Docker app

1. Publish the image to a public registry under the tag the template will name.
2. Write `templates/<name>.xml` (root element `Container`). This repository is the
   source of the template, so its `TemplateURL` is
   `https://raw.githubusercontent.com/tabilzad/unraid-plugins/main/templates/<name>.xml`.
   Give every port, path and variable the app needs a `Config` entry, and say in
   `Requires` what the user has to provide besides the container.
3. Add the app to the table above and mention it in `ca_profile.xml`.
4. Run `./scripts/validate`, commit, push.
5. The next catalog build reads the template. The repository page under
   [My submissions](https://ca.unraid.net/submissions) shows a notice if the image or the
   template needs attention. The portal has no approval step of its own for a Docker
   template in an approved repository; if the app has not appeared after a day, ask on
   the Submission Support board.

## Registration

Community Applications keeps one feed repository per account. This repository takes over
from `unraid-docker-batch-update`, which was registered first and still carries its own
profile and wrapper. Community Applications refuses a submission that repeats a
`PluginURL` it already lists, so the Docker Batch Update wrapper stays out of this
repository until the registration has been moved here; the old repository keeps serving
that listing meanwhile. Once the move is done, copy `plugins/docker.batch.update.xml` from
the old repository into `plugins/` here and remove the profile and wrapper there.
