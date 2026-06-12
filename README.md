<p align="center">
  <img src="./assets/pegasus-dl-banner.png" alt="Pegasus DL" width="100%">
</p>

<p align="center">
  <a href="https://github.com/pegasus-ps5/pegasus-dl/releases/tag/v1.3.0"><img alt="Release" src="https://img.shields.io/github/v/release/pegasus-ps5/pegasus-dl?label=release&color=24292f"></a>
  <a href="https://github.com/pegasus-ps5/pegasus-dl/releases/download/v1.3.0/pegasus_dl.elf"><img alt="Download" src="https://img.shields.io/badge/download-release-24292f"></a>
  <img alt="Built-in providers" src="https://img.shields.io/badge/providers-built--in-24292f">
  <img alt="PS5 homebrew" src="https://img.shields.io/badge/PS5-homebrew-24292f">
</p>

<p align="center">
  Direct package downloading from your PS5, managed through a local web interface.
</p>

---

## Overview

Pegasus DL runs as a payload on the console and serves a browser interface on
your local network. Add catalog sources, browse the combined Store catalog,
choose a download destination, queue packages, and follow progress from a phone,
tablet, or computer.

It is designed to keep the download workflow on the PS5 instead of routing
packages through another machine first.

Version 1.3.0 adds automatic extraction for supported RAR and 7z package
archives, a new installed-game Library page, APR Emu update actions, bundled
nanoDNS autostart for browser-assisted provider capture, a cleaner compact UI,
and fixes for queue refresh and provider link opening.

## Demo

![Pegasus DL demo](./assets/pegasus-dl-demo.webp)

## Download

| Release | Version |
| --- | --- |
| [`pegasus_dl.elf`](https://github.com/pegasus-ps5/pegasus-dl/releases/download/v1.3.0/pegasus_dl.elf) | `1.3.0` |

## Quick Start

1. Download the release asset above.
2. Send it to your PS5 with your usual payload loader.
3. Open Pegasus DL from a device on the same network.

   ```text
   http://<your-ps5-ip>:6970/
   ```

4. Choose a writable download folder in Settings.
5. Add a catalog source by upload or URL.
6. Search the Store catalog, select a package, and queue the download.
7. If a link opens a provider page, follow the PS5 browser flow until Pegasus
   captures or resolves the final download.
8. Use the Library tab to review installed games, update APR Emu where
   supported, or delete recognized installs.

## Features

| Area | Included in 1.3.0 |
| --- | --- |
| Sources | Add catalog files or URL sources, enable or disable sources, delete sources |
| Store catalog | Search packages, filter by source, review versions, sizes, details, and links |
| Installed Library | List installed games with app database metadata, art, storage details, APR Emu status, update actions, and supported deletion |
| Downloads | Queue direct or resolved links, track download, merge, extraction, finalization, speed, ETA, and final status |
| Archives | Detect supported single-file `.rar`, RAR5, and `.7z` links, extract automatically, prompt for RAR passwords, and reject split archives |
| Queue control | Pause, resume, cancel, retry, and clear finished jobs |
| Storage | Browse writable destinations, create folders, and choose where downloads land |
| Performance | Use multiple download connections with resume support when the host allows it |
| Logs | View payload messages and browser-side errors from the Logs tab |
| Provider links | Resolve supported providers inside the payload, or use PS5 browser-assisted capture for compatible provider pages |
| APR Emu | Detect and update bundled APR Emu 0.2.6 for supported folder-backed and exFAT image-backed installs |
| nanoDNS | nanoDNS is now embedded inside Pegasus DL and autostarts on launch |

## Catalogs

Pegasus DL does not ship with catalogs or package links. Bring catalog sources
you trust and are allowed to use.

Required package fields:

| Field | Purpose |
| --- | --- |
| `titleId` | Package identifier shown in the Store catalog |
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

## Archive Downloads

Pegasus DL 1.3.0 recognizes supported archive links before queueing them. A
single-file `.rar`, RAR5, or `.7z` download is extracted into the selected
destination automatically instead of leaving the archive file behind.

Archive jobs show extraction and finalization progress in the queue. RAR/RAR5
jobs can pause for a password when needed. 7z archives use HTTP range reads when
seeking is required, so the host must support byte ranges; password-protected
7z files are detected but not extracted yet.

Split archives such as `.part1.rar`, `.r00`, and `.7z.001` are rejected. During
finalization, Pegasus unwraps simple outer folders when the extracted content
contains a direct game folder with `EBOOT.BIN` or `sce_sys/param.sfo`.

## Installed Library

The Library tab reads installed titles from the PS5 app database and shows game
metadata, art when available, backing storage type, location, image-backed size,
and APR Emu state.

APR Emu 0.2.6 is bundled with Pegasus DL. The Library can update supported
folder-backed games and exFAT image-backed games by replacing
`fakelib/libSceAmpr.sprx`. `.ffpkg`, `.ffpfs`, and `.ffpfsc` storage is detected
but not patched yet.

The Library can also delete recognized installed games. Deletion is
irreversible: Pegasus removes the resolved backing image or folder, cleans the
title trackers when present, and removes the app database rows for that title.

## Provider Links

Pegasus DL does not require the separate Pegasus Resolver service in 1.3.0.
Provider handling now lives in the payload.

Direct links continue to download without any provider flow. For supported
provider pages, Pegasus can open the PS5 browser, watch for the final direct
download URL through a helper payload, validate the result, and queue it.
Pegasus also embeds nanoDNS 0.3 and starts it when browser-assisted capture
needs the helper and `nanodns.elf` is not already running.

Current provider handling:

| Provider state | Behavior |
| --- | --- |
| Built-in resolver | BuzzHeavier, DataNodes, and MediaFire can resolve direct package links from inside Pegasus DL |
| Browser-assisted | VikingFile and Rootz use the PS5 browser capture flow |
| Unknown | Pegasus can try guarded browser capture and queue the URL only after response validation |
| Not supported | The link can still be opened in the PS5 browser, but Pegasus will not queue from it automatically |

## 1.3.0 Notes

- Added automatic extraction for supported single-file RAR/RAR5 and 7z package
  archives.
- Added the installed-game Library page with metadata, art, storage details,
  APR Emu status, APR Emu update actions, and supported game deletion.
- Bundled APR Emu 0.2.6 and added update support for folder-backed installs
  and exFAT image-backed installs.
- Embedded nanoDNS and starts it automatically for browser-assisted provider
  capture.
- Cleaned up the web UI with top navigation, compact queue behavior, local
  fonts, no-image placeholders, loading states, and panel layout fixes.
- Fixed queue UI refresh issues and removed the browser warmup step when
  opening provider links.

## Scope

Pegasus DL is a downloader. It does not install packages, include package
catalogs, provide package links, bypass accounts, spoof PSN, bypass anti-cheat,
or unlock content.

Use it only with content you own or have permission to download.
