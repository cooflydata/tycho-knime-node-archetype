# Tycho Knime node archetype

[![Build Status Travis-CI](https://travis-ci.org/3D-e-Chem/tycho-knime-node-archetype.svg?branch=master)](https://travis-ci.org/3D-e-Chem/tycho-knime-node-archetype)
[![Build status AppVeyor](https://ci.appveyor.com/api/projects/status/70whq4bsdl0oq94m?svg=true)](https://ci.appveyor.com/project/3D-e-Chem/tycho-knime-node-archetype)
[![Download](https://api.bintray.com/packages/nlesc/tycho-knime-node-archetype/tycho-knime-node-archetype/images/download.svg) ](https://bintray.com/nlesc/tycho-knime-node-archetype/tycho-knime-node-archetype/_latestVersion)
[![DOI](https://zenodo.org/badge/59283343.svg)](https://zenodo.org/badge/latestdoi/59283343)

Generates [Knime](http://www.knime.org) workflow node skeleton repository with sample code.

This archetype was made because the instructions to create Knime nodes at https://tech.knime.org/developer-guide, requires interaction with Eclipse wizards. We wanted a way to start and perform node development from the command line and headless.
Knime nodes are Eclipse plugins. The [Tycho](https://eclipse.org/tycho/) Maven plugin is used to build and handle dependencies of Eclipse plugins, so we use Tycho for Knime node building.

The [Maven archetype](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html) will generate a multi-module project with the following structure:

* / - parent Maven project
* /plugin/ - code for Knime node
* /tests/ - tests of Knime node
* /feature/ - eclipse feature
* /p2/ - eclipse update site

## Requirements

* Java >=1.8
* Maven >=3.0

## Generate

1. Add catalog to [~/.m2/settings.xml](https://maven.apache.org/settings.html)
```
  <profiles>
    <profile>
      <id>knimearchetype</id>
      <repositories>
        <repository>
          <id>archetype</id>
          <url>https://dl.bintray.com/nlesc/tycho-knime-node-archetype</url>
        </repository>
      </repositories>
    </profile>
  </profiles>
```

2. Execute
```
mvn archetype:generate -DarchetypeGroupId=nl.esciencecenter \
-DarchetypeArtifactId=tycho-knime-node-archetype \
-DarchetypeVersion=1.4.1 -P knimearchetype
```
3. Enter the groupId
4. Enter the artifactId
5. Enter the name of the package under which your code will be created
6. Enter the version of your project, use `x.y.z-SNAPSHOT` format, where x.y.z is [semantic versioning](http://semver.org/).
7. Enter the Github organization name or Github username
8. Enter the Github repository name
9. Enter the Knime node name
10. Confirm
11. Change directory to generated code.
12. Make skeleton git aware, by running `git init`, `git add * .travis.yml .gitignore` and `git commit -m "Initial commit"`.
13. Fill in all placeholders (`[Enter ... here.]`) in

    * plugin/META-INF/MANIFEST.MF
    * plugin/src/**/*.xml
    * feature/feature.xml
    * p2/category.xml


Further instructions about generated project can be found in it's README.md file.

## New release

1. Adjust version in pom.xml
2. Update CHANGELOG.md & README.md
3. Commit & push
4. Create Github release
5. Deploy to Bintray, see Deploy chapter below

### Deploy

To deploy current version to Bintray.

1. Add bintray API key to [~/.m2/settings.xml](https://maven.apache.org/settings.html)

```
<server>
    <id>bintray-nlesc-tycho-knime-node-archetype</id>
    <username>************</username>
    <password>********************************</password>
</server>
```

2. Run `mvn deploy`

## Attribution

The https://github.com/open-archetypes/tycho-eclipse-plugin-archetype was used as inspiration for this archetype.
