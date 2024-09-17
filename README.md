# Custom Extension Tools for EspoCRM

This repository is based on espocrm/extension-tools. The enhancements provided by this repository speed up development in some situations, such as:
* Dropping and recreating the database to start fresh
* Running the sequence of steps from the `all` flag starting at `copy` until the end
* Running the `Before Install` scripts
* Including development packages in `composer install`
* Using a local copy of the EspoCRM archive instead of downloading it from Github. Multiple branches can be archived simultaneously.
* Running development-only PHP scripts, such as BeforeInstallDevelopment.php
* Adding module-specific constants, both for development and production

The original commandline switches are as follows:
* `--after-install`
* `--all`
* `--composer-install`
* `--copy`
* `--copy-file`
* `--extension`
* `--fetch`
* `--rebuild`

The new commandline switches are as follows:
* `--before-install`             Run only the beforeInstall process for the extension
* `--copy-to-end`                Run the `all` switch from the `copy` step until the end
* `--db-reset`                   Create (or drop and recreate) the database schema (only the schema, no tables)
* `--extension`                  Build the extension for distribution
* `--rebuild`                    Rebuild Espo's configuration (CLI version of UI->Administration->Rebuild)
* `--local`                      Use with any fetch command (`--all`, `--update-archive`, etc.) to use a local version of the repository instead of downloading it
* `--update-archive`             Download and store the latest version of Espo in the given branch for reuse