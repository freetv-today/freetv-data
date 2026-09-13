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

## Repository Structure

The following tree shows the canonical dataset paths managed or consumed by FreeTV’s publication and release workflows:

```text
freetv-data/
├── playlists/
│   ├── index.json
│   └── *.json
├── thumbs/
│   └── image files
├── releases/
│   ├── freetv-sample-data.zip
│   └── freetv-official-data.zip
├── config.json
├── freetv_mariadb_schema-create-db.sql
├── freetv_mariadb_schema-tables-only.sql
├── freetv_mariadb_full-create-db.sql
├── freetv_mariadb_full_data-tables-only.sql
├── freetv_mariadb_sample-create-db.sql
├── freetv_mariadb_sample_data-tables-only.sql
├── manifest.json
├── LICENSE
└── README.md
```

| Path                   | Purpose                                                                                                |
| ---------------------- | ------------------------------------------------------------------------------------------------------ |
| `config.json`          | Viewer-facing configuration exported from the Admin environment.                                       |
| `playlists/`           | Viewer playlist index and individual playlist JSON artifacts.                                          |
| `thumbs/`              | Canonical thumbnail collection referenced by Viewer playlist data.                                     |
| `manifest.json`        | Publication provenance and canonical playlist, show, sample-show, and thumbnail counts.                |
| `freetv_mariadb_*.sql` | Schema-only, complete-data, and sample-data MariaDB packages in create-database and tables-only forms. |
| `releases/`            | Locally generated Current Sample Data and Current Official Data First Run ZIPs.                        |

The publication workflow replaces the managed Viewer, thumbnail, SQL, and manifest paths as one validated dataset. Repository documentation, licensing, Git metadata, and other unrelated files are preserved.

## Data Architecture

The repository stores several synchronized representations of the official FreeTV dataset:

| Representation | Consumer | Purpose |
| --- | --- | --- |
| Viewer artifacts | FreeTV Viewer | Provide static configuration, playlist, show, and thumbnail data. |
| MariaDB packages | FreeTV Admin Dashboard and database operators | Create or populate a FreeTV MariaDB database. |
| Publication manifest | FreeTV Tooling and maintainers | Record dataset provenance and expected content counts. |
| First Run release packages | FreeTV Admin Dashboard | Initialize MariaDB and matching Viewer artifacts from one verified ZIP. |

These representations describe the same canonical dataset but serve different consumers. The Viewer does not read the SQL packages, and the Admin Dashboard does not use the repository’s JSON files as its live data store.

### Viewer Artifacts

The Viewer-facing dataset consists of:

```text
config.json
playlists/
├── index.json
└── *.json
thumbs/
└── image files
```

`config.json` contains published Viewer configuration and its publication timestamp.

`playlists/index.json` identifies the default playlist and lists the available playlist files. Each individual playlist file contains its playlist metadata and show records.

Viewer show records include information such as:

* category;
* active or disabled status;
* Internet Archive identifier;
* title and description;
* start and end years;
* IMDb identifier; and
* optional grouping information.

The Internet Archive identifier tells the Viewer which source item to open or play. The IMDb identifier is also used to associate a show with its thumbnail filename.

### MariaDB Packages

The repository contains three types of MariaDB package, each in two forms:

| Dataset                       | Create-database package               | Tables-only package                          |
| ----------------------------- | ------------------------------------- | -------------------------------------------- |
| Schema only                   | `freetv_mariadb_schema-create-db.sql` | `freetv_mariadb_schema-tables-only.sql`      |
| Complete official dataset     | `freetv_mariadb_full-create-db.sql`   | `freetv_mariadb_full_data-tables-only.sql`   |
| Representative sample dataset | `freetv_mariadb_sample-create-db.sql` | `freetv_mariadb_sample_data-tables-only.sql` |

Create-database packages create and select the FreeTV database before installing their schema or data.

Tables-only packages operate within a database selected by the importing application or database operator. They do not create or select the database themselves.

The schema-only packages contain the FreeTV database structure without the distributable playlist and show collection. The complete and sample packages contain the corresponding database structure and dataset records.

Admin users, credentials, sessions, problem reports, report IPs, and locally configured application-setting values are not copied from the source Admin environment into the distributable data packages. First Run creates the initial Administrator account separately.

### Publication Manifest

The root `manifest.json` describes the canonical publication. It records:

* publication format version;
* dataset generation timestamp;
* reconciled Data Snapshot name;
* reconciled snapshot capture timestamp;
* playlist count;
* complete show count;
* sample show count; and
* thumbnail count.

The snapshot fields record which captured Admin environment was reviewed before publication. They do not mean that the repository contents were copied from the snapshot. The published artifacts are generated from the configured local Admin environment after reconciliation and validation.

The root publication manifest records provenance and logical counts. It is distinct from the internal manifest included in each First Run release ZIP.

### First Run Release Packages

The `releases/` directory contains:

```text
freetv-sample-data.zip
freetv-official-data.zip
```

Each archive contains:

```text
manifest.json
database.sql
config.json
playlists/
thumbs/
```

The sample package supports the Admin Dashboard’s **Current Sample Data** First Run mode. It contains representative sample SQL, matching Viewer artifacts, and referenced thumbnails when available.

The official package supports **Current Official Data**. It contains the complete official SQL dataset, matching Viewer artifacts, and the complete canonical thumbnail collection.

Each package’s internal `manifest.json` identifies the dataset type and records the exact permitted files with their SHA-256 digests. First Run also verifies the complete downloaded ZIP against the separate archive-level digest supplied by the configured dataset-package metadata endpoint.

For the package-generation, validation, hosting, metadata, and testing procedures, see [Dataset Publishing and Distribution](https://github.com/freetv-today/freetv-tooling/blob/main/docs/dataset-distribution.md).

## License

This repository is released under the [GPL v3](LICENSE) license.
