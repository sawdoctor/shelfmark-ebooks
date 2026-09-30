# Shelfmark eBooks for Libraseer

This repository is the portable eBook acquisition role used by Libraseer. It
runs the official [Shelfmark](https://github.com/calibrain/shelfmark) image,
searches through Prowlarr, hands NZBs to SABnzbd, and imports completed EPUB/PDF
files into an eBook library.

It is a deployment repository, not a Shelfmark fork. Shelfmark remains
Copyright © 2024 CaliBrain and is distributed under its MIT licence.

## What is included

- a small Docker Compose deployment pinned to Shelfmark `v1.4.0`;
- portable host paths configured through `.env`;
- an optional post-import script that adds missing Calibre series metadata when
  a filename contains an unambiguous `[Series 2]` suffix.

The script never overwrites existing series metadata, and a script error is
reported without undoing an otherwise successful import.

## Quick start

Requirements: Docker Compose, an existing Prowlarr instance, an existing
SABnzbd instance, and a destination folder used by your eBook library.

```bash
cp .env.example .env
docker compose config
docker compose up -d
```

Before starting, edit `.env` and set:

- `PROWLARR_URL` and `PROWLARR_API_KEY`;
- `SABNZBD_URL` and `SABNZBD_API_KEY`;
- `EBOOK_LIBRARY_DIR` to the real host library path;
- `SAB_DOWNLOADS_DIR` to the host folder containing completed SABnzbd jobs.

Open `http://HOST:SHELFMARK_PORT`, create the initial Shelfmark administrator,
and confirm the Prowlarr and SABnzbd connections in Shelfmark settings.

## Download paths

Shelfmark must be able to read the path reported by SABnzbd. This Compose file
mounts `SAB_DOWNLOADS_DIR` at `/downloads` inside Shelfmark. Configure SABnzbd to
report completed paths below `/downloads`, or add an equivalent remote-path
mapping in Shelfmark.

Do not expose broader host storage merely to make a path resolve. Mount only the
completed-download folder Shelfmark needs and the intended library destination.

## Libraseer connection

Create a dedicated Shelfmark account for Libraseer, then configure Libraseer:

```env
SHELFMARK_EBOOK_URL=http://shelfmark-ebooks:8084
SHELFMARK_EBOOK_USERNAME=libraseer
SHELFMARK_EBOOK_PASSWORD=change-me
SHELFMARK_EBOOK_SOURCE=prowlarr
```

If the two projects use separate Compose networks, use a hostname/IP reachable
from Libraseer instead of the service name shown above.

## Updating

The image is deliberately pinned. Review Shelfmark's release notes and test an
upgrade before changing `SHELFMARK_IMAGE`; do not use `latest` for an unattended
production deployment.

## Credits

- [Shelfmark](https://github.com/calibrain/shelfmark) by CaliBrain — application
  and container image, MIT licensed.
- [Prowlarr](https://github.com/Prowlarr/Prowlarr) and
  [SABnzbd](https://github.com/sabnzbd/sabnzbd) — search and Usenet acquisition.
- Libraseer deployment maintained by sawdoctor.

See [LICENSE](LICENSE) for the deployment repository licence.
