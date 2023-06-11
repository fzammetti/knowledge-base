# NPM

---

* [To update all installed modules in the current directory](#ae608c3b-6a09-48da-9c44-b92fe3ca2ffd)
* [To update all global modules](#05bf870a-052d-4da2-89fe-ea5da8ddba85)
* [To list only top-level packages](#ec4ce048-0476-40e9-851d-0be48243dd73)
* [For some problems, clearing cache is necessary](#6b56ac26-55b4-4e07-9510-01addd7365e7)

---




<div id="ae608c3b-6a09-48da-9c44-b92fe3ca2ffd">

## To update all installed modules in the current directory

</div>

    npm update




<div id="05bf870a-052d-4da2-89fe-ea5da8ddba85">

## To update all global modules

</div>

    npm update -g




<div id="ec4ce048-0476-40e9-851d-0be48243dd73">

## To list only top-level packages

</div>

    npm list -g --depth=0

  Note: newer versions default ls to global, so the above is basically deprecated and you can instead simply use:

    npm ls -g

  Note the second: newer versions have deprecated -g, so instead now you should use:

    npm ls --location=global




<div id="6b56ac26-55b4-4e07-9510-01addd7365e7">

## For some problems, clearing cache is necessary

</div>

    npm --force cache clean
