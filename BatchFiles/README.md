# Batch Files

---

* [Search for a string in all files in the current directory and display the names of the files that it is found in](#354c83b4-4daa-4e47-a175-19b878e5e773)

---




<div id="354c83b4-4daa-4e47-a175-19b878e5e773">

## Search for a string in all files in the current directory and display the names of the files that it is found in

</div>

    @echo off
    for %%a in (*.*) do find /n /i "<string_to_find>" < %%a > nul & if not errorlevel 1 echo %%a
