[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.envimate/envimate-opensource-parent/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.envimate/envimate-opensource-parent)

# envimate-java-standards

Contains the java quality standards for envimate projects

##Usage
Replace the top section of your pom with the following:
```xml
<!--
  ~ Copyright (c) 2018 envimate GmbH - https://envimate.com/.
  ~
  ~ Licensed to the Apache Software Foundation (ASF) under one
  ~ or more contributor license agreements.  See the NOTICE file
  ~ distributed with this work for additional information
  ~ regarding copyright ownership.  The ASF licenses this file
  ~ to you under the Apache License, Version 2.0 (the
  ~ "License"); you may not use this file except in compliance
  ~ with the License.  You may obtain a copy of the License at
  ~
  ~   http://www.apache.org/licenses/LICENSE-2.0
  ~
  ~ Unless required by applicable law or agreed to in writing,
  ~ software distributed under the License is distributed on an
  ~ "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
  ~ KIND, either express or implied.  See the License for the
  ~ specific language governing permissions and limitations
  ~ under the License.
  -->

<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>com.envimate</groupId>
        <artifactId>envimate-opensource-parent</artifactId>
        <version>1.0.0</version>
    </parent>
```

##Checkstyle
Checkstyle is enabled after using this pom as parent in a maven project and executed before tests are run.

###Registering checkstyle suppressions
Add the following to your pom:
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
        ...
    <properties>
        ...
        <checkstyle.suppressions.location>
            ${project.basedir}/src/test/checkstyle/checkstyle-suppressions.xml
        </checkstyle.suppressions.location>
    </properties>
    ...
</project>
```
And create the suppression configuration file under `/src/test/checkstyle/checkstyle-suppressions.xml` with the 
contents of your choosing based on the following example:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
  ~ Copyright (c) 2018 envimate GmbH - https://envimate.com/.
  ~
  ~ Licensed to the Apache Software Foundation (ASF) under one
  ~ or more contributor license agreements.  See the NOTICE file
  ~ distributed with this work for additional information
  ~ regarding copyright ownership.  The ASF licenses this file
  ~ to you under the Apache License, Version 2.0 (the
  ~ "License"); you may not use this file except in compliance
  ~ with the License.  You may obtain a copy of the License at
  ~
  ~   http://www.apache.org/licenses/LICENSE-2.0
  ~
  ~ Unless required by applicable law or agreed to in writing,
  ~ software distributed under the License is distributed on an
  ~ "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
  ~ KIND, either express or implied.  See the License for the
  ~ specific language governing permissions and limitations
  ~ under the License.
  -->

<!DOCTYPE suppressions PUBLIC
        "-//Puppy Crawl//DTD Suppressions 1.1//EN"
        "http://www.puppycrawl.com/dtds/suppressions_1_1.dtd">
<suppressions>
    <suppress checks="VisibilityModifier" files=".*AggregatedValidationException.java" />
    <suppress checks="IllegalCatchCheck" files="NamedMethodSerializationCPMethod.java" />
    <suppress checks="IllegalCatchCheck" files="Deserializer.java" />
</suppressions>
```
###Intellij integration
Install the CheckStyle-IDEA plugin. Afterwards, open Settings -> Other Settings -> Checkstyle and add a new Checkstyle
configuration file, name it EnvimateOpenSource and use 
`https://bitbucket.org/envimate/envimate-opensource-parent/raw/master/checkstyle/checkstyle.xml` as a URL location.

##Registering spotbugs exclusions
Add the following to your pom:
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
        ...
    <properties>
        ...
        <spotbugs.excludeFilterFile>
            ${project.basedir}/src/test/spotbugs/spotbugs-exclude.xml
        </spotbugs.excludeFilterFile>
    </properties>
    ...
</project>
```
And create the suppression configuration file under `/src/test/spotbugs/spotbugs-exclude.xml` with the contents of your
choice based on the following example:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
  ~ Copyright (c) 2018 envimate GmbH - https://envimate.com/.
  ~
  ~ Licensed to the Apache Software Foundation (ASF) under one
  ~ or more contributor license agreements.  See the NOTICE file
  ~ distributed with this work for additional information
  ~ regarding copyright ownership.  The ASF licenses this file
  ~ to you under the Apache License, Version 2.0 (the
  ~ "License"); you may not use this file except in compliance
  ~ with the License.  You may obtain a copy of the License at
  ~
  ~   http://www.apache.org/licenses/LICENSE-2.0
  ~
  ~ Unless required by applicable law or agreed to in writing,
  ~ software distributed under the License is distributed on an
  ~ "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
  ~ KIND, either express or implied.  See the License for the
  ~ specific language governing permissions and limitations
  ~ under the License.
  -->

<FindBugsFilter
		xmlns="https://github.com/spotbugs/filter/3.0.0"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="https://github.com/spotbugs/filter/3.0.0 https://raw.githubusercontent.com/spotbugs/spotbugs/3.1.0/spotbugs/etc/findbugsfilter.xsd">
</FindBugsFilter>
```
##Skipping tests
Just run the maven command with `-DskipTests=true`.

##Releasing a version to maven central
The problem with releasing to maven central using a CI/CD pipeline is complicated, since both, the nexus staging account
and the gpg key must be kept private. If a very bad person gains access to one or both of these credentials users of
our open source products are in grave danger (https://youtu.be/ES9hWkn_WBA?t=27m29s). A bitbucket pipeline can output 
secure environmental variables with some neat tricks. Since we would need to store both valuable credentials as such,
that operation can only be done manually outside the bitbucket cloud environment:

* Ensure the gpg private key with the id `0xACB977BA5C6D5C6E` installed in the gpg key store.
* Ensure the maven settings.xml file contains the following section:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
	...
	<servers>
		<server>
			<id>envimate.opensource.sonatype.staging</id>
			<username>the username of the sonatype nexus staging account</username>
			<password>{the encrypted password of the Sonatype nexus staging account created using mvn --encrypt-password}</password>
		</server>
	</servers>
	...
</settings>
```
* ensure the project version has been set to the appropriate value
* run the following command to test the project:
```bash
mvn clean verify
```
* run the following command to release the project in case the tests passed:
```bash
mvn -P deployToMavenCentral -DskipTests=true clean deploy
```
