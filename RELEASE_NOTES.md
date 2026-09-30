# Libraseer eBook deployment 0.1.0-alpha.7

This release turns the previous server-specific Compose file into a portable
deployment of official Shelfmark.

## Changed

- pins `ghcr.io/calibrain/shelfmark:v1.4.0`;
- moves host paths, service URLs, IDs, timezone, and port to `.env`;
- uses an ordinary project network instead of requiring private external
  networks;
- documents the SABnzbd same-path/remote-path-mapping requirement;
- retains the small, non-destructive EPUB series metadata script.

## Removed

- maintainer-specific absolute host paths;
- the full `base_handler.py` container override;
- the InfiniDysk-specific cleanup hook and private network assumptions.

The official Shelfmark v1.4.0 image already contains the general retry and
path-safety work that the old override duplicated. Review and test future image
updates before changing the pin.
