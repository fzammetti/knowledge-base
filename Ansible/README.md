# Ansible
----------

## Create a directory (remember to set appropriate rights and owner!)

    - name: Create directory
      file:
        path: "<path>"
        mode: "0777"
        owner: "root"
        group: "root"
        state: "directory"

## Unarchive a file (remember to set appropriate rights and owner!)

    - name: Unarchive file
      unarchive:
        remote_src: true
        src: "<path>"
        dest: "<path>"
        mode: "0777"
        owner: "root"
        group: "root"

## Copy a file (remember to set appropriate rights and owner!)

    - name: Copy file
      copy:
        remote_src: true
        src: "<path>"
        dest: "<path>"
        mode: "0777"
        owner: "root"
        group: "root"

## Delete a directory

    - name: Delete directory
      file:
        path: "<path>"
        state: absent

## Download from Maven/Nexus repo (remember to set appropriate rights and owner!)

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

## Execute a command

    - name: Create Docker Netwokr
      command: docker network create -d bridge <network_name>
      ignore_errors: yes

## Download file from SVN (unverified)

    - name: Download file from SVN
      subversion:
        repo: https://<server>/<path>/<file>
        username: <username>
        password: <password>
        dest: /<path>/<filename>
        export: yes

