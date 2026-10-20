# Miscellaneous

---

* [To download an entire collection from the Internet Archives](#878ae23c-4921-478b-a629-9f635313f7f2)
* [Fix QBitorrent search plugins install after an update](#f0ae10d1-5cde-43dc-b968-2932c8e2686d)

---




<div id="878ae23c-4921-478b-a629-9f635313f7f2">

## To download an entire collection from the Internet Archives

</div>

First, go to archive.org and do an Advanced Search.  In the 'Advanced Search returning JSON, XML, and more' section,
entire a search term that gets you the collection you want, and select 'identifier' in the 'Fields to return' box.
Set 'Number of results' to a number large enough to get all the items in the collection (maybe 5000 or so).
Select CSV as output type and click Search.  Open the file and remove all leading and trailing quotation marks from
each line.  Then, execute this command:

    wget -r -H -nc -np -nH --cut-dirs=1 -e robots=off -l1 -i ./search.csv -B "http://archive.org/download/"




<div id="f0ae10d1-5cde-43dc-b968-2932c8e2686d">

## Fix QBitorrent search plugins install after an update

</div>

Sometimes, after a QBitorrent update, you'll find that you can't install search plugins, and the list is empty.
You may also find that it seems to be looking for only LUA files rather than Python.  To address this, go to
%LOCALAPPDATA%\qBittorrent and delete the nova3 directory.  Restart QBitorrent and all should be right with the world.
