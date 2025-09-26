# freetv-data

**Version:** 1.0.0-beta

This repository contains all public data and static files used by the Free TV apps (viewer, admin dashboard, and API).

## Structure

- `/playlists/` — All playlist JSON files (e.g., `index.json`, `freetv.json`, etc.)
- `/thumbs/` — Show/movie thumbnail images
- `/config.json` — Global app configuration

## Usage

- **Viewer apps** fetch data from this repo's deployed location (e.g., `/playlists/index.json`, `/config.json`).
- **Admin dashboard** reads/writes these files to manage content.
- **APIs** (e.g., `beacon.php`, `report-problem.php`) may read/write as needed.

## Contribution

- Please submit changes to playlists or config via pull request.
- Do not include sensitive or private data.

## License

Data in this repository is intended for public use by the Free TV project.
