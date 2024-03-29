# Maven

---

* [Install JAR into local file repository so it�s available in POM](#0d290369-f17c-4f39-bef4-2a3c4e1e0c86)
* [Use a JAR from the file system WITHOUT installing it into local repo](#0068c45c-e994-4821-8343-b00af1d3c687)
* [Execute an arbitrary OS-level command from Maven](#582232c5-c356-4c15-bd72-9a63c3c411fb)

---




<div id="0d290369-f17c-4f39-bef4-2a3c4e1e0c86">

## Install JAR into local file repository so it�s available in POM

</div>

    mvn install:install-file -Dfile="<absolute_path_to_jar" -DgroupId=<group_id> -DartifactId=<artifact_id> -Dversion=<version> -Dpackaging=jar`




<div id="0068c45c-e994-4821-8343-b00af1d3c687">

## Use a JAR from the file system WITHOUT installing it into local repo

</div>

Replace values surrounded by question marks with the appropriate real values

    <!-- Add Dependency -->
    <dependency>
      <groupId>???_GROUP_ID_???</groupId>
      <artifactId>???_ARTIFACT_ID_???</artifactId>
      <version>???_VERSION_???</version>
      <scope>system</scope>
      <systemPath>???_ABSOLUTE_PATH_TO_JAR_???</systemPath>
    </dependency>

    <!-- Copy file into build directory (add to copy-rename-maven-plugin section) -->
    <execution>
      <id>copy-file</id>
      <phase>prepare-package</phase>
      <goals>
        <goal>copy</goal>
      </goals>
      <configuration>
        <fileSets>
          <fileSet>
            <sourceFile>???_ABSOLUTE_PATH_TO_JAR_???</sourceFile>
            <destinationFile>
              ${project.build.directory}/${project.build.finalName}/WEB-INF/lib/???_JAR_NAME_???
            </destinationFile>
          </fileSet>
        </fileSets>
      </configuration>
    </execution>




<div id="582232c5-c356-4c15-bd72-9a63c3c411fb">

## Execute an arbitrary OS-level command from Maven

</div>

This config can be added to a POM to execute any command at the OS level you want.  The example shows executing a script.  Keep in mind however that doing this may make your POM not platform-agnostic, so use with caution!
To execute arbitrary OS-level commands:

    <plugin>
      <artifactId>exec-maven-plugin</artifactId>
      <groupId>org.codehaus.mojo</groupId>
      <executions>
        <execution>
          <id>Version Calculation</id>
          <phase>generate-sources</phase>
          <goals>
            <goal>exec</goal>
          </goals>
          <configuration>
            <executable>${basedir}/scripts/calculate-version.sh</executable>
          </configuration>
        </execution>
      </executions>
    </plugin>
