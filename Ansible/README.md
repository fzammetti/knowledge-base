# Ansible

---

* [Create a directory](#d7925673-cb08-4fb2-94df-c3444e6c1e88)
* [Unarchive a file](#319c6e3f-ab64-4340-a992-fb603daaf927)
* [Copy a file](#f242ae5d-ca1b-43d4-82d0-2ffd56cad85d)
* [Delete a directory](#5f3993f2-7651-498e-b2d2-f66fe528c896)
* [Download from Maven/Nexus repo](#b7a2e3e8-432c-42b2-ad10-1e23544123b3)
* [Execute a command](#1d96268e-c724-4a59-ba7e-2eb1d713a2f8)
* [Download file from SVN (unverified)](#4b2c9a64-8506-4d59-8080-d6fa760e1cd6)

---




<div id="d7925673-cb08-4fb2-94df-c3444e6c1e88">

## Create a directory

</div>

Remember to set appropriate rights and owner!

    - name: Create directory
      file:
        path: "<path>"
        mode: "0777"
        owner: "root"
        group: "root"
        state: "directory"




<div id="319c6e3f-ab64-4340-a992-fb603daaf927">

## Unarchive a file

</div>

Remember to set appropriate rights and owner!

    - name: Unarchive file
      unarchive:
        remote_src: true
        src: "<path>"
        dest: "<path>"
        mode: "0777"
        owner: "root"
        group: "root"




<div id="f242ae5d-ca1b-43d4-82d0-2ffd56cad85d">

## Copy a file

</div>

Remember to set appropriate rights and owner!

    - name: Copy file
      copy:
        remote_src: true
        src: "<path>"
        dest: "<path>"
        mode: "0777"
        owner: "root"
        group: "root"




<div id="5f3993f2-7651-498e-b2d2-f66fe528c896">

## Delete a directory

</div>

    - name: Delete directory
      file:
        path: "<path>"
        state: absent




<div id="b7a2e3e8-432c-42b2-ad10-1e23544123b3">

## Download from Maven/Nexus repo

</div>

Remember to set appropriate rights and owner!

    - name: Download from Maven/Nexus repo
      maven_artifact:
        repository_url: "URL"
        group_id: "group_id"
        artifact_id: "artifact_id"
        version: "version"
        classifier: "classifier"
        dest: "<path>"
        mode: "0777"
        owner: "root"
        group: "root"
        state: "present"




<div id="1d96268e-c724-4a59-ba7e-2eb1d713a2f8">

## Execute a command

</div>

    - name: Create Docker Netwokr
      command: docker network create -d bridge <network_name>
      ignore_errors: yes




<div id="4b2c9a64-8506-4d59-8080-d6fa760e1cd6">

## Download file from SVN (unverified)

</div>

    - name: Download file from SVN
      subversion:
        repo: https://<server>/<path>/<file>
        username: <username>
        password: <password>
        dest: /<path>/<filename>
        export: yes

