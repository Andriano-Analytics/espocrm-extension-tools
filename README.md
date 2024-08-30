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
* `--update-archive`             Download and store the latest version of Espo in the given branch for reuse

### Development Packages in Composer
The original repository does _not_ allow composer to install development packages. This repository _does_ allow development packages to be installed with composer. For example, `fzaninotto/Faker` allows the PHP scripts to generate fake data, which is helpful for testing. However, `fzaninotto/Faker` most likely should not be installed by the extension on a production system. To add development dependencies, add the following to the `composer.json` file in your module's code:

`src/files/custom/Espo/Modules/<ModuleName>/composer.json`:
```json
{
    "require-dev": {
        "fakerphp/faker": "^1.23"
    }
}
```
`src/files/custom/Espo/Modules/<ModuleName>/Resources/autoload.json`:
```json
{
    "psr-4": {
        "Faker\\": "custom/Espo/Modules/<ModuleName>/vendor/faker/src/Faker/"
    }
}
```

### Example Workflows
* `node build --all [--db-reset] [--local]`
* `node build --fetch --local; node build --install`
* `node build --copy`
* `node build --copy; node build --composer-install`

### Development Scripts
The espocrm-extension-template repository defines several files which are meant to be used for development purposes only. This repository is configured to ignore the development-only files when building the extension package. Here is the list of ignored files:
```javascript
const ignore = [
    cwd + '/src/files/custom/Espo/Modules/' + extensionParams.module + '/Classes/ConstantsDevelopment.php',
    cwd + '/src/scripts/AfterInstallDevelopment.php',
    cwd + '/src/scripts/AfterUninstallDevelopment.php',
    cwd + '/src/scripts/BeforeInstallDevelopment.php',
    cwd + '/src/scripts/BeforeUninstallDevelopment.php',
];
```