# Python
--------

## List all packages

    pip list
    ...or...
    pip freeze


## Show outdated package

    pip list --outdated

## Show details about a package

    pip show <package_name>

## Install a specific version of a package

    pip install <package_name>==<version>

## Upgrade a package

    pip install --upgrade <package_name>

## Upgrade all packages (Windows only, must execute in Powershell)

    pip freeze | %{$_.split('==')[0]} | %{pip install --upgrade $_}
