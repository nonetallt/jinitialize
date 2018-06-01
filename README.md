# jinitialize
Modular and extensible automation scripts in php. Based on [Symfony\Console](https://symfony.com/doc/current/components/console.html) component. This package was mainly created to allow php programmers to create robust setup scripts for their projects that can be ran almost anywhere given the wide spread of php installations on web servers.

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

# Installing plugins
Any packages that you require in your jinitialize project that define the "jinitialize-plugin" section in their extra section will be autopublished to bootstrap/cache/plugins.php. Installing a new plugin for your project is as simple as requiring it:

```
composer require nonetallt/jinitialize-plugin-project
```

You can use either composer or the search command to look for packages
```
composer search jinitialize-plugin-

php jinitialize core:search-plugins
```

# Creating your own plugins
Check out the [jinitialize-plugin](https://github.com/nonetallt/jinitialize-plugin) boilerplate project  for more information on how to create your own plugin packages.
