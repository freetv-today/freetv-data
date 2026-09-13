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

## License

This code is released under the [GPL v3](LICENSE) license.
