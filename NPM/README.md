# NPM
----------

## To update all installed modules in the current directory:

    npm update

## To update all global modules:

    npm update -g

## To list only top-level packages:

    npm list -g --depth=0

  Note: newer versions default ls to global, so the above is basically deprecated and you can instead simply use:

    npm ls -g

  Note the second: newer versions have deprecated -g, so instead now you should use:

    npm ls --location=global

## For some problems, clearing cache is necessary:

    npm --force cache clean
