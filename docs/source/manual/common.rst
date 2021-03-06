beadledom-common
################

Overview
--------

This project contains the tooling to create the service specific build information.

Download
--------

Download using Maven:

.. code-block:: xml

  <dependency>
      <groupId>com.cerner.beadledom</groupId>
      <artifactId>beadledom-common</artifactId>
      <version>[insert latest version]</version>
  </dependency>

Usage
-----

* Add the `git commit plugin <https://github.com/ktoso/maven-git-commit-id-plugin>`_ to the pom.xml

.. code-block:: xml

  <plugins/>
    ...
    <plugin/>
      <groupId/>pl.project13.maven</groupId/>
      <artifactId/>git-commit-id-plugin</artifactId/>
    </plugin/>
        ...
  </plugins/>

* Add the ``build-info.properties`` in the class path (for example in the project resources directory - ``src/main/resources/com/cerner/mypackage/build-info.properties``).

.. code-block:: properties

  //build-info.properties
  //Required Properties
  git.commit.id=${git.commit.id}
  project.artifactId=${project.artifactId}
  project.groupId=${project.groupId}
  project.version=${project.version}

  //Optional Properties
  project.build.date=${mvn.build.timestamp}

.. note::
  ``project.build.date`` is an optional property, that requires you to define a new property because maven prevents the maven.build.timestamp property from getting passed to the filtering resource.
  This work around is needed because of this issue: https://issues.apache.org/jira/browse/MRESOURCES-99.

.. code-block:: xml

  <properties>
     <mvn.build.timestamp>${maven.build.timestamp}</mvn.build.timestamp>
  <properties>

* Filter the resource directory - if the ``build-info.properties`` file is added to the resource directory.

.. code-block:: xml

  <build>
    ...
    <resources>
    ...
      <resource>
        <directory>src/main/resources</directory>
        <filtering>true</filtering>
      </resource>
      ...
    </resources>
    ...
  </build>

* Load the build info.

.. code-block:: java

  BuildInfo buildInfo = BuildInfo.load("/path/to/build-info.properties");

* Create the ServiceMetadata.

.. code-block:: java

  ServiceMetadata serviceMetadata = ServiceMetadata.create(buildInfo);
