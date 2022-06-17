# Batch Files
----------

## Search for a string in all files in the current directory and display the names of the files that it is found in:

    @echo off
    for %%a in (*.*) do find /n /i "<string_to_find>" < %%a > nul & if not errorlevel 1 echo %%a
