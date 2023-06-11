# Python

---

* [List all packages](#62e87ccb-e577-437b-a131-77f4884666ee)
* [Show outdated package](#931a0cd7-4f63-439c-9f21-5d9dde36a09c)
* [Show details about a package](#4956f632-e96e-49ea-8b70-308331322aaa)
* [Install a specific version of a package](#2649f3be-2ae3-4dd4-88bd-9c91f6fdd511)
* [Upgrade a package](#41d6a4a3-2dcd-4162-b3e2-78dc7d2c766f)
* [Upgrade all packages (Windows only, must execute in Powershell)](#682fb3cc-99f7-49a0-adab-bc920b17a2e4)
* [Upgrade Pip, avoiding frequent errors](#85fdcca7-33b6-4dbb-b056-a2455a7111c5)

---




<div id="62e87ccb-e577-437b-a131-77f4884666ee">

## List all packages

</div>

    pip list
    ...or...
    pip freeze




<div id="931a0cd7-4f63-439c-9f21-5d9dde36a09c">

## Show outdated package

</div>

    pip list --outdated




<div id="4956f632-e96e-49ea-8b70-308331322aaa">

## Show details about a package

</div>

    pip show <package_name>




<div id="2649f3be-2ae3-4dd4-88bd-9c91f6fdd511">

## Install a specific version of a package

</div>

    pip install <package_name>==<version>




<div id="41d6a4a3-2dcd-4162-b3e2-78dc7d2c766f">

## Upgrade a package

</div>

    pip install --upgrade <package_name>




<div id="682fb3cc-99f7-49a0-adab-bc920b17a2e4">

## Upgrade all packages (Windows only, must execute in Powershell)

</div>

    pip freeze | %{$_.split('==')[0]} | %{pip install --upgrade $_}




<div id="85fdcca7-33b6-4dbb-b056-a2455a7111c5">

## Upgrade Pip, avoiding frequent errors

</div>

    python -m pip uninstall pip
    python -m ensurepip
    python -m pip install -U pip
