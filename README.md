# Tycho KNIME node archetype

[![Java CI with Maven](https://github.com/cooflydata/tycho-knime-node-archetype/actions/workflows/ci.yml/badge.svg)](https://github.com/cooflydata/tycho-knime-node-archetype/actions/workflows/ci.yml)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.597989.svg)](https://doi.org/10.5281/zenodo.597989)

Generates [KNIME](http://www.knime.org) workflow node skeleton repository with sample code.

This archetype was made because the instructions to create KNIME nodes at https://www.knime.com/developer-guide, requires interaction with Eclipse wizards. We wanted a way to start and perform node development from the command line and headless.
KNIME nodes are Eclipse plugins. The [Tycho](https://eclipse.org/tycho/) Maven plugin is used to build and handle dependencies of Eclipse plugins, so we use Tycho for KNIME node building.

The [Maven archetype](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html) will generate a multi-module project with the following structure:

* / - parent Maven project
* /plugin/ - code for KNIME node
* /tests/ - tests of KNIME node
* /feature/ - eclipse feature
* /p2/ - eclipse update site

The generated project offers:

* Usage from the command-line using maven or from Eclipse
* Integrates with normal Eclipse plugin development flow
* Integration with Continuous Integration builds on [GitHub Actions](https://docs.github.com/en/actions)
* Integration with [SonarCloud](https://sonarcloud.io) quality analysis via GitHub Actions
* Test workflows for end-2-end tests
* Test coverage
* Start KNIME from Eclipse to test nodes without having to release them in a update site

Items above are documented in the generated README.md file, the README also includes instructions for setup for a user, setup for a developer, creating a release and citing the software.

## Requirements

* Java ==17
* Maven >=3.0

The archetype is hosted on a [GitHub packages repository](https://github.com/orgs/cooflydata/packages?repo_name=tycho-knime-node-archetype).
Maven does not resolve to this GitHub packages repository by default so it must be added.

The [~/.m2/settings.xml](https://maven.apache.org/settings.html) should contain the following profile:
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                          https://maven.apache.org/xsd/settings-1.0.0.xsd">
    <profiles>
        <profile>
            <id>knime-archetype</id>
            <repositories>
                <repository>
                    <id>archetype</id>
                    <url>https://maven.pkg.github.com/cooflydata</url>
                </repository>
            </repositories>
        </profile>
    </profiles>
    <servers>
        <server>
            <id>github-cooflydata</id>
            <username>cooflydata</username>
            <password>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</password>
        </server>
    </servers>
</settings>
```

## Generate

The following command will generate a skeleton project
```sh
mvn archetype:generate -DarchetypeGroupId=com.github.cooflydata \
-DarchetypeArtifactId=tycho-knime-node-archetype \
-DarchetypeVersion=2.0.4 -P knime-archetype
```

The command will ask the following questions:

1. Enter the groupId
2. Enter the artifactId
3. Enter the name of the package under which your code will be created
4. Enter the version of your project, use `x.y.z-SNAPSHOT` format (for example `1.2.3-SNAPSHOT`), where x.y.z is [semantic versioning](http://semver.org/).
5. Enter the GitHub organization name or GitHub username
6. Enter the GitHub repository name
7. Enter the KNIME node name
8. Enter the vendor name
9. For branded property enter `Y` to enable the Coofly project branding like splash logo, node category and update site category or enter `N` to skip the branding.
9. Confirm

The skeleton has been generated in a sub-directory named after the artifactId in the current working directory.

The following steps are needed to get a ready to use project.

9. Change directory to generated code.
10. Make skeleton git aware, by running `git init`.
11. Fill in all placeholders (`[Enter ... here.]`) in

    * plugin/src/**/*.xml
    * feature/feature.xml

12. Commit all changes and push to GitHub
13. Optionally, setup Continuous Integration as described in the project README.md file.

Further instructions about generated project can be found in it's README.md file.

## Generate from inside KNIME SDK

1. Install Java 17
2. Install Eclipse for [RCP and RAP developers](ttps://www.eclipse.org/downloads/packages/installer)
3. Configure Java 17 inside Eclipse Window > Preferences > Java > Installed JREs
4. Register the archetype catalog which contains this archetype

      1. Goto Window > Preferences > Maven > Archetypes
      2. Add a remote catalog with `https://github.com/cooflydata/tycho-knime-node-archetype/raw/master/archetype-catalog.xml`

5. Create a new project based on archetype

      1. Goto File > New > Project ... > Maven
      2. Select Maven Project & press Next button
      3. Use default location & press Next button
      4. Select Catalog you added in step 4.2
      5. Select the archetype with artifact id `tycho-knime-node-archetype` & press Next button
      6. Fill in the form & press Finish button

Further instructions about generated project can be found in it's README.md file.

## New release

1. Adjust version in pom.xml & CHANGELOG.md & README.md & archetype-catalog.xml
2. Commit & push
3. Test archetype by running `mvn verify`
4. Create GitHub release
5. Deploy to GitHub packages, see Deploy chapter below

### Deploy

To deploy current version to GitHub Packages.

0. Create personal access token with write:packages scope
1. Create [~/.m2/settings.xml](https://docs.github.com/en/packages/guides/configuring-apache-maven-for-use-with-github-packages#authenticating-with-a-personal-access-token)
2. Run `mvn --batch-mode deploy`

## Attribution

The https://github.com/open-archetypes/tycho-eclipse-plugin-archetype was used as inspiration for this archetype.
Also https://github.com/knime-community/community-repository-template/ and https://github.com/vernalis/vernalis-knime-nodes/ where used as inspiration to upgrade to KNIME 5.1.
