# FreeTV Data

FreeTV Data contains the official distributable datasets used by the [FreeTV Viewer](https://github.com/freetv-today/freetv-viewer) and the [FreeTV Admin Dashboard](https://github.com/freetv-today/freetv-server).

The repository includes Viewer-compatible configuration, playlist JSON, thumbnails, MariaDB installation packages, publication metadata, and the release packages used by the Admin Dashboard’s Current Sample Data and Current Official Data First Run modes.

MariaDB is authoritative for content managed through the Admin Dashboard. `freetv-data` is the canonical published representation of the official distributable FreeTV dataset. Its managed contents are generated, validated, and published through [`freetv-tooling`](https://github.com/freetv-today/freetv-tooling), not used as live Admin storage.

## Contents

* Viewer configuration and playlist JSON
* Show and movie thumbnail images
* Complete and sample MariaDB datasets
* Schema-only MariaDB packages
* Publication provenance and dataset counts
* Current Sample Data and Current Official Data First Run packages

> [!IMPORTANT]
> This repository contains published data artifacts. Editing generated files directly can make the Viewer, SQL, manifest, and release-package representations disagree. Use the documented FreeTV Tooling workflows when maintaining the official dataset.

## How do I ...  ?

`freetv-data` is a versioned artifact repository rather than a standalone application. It has no development server or package-installation step. The actions below use the repository directly or operate on it through other FreeTV repositories.<br/>

| I want to... | What do I do? | What happens? |
| --- | --- | --- |
| **Inspect or download the official FreeTV dataset** | Clone or download this repository. | Provides the canonical published Viewer artifacts, thumbnails, MariaDB packages, manifest, and locally built First Run release packages. |
| **Load the official data into a local Viewer** | Use `npm run dev:install-viewer-data` from [`freetv-tooling`](https://github.com/freetv-today/freetv-tooling#viewer-development-data). | Resets the Viewer’s disposable development data from this repository without modifying the canonical dataset. |
| **Initialize FreeTV with the current dataset** | Use Current Sample Data or Current Official Data during the Admin Dashboard’s [First Run](https://github.com/freetv-today/freetv-server#first-run). | Downloads and verifies the selected release package, then initializes MariaDB and the matching Viewer artifacts. |
| **Maintain or update the official dataset** | Follow the [Dataset Publishing and Distribution](https://github.com/freetv-today/freetv-tooling/blob/main/docs/dataset-distribution.md) workflow. | Validates current Admin data and publishes matching Viewer, thumbnail, SQL, and manifest artifacts to this repository. |
| **Build distributable First Run packages** | Run `npm run release:build` from `freetv-tooling` after validating and publishing the canonical dataset. | Creates and validates the Current Sample Data and Current Official Data ZIPs under `releases/`. |

## License

This code is released under the [GPL v3](LICENSE) license.
