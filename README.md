# how to build it locally
 
 Java version :: openjdk version "1.8.0_292"
 Make sure to set JAVA_HOME as environmental variable and create/update the settings.xml file to have below code
 
 <settings>
  <pluginGroups>
    <pluginGroup>org.jenkins-ci.tools</pluginGroup>
  </pluginGroups>

  <profiles>
    <!-- Give access to Jenkins plugins -->
    <profile>
      <id>jenkins</id>
      <activation>
        <activeByDefault>true</activeByDefault> <!-- change this to false, if you don't like to have it on per default -->
      </activation>
      <repositories>
        <repository>
          <id>repo.jenkins-ci.org</id>
          <url>https://repo.jenkins-ci.org/public/</url>
        </repository>
      </repositories>
      <pluginRepositories>
        <pluginRepository>
          <id>repo.jenkins-ci.org</id>
          <url>https://repo.jenkins-ci.org/public/</url>
        </pluginRepository>
      </pluginRepositories>
    </profile>
  </profiles>
  <mirrors>
    <mirror>
      <id>repo.jenkins-ci.org</id>
      <url>https://repo.jenkins-ci.org/public/</url>
      <mirrorOf>m.g.o-public</mirrorOf>
    </mirror>
  </mirrors>
</settings>



# JavaScript Framework Library "bundle" plugins

This repository contains Jenkins HPI plugins that "externalize" common/shared JavaScript libraries
that can't be externalized via the [easy/recommended externalization process].

> __Note__: The [easy/recommended externalization process] was created after this repository was created.
> Before the new/easy process was created, all JavaScript libraries needed to be externalized via HPI plugins, which was a bit painful.
> This is the reason why you still see HPI plugins sub-modules here for some libraries that can now work via the
> [easy/recommended externalization process]. In each of these cases, the top level README.md files will indicate
> that the [easy/recommended externalization process] can now be used.

# Available Libs
See the README.md files for the different bundle plugins (sub-modules of this repo) for details on how to use them
e.g. 

* [Ace Editor](https://github.com/jenkinsci/js-libs/tree/master/ace-editor)  
* [Handlebars.js](https://github.com/jenkinsci/js-libs/tree/master/handlebars)
  
Other sub-modules in this repo (`momentjs` etc) contain plugin definitions for common/shared JavaScript libraries
that __can__ be externalized via the [easy/recommended externalization process].

> Also see __[sample plugins](https://github.com/jenkinsci/js-samples)__. 

[jenkins-js-modules]: https://github.com/tfennelly/jenkins-js-modules
[easy/recommended externalization process]: https://github.com/jenkinsci/js-samples/blob/master/step-04-externalize-libs/HOW-IT-WORKS.md#configure-node-build-to-externalize-dependencies
