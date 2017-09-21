[![Latest Stable Version](https://poser.pugx.org/drush/drush/v/stable.png)](https://packagist.org/packages/drush/drush) [![Total Downloads](https://poser.pugx.org/drush/drush/downloads.png)](https://packagist.org/packages/drush/drush) [![Latest Unstable Version](https://poser.pugx.org/drush/drush/v/unstable.png)](https://packagist.org/packages/drush/drush) [![License](https://poser.pugx.org/drush/drush/license.png)](https://packagist.org/packages/drush/drush) [![Documentation Status](https://readthedocs.org/projects/drush/badge/?version=master)](https://readthedocs.org/projects/drush/?badge=master)
Shields to consider: https://shields.io/

Note: This documentation is in progress and should not be relied on. The project is in full development.

# NextEuropa Toolkit
<img align="left" width="50%" src="https://ec.europa.eu/info/sites/info/themes/europa/images/svg/logo/logo--en.svg" />

<p>The NextEuropa Toolkit is a composer package designed to speed up the
development of Drupal websites in the NextEuropa project. It's main
component is the Phing build system that builds your development
environments, deploy packages and test packages.</p>

<details><summary>Table of Contents</summary>

- [Background](#background)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
	- [Build properties](#build-properties)
	- [Phing](#phing)
- [Maintainers](#maintainers)
- [Contribute](#contribute)
- [License](#license)
</details>

## Background
This composer package helps developers working on Drupal websites in the
NextEuropa project speed up and align their development. The toolkit is
opensource and in no way obligated to provide support or guarantee
compatibility with your system. It is officially maintained by members
of the Quality Assurance team for the NextEuropa project. They oversee
general workflow and overall quality of projects. The standards emposed
by the Quality Assurance team are a mix of internally provided standards
and a collection of standards established by the leading contributors to
the project.

## Requirements
There are three separate ways of using the NextEuropa project. Either
you use an environment with Docker installed, an environment without, or
a mix of both.
  
<details><summary><b>Docker Solo</b></summary>

This requirement for docker only needs to have docker in docker support.
The configuration to accomplish this is complex and if implemented
incorrectly can give you problems. We recommend this approach only
for seasond docker users.<br>*Required components*:
[Docker](https://docs.docker.com/engine/installation/linux/docker-ce/centos/)
</details>
<details><summary><b>Docker Plus</b></summary>

Instead of having the absolute minimal requirement you can install the
host level components Composer and Phing on the non-docker environment.
Then this can spin up the docker containers for you without having to
configure a complicated docker installation.<br>*Required components*:
[Composer](https://getcomposer.org/),
[Phing](https://packagist.org/packages/phing/phing),
[Docker](https://docs.docker.com/engine/installation/linux/docker-ce/centos/)
</details>
<details><summary><b>Docker Zero</b></summary>

If you are not interested in the advantages that the toolkit can give
you with the provided docker images you can keep a normal host only setup.
But it is very much recommended to use docker as it will give you
everything you need.<br>*Required components*:
[Composer](https://getcomposer.org/),
[LAMP Stack](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-centos-7)
</details>

## Installation
There are two different types of projects for which to install this
composer package. You can either create a new platform repository or a
new subsite repository. Both types use this toolkit to build the project
and it's release packages.

<details><summary><b>composer create-project ec-europa/platform dirname ~3.0.0</b></summary>

This command will clone the repository of the ec-europa/platform project
and run composer install on it. The installation of the toolkit itself
is run seperately to create a clear separation between the toolkit and
your project source code. Extending the toolkit is not possible without
contributing your functionalities through pull requests. You will be
requested to remove or keep the VCS files after cloning the project. For
development purposes you should NOT agree to remove these files. Only for
deploy and testing purposes it is recommended to remove the version
control system. There is only one official platform project which is
maintained by the NextEuropa core development team.
</details>

<details><summary><b>composer create-project ec-europa/subsite dirname ~3.0.0</b></summary>

This command will clone the repository of the ec-europa/subsite project
and run composer install on it. The installation of the toolkit itself
is run seperately to create a clear separation between the toolkit and
your project source code. Extending the toolkit is not possible without
contributing your functionalities through pull requests. You will be
requested to remove or keep the VCS files after cloning the project.
Upon initial creation of your project you need to remove the VCS files
as you will commit the source code to your own repository. After your
project is registered by NextEuropa as an official subsite you will be
able to direct pull requests to a reference repository.

After your project is accepted you can register your fork locally or
through packagist to use the same composer create-project command on 
your fork that serves development only.

<details><summary>To locally register your package the following code to your global config.json:</summary><p>

```json
{
  "repositories": [
    {
      "type": "package",
      "package": {
        "name": "ec-europa/<project-id>-dev",
        "version": "dev-master",
        "source": {
          "type" : "git",
          "url" : "https://github.com/<github-account>/<project-id>-dev.git",
          "reference" : "master"
        }
      }
    }
  ],
}

```
</p></details>

<details><summary>To globally register your development repository you can visit packagist.org.</summary><p>

[https://packagist.org/packages/submit]
</p></details>
</details>

## Usage

### Build properties

There are 3 different sets of build properties files that you can use.
If you are unfamiliar with the purpose behind each different type of
properties file please open the descriptions and read what they are
designed for.

<details><summary><b>default.props</b><br></summary>

This properties file contains the default settings, acts as a loading
mechanism and is an example file of what properties are available to
you. Upon the installation or update of the toolkit this file will be
placed in your repository.

</details>
<details><summary><b>develop.props</b></summary>

This file will contain configuration which is unique to your development
environment. It is useful for specifying your database credentials and
the username and password of the Drupal admin user so they can be used
during the installation. Next to credentials you have many development
settings that you can change to your liking. Because these settings are
personal they should not be shared with the rest of the team. Make sure
you never commit this file.
</details>
<details><summary><b>project.props</b><br></summary>

Always commit this file to your repository. This file is required for
all NextEuropa projects. Without it your build system will fail. It must
contain a minimum set of properties, like project.id, etc. The toolkit
will notify you if any properties are missing.
</details>

### Phing
This is the main component of the toolkit. It allows you to locally set
up your project and is integrated with the CI and CD tools to optimize
the development process. To show the list of possible targets you can
execute:

<details><summary><b>./ssk/phing</b></summary>

```
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| Target name                           | Visibility | Description                                                                   |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| deprecated                                                                                                                         |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| build-clone                           | hidden     |                                                                               |
| build-dev                             | hidden     |                                                                               |
| build-dist                            | hidden     |                                                                               |
| install                               | hidden     |                                                                               |
| install-dev                           | hidden     |                                                                               |
| behat                                 | hidden     |                                                                               |
| mjolnir                               | hidden     |                                                                               |
| setup-php-codesniffer                 | hidden     |                                                                               |
| setup-phpcs-pre-push                  | hidden     |                                                                               |
| regenerate-settings                   | hidden     |                                                                               |
| link-docroot                          | hidden     |                                                                               |
| download-prod-db                      | hidden     |                                                                               |
| import-prod-db                        | hidden     |                                                                               |
| backup-site                           | hidden     |                                                                               |
| backup-site-item                      | hidden     |                                                                               |
| copy-dist-resources                   | hidden     |                                                                               |
| copy-folder                           | hidden     |                                                                               |
| delete-dev                            | hidden     |                                                                               |
| delete-dist                           | hidden     |                                                                               |
| download-development-modules          | hidden     |                                                                               |
| download-platform                     | hidden     |                                                                               |
| enable-development-modules            | hidden     |                                                                               |
| enable-install-modules                | hidden     |                                                                               |
| enable-modules                        | hidden     |                                                                               |
| generate-development-makefile         | hidden     |                                                                               |
| install-build-dependencies            | hidden     |                                                                               |
| install-dev-dependencies              | hidden     |                                                                               |
| link-behat                            | hidden     |                                                                               |
| link-dev-resources                    | hidden     |                                                                               |
| make-dev                              | hidden     |                                                                               |
| make-dist                             | hidden     |                                                                               |
| quality-assurance                     | hidden     |                                                                               |
| rebuild-node-access                   | hidden     |                                                                               |
| restore-site                          | hidden     |                                                                               |
| restore-site-item                     | hidden     |                                                                               |
| setup-behat                           | hidden     |                                                                               |
| setup-files-directory                 | hidden     |                                                                               |
| unpack-platform                       | hidden     |                                                                               |
| update-htaccess                       | hidden     |                                                                               |
| wget-prod-db                          | hidden     |                                                                               |
| build-custom                          | hidden     |                                                                               |
| provision-stack                       | hidden     |                                                                               |
| create-tmp-dirs                       | hidden     |                                                                               |
| delete-deployment-group               | hidden     |                                                                               |
| delete-stack                          | hidden     |                                                                               |
| install-dist-dependencies             | hidden     |                                                                               |
| move-download-to-tmp-dir              | hidden     |                                                                               |
| run-stack                             | hidden     |                                                                               |
| setup-aws                             | hidden     |                                                                               |
| setup-deployment-group                | hidden     |                                                                               |
| check-for-default-settings-or-rebuild | hidden     |                                                                               |
| check-starterkit                      | hidden     |                                                                               |
| rebuild-dev                           | hidden     |                                                                               |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| docker                                                                                                                             |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| docker-compose-up                     | hidden     | Start docker project.                                                         |
| docker-compose-stop                   | hidden     | Stop docker project.                                                          |
| docker-compose-down                   | hidden     | Trash docker project.                                                         |
| docker-compose-backup                 | hidden     | Backup database.                                                              |
| docker-compose-restore                | hidden     | Restore database.                                                             |
| docker-check-mysql                    | hidden     | Check if mysql container exists.                                              |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| drush                                                                                                                              |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| drush-site-install                    | visible    | Install a site.                                                               |
| drush-generate-aliases                | visible    | Generate drush aliases for each subsite folder.                               |
| drush-enable-modules                  | hidden     | Enable a set of modules.                                                      |
| drush-create-files-dirs               | hidden     | Create site files directories.                                                |
| drush-sql-create                      | visible    | Create a database.                                                            |
| drush-sql-import                      | visible    | Import a database.                                                            |
| drush-sql-drop                        | visible    | Drop a database.                                                              |
| drush-sql-dump                        | visible    | Make a dump of database.                                                      |
| drush-enable-solr                     | visible    | Enable the solr module.                                                       |
| drush-make-no-core                    | hidden     | Make a file without core.                                                     |
| drush-generate-settings               | visible    | Generate the settings.php file.                                               |
| drush-registry-rebuild                | visible    | Perform a registry rebuild.                                                   |
| drush-registry-rebuild-dl             | visible    | Download drush registry-rebuild.                                              |
| drush-rebuild-node-access             | visible    | Rebuild the node access.                                                      |
| drush-file-ensure-htaccess            | visible    | Rebuild the node access.                                                      |
| drush-make-secure-all                 | visible    |                                                                               |
| drush-make-secure                     | visible    |                                                                               |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| general                                                                                                                            |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| package-download-unpack               | hidden     | Download a package and unpack it into location.                               |
| package-download                      | hidden     | Download package with curl.                                                   |
| package-unpack                        | hidden     | Unpack package with tar zxf.                                                  |
| folder-copy                           | hidden     | Copy a folder to a destination.                                               |
| folder-delete                         | hidden     | Delete a folder.                                                              |
| folder-unprotect                      | hidden     | Open up filesystem permissions on folder.                                     |
| reset-filesystem-permissions          | hidden     | Reset filesystem permissions.                                                 |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| github                                                                                                                             |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| github-init-target                    | visible    |                                                                               |
| github-init-auth                      | visible    |                                                                               |
| github-init-user                      | hidden     |                                                                               |
| github-init-team                      | hidden     |                                                                               |
| github-init-pass                      | hidden     |                                                                               |
| github-init-token                     | hidden     |                                                                               |
| github-init-owner                     | hidden     |                                                                               |
| github-init-repo                      | hidden     |                                                                               |
| github-create-release                 | visible    |                                                                               |
| github-create-release-assets          | visible    |                                                                               |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| toolkit                                                                                                                            |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| toolkit-initialize                    | visible    | Initializes a project for use of the toolkit.                                 |
| toolkit-link-binary                   | visible    | Provide project with toolkit binary at root level.                            |
| toolkit-generate-structure            | visible    | Create the lib directory structure.                                           |
| toolkit-generate-docs                 | visible    | Generate documentation md files for toolkit.                                  |
| toolkit-copy-templates                | visible    | Copies template files to your project for toolkit integration.                |
| toolkit-upgrade-starterkit            | visible    | Perform upgrade tasks for upgrading from 2.x to 3.x.                          |
| toolkit-composer-hook-phingcalls      | hidden     | Echo the composer hook phing targets for use in bash script.                  |
| toolkit-git-hook-phingcalls           | hidden     | Echo the git hook phing targets for use in bash script.                       |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| project                                                                                                                            |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| install-project-clean                 | visible    | Install NextEuropa site from scratch.                                         |
| install-project-clone                 | visible    | Install NextEuropa site with sanitized production data.                       |
| project-build-platform                | visible    | Build NextEuropa Platform code without version control.                       |
| project-build-subsite                 | visible    | Build NextEuropa Subsite code without version control (non-functional).       |
| project-build-theme                   | visible    | Build EC Europa theme without version control.                                |
| project-composer-install              | hidden     | Runs composer install.                                                        |
| project-database-download             | visible    | Download sanitized production database from archive.                          |
| project-database-import               | visible    | Import database for project with drush.                                       |
| project-modules-devel-en              | visible    | Enable development modules with drush.                                        |
| project-platform-delete               | visible    | Remove previous platform build..                                              |
| project-platform-devops-unpack        | visible    | Download and unpack fpfis resource package.                                   |
| project-platform-package-unpack       | visible    | Download and unpack platform deploy package.                                  |
| project-platform-set-htaccess         | visible    | Append htaccess config to root .htaccess.                                     |
| project-platform-set-version          | hidden     | Save the platform version used for builds.                                    |
| project-scratch-build                 | visible    | Delete previous build to start over clean.                                    |
| project-subsite-backup                | visible    | Backup site defined files from properties.                                    |
| project-subsite-backup-item           | hidden     | Backup site item from configuraton list.                                      |
| project-subsite-restore               | visible    | Restore site defined files from properties.                                   |
| project-subsite-restore-item          | hidden     | Restore site item from configuration list.                                    |
| project-subsite-setup-files           | visible    | Create files directories for subsite.                                         |
| project-modules-devel-make            | visible    | Makes the development resources with drush.                                   |
| project-modules-install-en            | visible    | Install list of modules to enable by default.                                 |
| project-platform-composer-dev         | visible    | Run composer install with dev on platform.                                    |
| project-platform-composer-no-dev      | visible    | Run composer install without dev on platform.                                 |
| project-platform-set-docroot          | visible    | Link the platform root to your docroot.                                       |
| project-rebuild-check                 | hidden     | Rebuild project if needed. (needs work)                                       |
| project-subsite-composer-no-dev       | visible    | Run composer install without dev on subsite.                                  |
| project-subsite-composer-dev          | visible    | Run composer install with dev on subsite.                                     |
| project-validate-properties           | visible    | Validate the build properties file.                                           |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| theme                                                                                                                              |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| build-theme-dev                       | visible    | Build EC Europa theme with version control.                                   |
| theme-europa-build                    | visible    | Build the EC europa theme with NPM.                                           |
| theme-europa-repo-clone               | visible    | Clone the Atomium and EC Europa repositories.                                 |
| theme-europa-create-symlinks          | visible    | Create symlinks to themes in lib for development.                             |
| theme-europa-download-extract         | visible    | Download and unpack the EC Europa theme.                                      |
| theme-europa-embed-ecl-assets         | visible    | Download and unpack the ECL assets for EC Europa theme.                       |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| platform                                                                                                                           |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| build-platform-dev                    | visible    | Build a local development version with a single platform profile.             |
| build-platform-dev-all                | visible    | Build a local development version with all platform profiles.                 |
| build-platform-dist                   | visible    | Build a single platform profile intended as a release package.                |
| build-platform-dist-all               | visible    | Build all platform profiles intended as a release package.                    |
| build-platform-test                   | visible    | Build a platform test package to test this release.                           |
| build-platform-copy-profile           | visible    | Copy single profile for distribution.                                         |
| build-platform-copy-profiles          | visible    | Copy all profiles for distribution.                                           |
| build-platform-copy-resources         | visible    | Copy platform resources for distribution.                                     |
| build-platform-delete                 | visible    | Build a platform test package to test this release.                           |
| build-platform-link-profiles          | visible    | Link platform profiles to lib folder for development.                         |
| build-platform-link-resources         | visible    | Link platform resources to lib folder for development.                        |
| build-platform-make-drupal            | visible    | Build the Drupal core codebase.                                               |
| build-platform-make-profile           | visible    | Makes single profile resources with drush.                                    |
| build-platform-make-profiles          | visible    | Makes all profile resources with drush.                                       |
| build-platform-type-dev               | visible    | Sets the type of build (dev or dist).                                         |
| build-platform-type-dist              | visible    | Sets the type of build (dev or dist).                                         |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| subsite                                                                                                                            |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| build-subsite-dev                     | visible    | Build a local development version of the site.                                |
| build-subsite-dist                    | visible    | Build a site intended as a release package.                                   |
| build-subsite-copy-resources          | visible    | Copy subsite resources for distribution.                                      |
| build-subsite-delete                  | visible    | Delete subsite build.                                                         |
| build-subsite-make-site               | visible    | Makes the subsite resources with drush.                                       |
| build-subsite-package                 | visible    | Build a subsite package in the releases folder.                               |
| build-subsite-release                 | visible    | Uploads the distribution package as release to github.                        |
| build-subsite-link-resources          | visible    | Link subsite resources to lib folder for development.                         |
| build-subsite-release-package         | visible    | Build a subsite release package for deployment.                               |
| build-subsite-test                    | visible    | Build a subsite test package to test this release.                            |
| build-subsite-type-dev                | visible    | Sets the type of build (dev or dist).                                         |
| build-subsite-type-dist               | visible    | Sets the type of build (dev or dist).                                         |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| test                                                                                                                               |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| test-run-phpcs                        | visible    | Refresh configuration and run phpcs review.                                   |
| test-run-qa                           | visible    | Refresh configuration and run qa review.                                      |
| build-project-test                    | hidden     |                                                                               |
| test-qa-exec                          | visible    |                                                                               |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| starterkit                                                                                                                         |
+---------------------------------------+------------+-------------------------------------------------------------------------------+
| help-starterkit                       | hidden     |                                                                               |
| help-build                            | hidden     | The main starterkit build file.                                               |
| help-boot                             | hidden     | Contains all import files needed for the starterkit to function.              |
| help-properties                       | hidden     | Build properties for configuration and setting of conditional properties.     |
| help-extensions                       | hidden     | Custom classes for extra tasks, conditions, etc.                              |
| help-directories                      | hidden     | Create needed directories to optimize builds.                                 |
| help-custom                           | hidden     |                                                                               |
| help-help                             | hidden     | Contains all help file imports.                                               |
| help-deprecated                       | hidden     | Contains a mapping of subsite-starterkit to toolkit targets.                  |
| help-docker                           | hidden     | Contains phing targets to manage docker containers. (experimental!).          |
| help-drush                            | hidden     | Contains drush helper targets that help manage your Drupal installation.      |
| help-general                          | hidden     | General helper targets that can be used by multiple projects.                 |
| help-github                           | hidden     | Contains phing targets for github.                                            |
| help-toolkit                          | hidden     |                                                                               |
| help-main                             | hidden     |                                                                               |
| help-project                          | hidden     | Targets shared by differenct project types.                                   |
| help-theme                            | hidden     | Builds themes like ec_europa and places files in correct location.            |
| help-platform                         | hidden     | Builds the platform if a profiles folder is detected.                         |
| help-subsite                          | hidden     | Builds subsites within the platform.                                          |
| help-test                             | hidden     | The test file contains different targets to ease the testing of your project. |
+---------------------------------------+------------+-------------------------------------------------------------------------------+

```
</details>

## Maintainers

This project is maintained by members of the Quality Assurance team who
review incoming pull requests for the NextEuropa project. The board on
which they operate can be found at [https://webgate.ec.europa.eu/CITnet/jira].

<details><summary><b>Toolkit maintainers</b></summary>

|Full name|Username|Department|Role|
|:---|:---|:---|:---|
|Alex Verbruggen|[verbruggenalex]|Quality Assurance|Maintainer + Contact for Devops & Platform|
|Joao Santos|[jonhy81]|Quality Assurance|Maintainer + Contact for Subsites|
</details>

## License

* [European Union Public License 1.1](LICENSE.md)

[https://webgate.ec.europa.eu/CITnet/jira]: https://webgate.ec.europa.eu/CITnet/jira/secure/RapidBoard.jspa?rapidView=581
[verbruggenalex]: https://github.com/verbruggenalex
[jonhy81]: https://github.com/jonhy81