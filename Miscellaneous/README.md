# Miscellaneous
----------

## To download an entire collection from the Internet Archives

First, go to archive.org and do an Advanced Search.  In the 'Advanced Search returning JSON, XML, and more' section,
entire a search term that gets you the collection you want, and select 'identifier' in the 'Fields to return' box.
Set 'Number of results' to a number large enough to get all the items in the collection (maybe 5000 or so).
Select CSV as output type and click Search.  Open the file and remove all leading and trailing quotation marks from
each line.  Then, execute this command:

    wget -r -H -nc -np -nH --cut-dirs=1 -e robots=off -l1 -i ./search.csv -B "http://archive.org/download/"
