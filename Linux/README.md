# Linux

* How to verify the integrity of a directory full of JPG files and only show those that aren't uncorrupted:

  jpeginfo -c -v /some_directory/*.jpg | awk '$0 !~ /\[OK\]/'

