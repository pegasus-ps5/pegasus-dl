<p align="center">
  <img src="./assets/pegasus-dl-banner.png" alt="Pegasus DL" width="100%">
</p>

<p align="center">
  <a href="https://github.com/pegasus-ps5/pegasus-dl/releases/tag/v1.0.0"><img alt="Release" src="https://img.shields.io/github/v/release/pegasus-ps5/pegasus-dl?label=release&color=24292f"></a>
  <a href="https://github.com/pegasus-ps5/pegasus-dl/releases/download/v1.0.0/pegasus_dl.elf"><img alt="Download" src="https://img.shields.io/badge/download-release-24292f"></a>
  <a href="https://github.com/pegasus-ps5/pegasus-resolver"><img alt="Resolver" src="https://img.shields.io/badge/resolver-pegasus--resolver-24292f"></a>
  <img alt="PS5 homebrew" src="https://img.shields.io/badge/PS5-homebrew-24292f">
</p>

<p align="center">
  Direct package downloading from your PS5, managed through a local web interface.
</p>

---

## Overview

Pegasus DL runs as a payload on the console and serves a browser interface on
your local network. Add catalog sources, browse the combined library, choose a
download destination, queue packages, and follow progress from a phone, tablet,
or computer.

It is designed to keep the download workflow on the PS5 instead of routing
packages through another machine first.

## Demo

![Pegasus DL demo](./assets/pegasus-dl-demo.webp)

## Download

| Release | Version | SHA-256 |
| --- | --- | --- |
| [`pegasus_dl.elf`](https://github.com/pegasus-ps5/pegasus-dl/releases/download/v1.0.0/pegasus_dl.elf) | `1.0.0` | `ccf08f811e500fedd45b7ba0646342f37180891a0ff5bdc06b7cfe4a4601fe46` |

## Quick Start

1. Download the release asset above.
2. Send it to your PS5 with your usual payload loader.
3. Open Pegasus DL from a device on the same network.

   ```text
   http://<your-ps5-ip>:6970/
   ```

4. Choose a writable download folder in Settings.
5. Add a catalog source.
6. Search the library, select a package, and queue the download.

## Features

| Area | Included in 1.0.0 |
| --- | --- |
| Sources | Add catalog files, enable or disable sources, delete sources |
| Library | Search packages, filter by source, review versions, sizes, details, and links |
| Downloads | Queue direct links, track progress, speed, ETA, and final status |
| Queue control | Pause, resume, cancel, retry, and clear finished jobs |
| Storage | Browse writable destinations, create folders, and choose where downloads land |
| Performance | Use multiple download connections with resume support when the host allows it |
| Logs | View payload messages and browser-side errors from the Logs tab |
| Resolver | Optionally use [Pegasus Resolver](https://github.com/pegasus-ps5/pegasus-resolver) when a source page needs to be resolved first |

## Catalogs

Pegasus DL does not ship with catalogs or package links. Bring catalog sources
you trust and are allowed to use.

Required package fields:

| Field | Purpose |
| --- | --- |
| `titleId` | Package identifier shown in the library |
| `title` | Package name |
| `downloadLinks[].url` | At least one valid direct download URL |

Common optional fields:

| Field | Purpose |
| --- | --- |
| `version` | Package version shown beside the title |
| `posterUrl` | Cover or poster image |
| `description` | Notes shown in the detail view |
| `downloadSource` | Original source page |
| `sizeBytes` | Estimated package size |

Minimal catalog:

```json
{
  "name": "My Catalog",
  "packages": [
    {
      "titleId": "ABCD12345",
      "title": "Example Package",
      "version": "1.00",
      "downloadLinks": [
        {
          "name": "Mirror",
          "url": "https://example.com/downloads/example-package"
        }
      ]
    }
  ]
}
```

## Pegasus Resolver

[Pegasus Resolver](https://github.com/pegasus-ps5/pegasus-resolver) is optional.
Use it when a catalog link opens a provider page that needs one more step before
Pegasus DL can queue the package.

Open Settings, paste your Resolver URL, and save. Direct links continue to
download without the resolver.

## 1.0.0 Notes

- Uploaded catalog files are supported.
- Remote catalog URL sources are visible in the UI but are not wired in this build.
- The queue does not survive a reboot yet.
- Resume support depends on the download host.
- Firmware and loader compatibility depends on your PS5 homebrew environment.
- The web UI is available on your local network while the payload is running.

## Scope

Pegasus DL is a downloader. It does not install packages, include package
catalogs, provide package links, bypass accounts, spoof PSN, bypass anti-cheat,
or unlock content.

Use it only with content you own or have permission to download.
