# Jinitialize

Modular and extensible automation scripts in php. Based on [Symfony\Console](https://symfony.com/doc/current/components/console.html) component. This package was created to allow php programmers to create robust setup scripts for their projects that can be ran almost anywhere given the wide spread of php installations.

```
php jinitialize php-package
```
![php-package procedure script](https://i.imgur.com/2ywPHbh.png)

# Introduction

As the number of tools you use for your projects increases, so does the time required to set everything up. These tasks include but are not limited to:
* Setting up version control
* Setting up your testing tools
* Setting up compilers (javascript with babel etc.)
* Setting up automation scripts (watching file changes and running tests etc.)
* Setting up local site to try out your web application
* Setting up configuration for your favorite packages and libraries

Even though more simplistic scripts can be used in even wider variety of environments, some of these tasks quickly become messy and lead to project specific installation scripts and duplicated code. Because of these reasons, it is ideal to break down the complex setup scripts into more modular, reusable components.

# Installation
```
composer create-project nonetallt-jinitialize [my-project]
```

# Commands

Commands are called by their command signature that always starts with the name of the plugin that command belongs to as command namespace. For example, to call a "create" command defined in the "project" plugin, you would enter the following command:
```
php jinitialize project:create
```

You can pass [arguments and options](https://symfony.com/doc/current/components/console/console_arguments.html) to the command by appending them to the command call:
```
php jinitialize [command] [argument] [--option]
```

# Procedures
Procedures are series of commands defined in json format. They can be executed same way as commands and allow you to pass results and information from executed commands to other commands that are part of the procedure.

```json
{
    "example-procedure": {
        "description": "This is an example procedure",
        "commands": [
            "project:create",
            "project:folder newFolder"
        ]
    }
}
```

Example procedure for creating a php library package using three plugins; project, git and github.
```json
{
"php-package": {
        "description": "Create a new generic php package.",
        "commands": [ 
            "project:create --path=[PACKAGES_DIRECTORY]",
            "project:folder src",
            "project:folder tests",
            "project:folder tests/Unit",
            "project:folder tests/Feature",
            "project:folder tests/input",
            "project:folder tests/expected",
            "project:folder tests/output",
            "project:copy [STUBS_FOLDER]/phpunit.xml phpunit.xml",
            "project:copy [STUBS_FOLDER]/composer.stub.json composer.json --env --exported",
            "git:init [project:path]",
            "github:authenticate [GIT_USER] [GIT_PASSWORD]",
            "github:create-repository [project:name] --private",
            "github:create-webhook [project:name] --name=packagist",
            "git:set-remote [github:ssh_url]",
            "git:ignore tags/",
            "git:ignore vendor/",
            "git:ignore tests/output",
            "core:shell 'ctags -f [project:path]/tags -R --fields=+laimS --languages=php --exclude=.git'"
        ]
    },
}
```

# Settings
This project uses the [dotenv](https://github.com/vlucas/phpdotenv) library to easily load user defined options. Create a file called .env in the project root directory or create one from the .env.example file:

```
cp .env.example .env
```

* **JINITIALIZE_PLACEHOLDER_FORMAT**, the format used for replacing placeholder values in procedures, default: [$]

Jinitialize plugins can request default options for your convenience. A project plugin could for example request you to define **DEFAULT_PROJECTS_FOLDER** to suggest a reasonable default path when creating new projects. This way you don't have to type the full path each time.

```
DEFAULT_PROJECTS_FOLDER=/home/nonetallt/Code/packages
```

# Plugins

### List of plugins
![list of plugins: https://nonetallt.com/jinitialize/plugins](https://nonetallt.com/jinitialize/plugins/image)

### Installing plugins
Any packages that you require in your jinitialize project that define the "jinitialize-plugin" section in their extra section will be autopublished to bootstrap/cache/plugins.php. Installing a new plugin for your project is as simple as requiring it:

```
composer require nonetallt/jinitialize-plugin-project
```

You can use either composer or the search command to look for packages
```
composer search jinitialize-plugin-

php jinitialize core:search-plugins
```

### Creating your own plugins
Check out the [jinitialize-plugin](https://github.com/nonetallt/jinitialize-plugin) boilerplate project  for more information on how to create your own plugin packages.
