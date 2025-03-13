---
title: "PHP Composer"
date: "2020-02-01"
---

Repository: [https://packagist.org/](https://packagist.org/)

## What is composer?

Composer, nowadays (2016 at least), is a standard for PHP content creation.  
Composer is like npm for node. Why use composer?

1. Ensure all developers run the correct package versions.
2. Easy for all developers to set up and running.
3. Easy auto-loading

## Getting Started

- Download and install composer from [https://getcomposer.org/](https://getcomposer.org/)
- You can make composer available globally or just in your project root.
- Require your first package

For Windows users just download the installer and run it, is quite self-explanatory.  
For mac and Linux users you will find info on the composer website.

Now when you install composer you can choose to install it globally or just for your project, I personally suggest having a global installation, since is pretty much an integral part of modern (2016) PHP development.

## Composer.json / Composer.lock

Composer use 2 files to control dependencies: composer.json and composer.lock but those 2 files are not the same!  
First, you create the JSON file, inside you will put all the dependency you need, but you will not refer to the exact version of it, once you have the list of the dependencies you need, they will be installed and a composer.lock file is created, this time with all the versions you are using (since you don't specify a version, composer will download the latest version).  
So in the "composer.lock" we will know exactly the currently installed version of a dependency.

But you don't need to write manually inside the JSON what you need you use composer to get the job done automatically we will see that later. (it will also add folder and files if needed)

## How do you find packages?

You can find packages and repository at [https://packagist.org/](https://packagist.org/)

## Installing a package

Go to the package you want to install, find the code you need:

```
$composer require module_name
```

copy that code and go to the command line (i imagine you do it from the folder where you want to create the package). It will create the JSON file and the LOCK file, and different folders.

## Package names

The package name consists of a vendor name and the project's name. Often these will be identical - the vendor name only exists to prevent naming clashes. For example, it would allow two different people to create a library named `json`. One might be named `igorw/json` while the other might be `seldaek/json`.

Read more about publishing packages and package naming [here](https://getcomposer.org/doc/02-libraries.md). (Note that you can also specify "platform packages" as dependencies, allowing you to require certain versions of server software. See [platform packages](https://getcomposer.org/doc/01-basic-usage.md#platform-packages) below.)

## Package version constraints

In our example, we are requesting the Monolog package with the version constraint [`1.0.*`](https://semver.mwl.be/#?package=monolog%2Fmonolog&version=1.0.*). This means any version in the `1.0` development branch, or any version that is greater than or equal to 1.0 and less than 1.1 (`>=1.0 <1.1`).

Please read [versions](https://getcomposer.org/doc/articles/versions.md) for more in-depth information on versions, how versions relate to each other, and on version constraints.

## Composer install

"composer install" will look check if a composer.lock file exists and if not then it runs "composer update" that automatically creates the lock file. If the lock file exists, then install the declared dependencies from there. So when you are importing a project you don't need to put all the code because it will be automatically installed ruinning this code

## Composer update

Composer Update will install the latest version for each module in your project (since using composer.lock prevents from that) . This is equivalent to deleting the `composer.lock` file and running `install` again.

 `php composer.phar update` 

If you only want to install or update one dependency, you can whitelist them:

```
php composer.phar update monolog/monolog [...]
```

## Use modules in your code

I need to investigate further on this subject

## Using create-project

You can use composer to create new projects from an existing package. This is the equivalent of doing a git clone/svn checkout followd by a `composer-install` of the vendors.

This is useful for different reasons:

1. You can deploy application packages.
2. You can check out any package and start developing on patchers for examples
3. Projects with multiple developers can use this feature to bootstrap the initial application for development.

To create a new project using Composer you can use the `create-project` command. Pass it a package name, and the directory to create the project in. You can also provide a version as third argument, otherwise the latest version is used.

If the directory does not exist, it will be created during installation.

It is al possible to run the command without params in a directory with an existing `composer.json` file to bootstrap a project.

You can look for packages in packagist.org

### Options

- **\--stability (-s):** Minimum stability of package. Defaults to `stable`.
- **\--prefer-source:** Install packages from `source` when available.
- **\--prefer-dist:** Install packages from `dist` when available.
- **\--repository:** Provide a custom repository to search for the package, which will be used instead of packagist. Can be either an HTTP URL pointing to a `composer` repository, a path to a local `packages.json` file, or a JSON string which similar to what the [repositories](https://getcomposer.org/doc/04-schema.md#repositories) key accepts. You can use this multiple times to configure multiple repositories.
- **\--add-repository:** Add the custom repository in the composer.json. If a lock file is present it will be deleted and an update will be run instead of install.
- **\--dev:** Install packages listed in `require-dev`.
- **\--no-dev:** Disables installation of require-dev packages.
- **\--no-scripts:** Disables the execution of the scripts defined in the root package.
- **\--no-progress:** Removes the progress display that can mess with some terminals or scripts which don't handle backspace characters.
- **\--no-secure-http:** Disable the secure-http config option temporarily while installing the root package. Use at your own risk. Using this flag is a bad idea.
- **\--keep-vcs:** Skip the deletion of the VCS metadata for the created project. This is mostly useful if you run the command in non-interactive mode.
- **\--remove-vcs:** Force-remove the VCS metadata without prompting.
- **\--no-install:** Disables installation of the vendors.
- **\--ignore-platform-reqs:** ignore all platform requirements (`php`, `hhvm`, `lib-*` and `ext-*`) and force the installation even if the local machine does not fulfill these. See also the [`platform`](https://getcomposer.org/doc/06-config.md#platform) config option.
- **\--ignore-platform-req:** ignore a specific platform requirement(`php`, `hhvm`, `lib-*` and `ext-*`) and force the installation even if the local machine does not fulfill it.
