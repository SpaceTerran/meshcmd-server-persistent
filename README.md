# meshcmd-server-persistent

Dockerized **MeshCommander** with **server-side persistence** for the
computer list, designed for Intel® AMT / vPro environments.

> ⚠️ This is an **unofficial**, community-maintained project.\
> It is not affiliated with or endorsed by Intel or the MeshCommander
> upstream project.

------------------------------------------------------------------------

## Overview

MeshCommander traditionally stores its computer list in browser
`localStorage`, which means entries are lost when: - Browser cache is
cleared - A different browser is used - You access it from another
machine

This project packages MeshCommander into a Docker container and applies
build-time patches that allow the computer list to be stored
**server-side** on a persistent volume.

------------------------------------------------------------------------

## Features

-   Server-side persistent computer list
-   Works across browsers and devices
-   Docker-based deployment
-   Optional Traefik / reverse-proxy support
-   Node.js 16 for compatibility with older AMT TLS implementations

------------------------------------------------------------------------

## How It Works

-   Computer entries are still stored in browser localStorage
-   The same data is also written to a server-side file
-   On page load, the server-side list is loaded automatically

This allows persistence without modifying MeshCommander at runtime.

------------------------------------------------------------------------

## Security Notice

> **Passwords may be stored in plain text.**

If you choose to save credentials, they are written unencrypted to:

    data/computerlist.config

### Recommendations

-   Do not save passwords if you are uncomfortable with this
-   Restrict filesystem permissions on the data directory
-   Run only on trusted hosts and networks
-   Prefer HTTPS when exposing the service

------------------------------------------------------------------------

## Node.js Compatibility

Node.js 16 is used intentionally.

Older Intel AMT implementations rely on legacy TLS behavior that fails
on Node.js 18+ with errors such as: -
`ERR_SSL_UNSAFE_LEGACY_RENEGOTIATION_DISABLED` -
`ERR_SSL_TLSV1_ALERT_PROTOCOL_VERSION`

------------------------------------------------------------------------

## Upstream & Attribution

This project is based on the following upstream work:

-   **MeshCommander** by Ylian Saint-Hilaire\
    https://github.com/Ylianst/MeshCommander

-   **meshcmd Docker image** by brytonsalisbury\
    https://hub.docker.com/r/brytonsalisbury/meshcmd

This repository: - Builds MeshCommander from npm - Applies build-time
patches for persistence - Does not claim ownership of MeshCommander

------------------------------------------------------------------------

## License

MIT License.

MeshCommander is © Intel / Ylian Saint-Hilaire and is distributed under
its original license terms.
