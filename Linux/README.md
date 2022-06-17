# Linux

## How to verify the integrity of a directory full of JPG files and only show those that aren't uncorrupted:

    jpeginfo -c -v /<directory_name>/*.jpg | awk '$0 !~ /\[OK\]/'
