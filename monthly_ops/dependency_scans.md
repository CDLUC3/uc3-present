# Merritt Dependency Scan Review

> [!NOTE]  
> Review every month.  Create new issues to address any vulnerabilities that are found.

_A meeting is held once per month to review this information._

## Java

- https://github.com/CDLUC3/mrt-core2/security/dependabot
- https://github.com/CDLUC3/mrt-cloud/security/dependabot
- https://github.com/CDLUC3/mrt-ingest/security/dependabot
- https://github.com/CDLUC3/mrt-store/security/dependabot
- https://github.com/CDLUC3/mrt-inventory/security/dependabot
- https://github.com/CDLUC3/mrt-audit/security/dependabot
- https://github.com/CDLUC3/mrt-replic/security/dependabot

## Java and Ruby
- https://github.com/CDLUC3/mrt-zk/security/dependabot

## Ruby
- https://github.com/CDLUC3/mrt-dashboard/security/dependabot
- https://github.com/CDLUC3/mrt-admin-lambda/security/dependabot
- https://github.com/CDLUC3/uc3-ssm/security/dependabot
- https://github.com/CDLUC3/merritt-docker/security/dependabot
- https://github.com/CDLUC3/mrt-atom/security/dependabot
- https://github.com/CDLUC3/mrt-cron/security/dependabot
- https://github.com/CDLUC3/mrt-integ-tests/security/dependabot
- https://github.com/CDLUC3/s3-sinatra/security/dependabot

## Tag Reports
- [mrt-admin-lambda](https://merritt.uc3dev.cdlib.org/?file=/reports/mrt-admin-lambda.md)
- [mrt-ingest](https://merritt.uc3dev.cdlib.org/?file=/reports/mrt-ingest.md)
- [mrt-inventory](https://merritt.uc3dev.cdlib.org/?file=/reports/mrt-inventory.md)
- [mrt-store](https://merritt.uc3dev.cdlib.org/?file=/reports/mrt-store.md)
- [mrt-audit](https://merritt.uc3dev.cdlib.org/?file=/reports/mrt-audit.md)
- [mrt-replic](https://merritt.uc3dev.cdlib.org/?file=/reports/mrt-replic.md)
- [mrt-dashboard](https://merritt.uc3dev.cdlib.org/?file=/reports/mrt-dashboard.md)
---

## 3rd Party Library Feature Review (beyond vulnerabilities)

### Apache Tika 
> [!NOTE]  
> Review every 6mos.  Last review: 2024-07

- Determine if should be updated to support new mime types
- [Merritt Version in Use](https://github.com/search?q=repo%3ACDLUC3%2Fmrt-core2+tika+language%3A%22Maven+POM%22&type=code&l=Maven+POM) 
- [Available Tika Versions](https://tika.apache.org/)

### Other Jars to Review
> [!NOTE]  
> Review every 3mos.  Last review: Report generated 2024-07, not reviewed

- [Jars in Use - BOM](https://github.com/CDLUC3/mrt-core2/blob/main/reflect/pom.xml)
- From mrt-core2/reflect, run `mvn versions:display-dependency-updates`

```
[INFO] The following dependencies in Dependency Management have newer versions:
[INFO]   asm:asm ....................................... 3.1 -> 20041228.180559
[INFO]   co.elastic.logging:ecs-logging-core ................... 1.5.0 -> 1.6.0
[INFO]   co.elastic.logging:log4j2-ecs-layout .................. 1.5.0 -> 1.6.0
[INFO]   com.amazonaws:aws-java-sdk-core ................. 1.12.261 -> 1.12.762
[INFO]   com.amazonaws:aws-java-sdk-kms .................. 1.12.261 -> 1.12.762
[INFO]   com.amazonaws:aws-java-sdk-s3 ................... 1.12.261 -> 1.12.762
[INFO]   com.amazonaws:aws-java-sdk-ssm .................. 1.12.190 -> 1.12.762
[INFO]   com.box:box-java-sdk ................................. 4.0.1 -> 4.11.1
[INFO]   com.fasterxml.jackson.core:jackson-core ............. 2.15.2 -> 2.17.2
[INFO]   com.fasterxml.jackson.core:jackson-databind ......... 2.15.2 -> 2.17.2
[INFO]   com.google.code.gson:gson ............................ 2.8.9 -> 2.11.0
[INFO]   com.google.guava:guava ...................... 32.1.2-jre -> 33.2.1-jre
[INFO]   com.zaxxer:HikariCP ................................... 4.0.3 -> 5.1.0
[INFO]   commons-codec:commons-codec ............................ 1.3 -> 1.17.1
[INFO]   commons-io:commons-io .................................. 2.7 -> 2.16.1
[INFO]   commons-logging:commons-logging ....................... 1.1.1 -> 1.3.3
[INFO]   javax.mail:mail ................................... 1.4.1 -> 1.5.0-b01
[INFO]   javax.servlet:servlet-api ......................... 2.5 -> 3.0-alpha-1
[INFO]   javax.ws.rs:javax.ws.rs-api ........................... 2.0.1 -> 2.1.1
[INFO]   javax.xml.bind:jaxb-api .................. 2.3.1 -> 2.4.0-b180830.0359
[INFO]   jaxen:jaxen ........................................... 1.2.0 -> 2.0.0
[INFO]   junit:junit ......................................... 4.13.1 -> 4.13.2
[INFO]   mysql:mysql-connector-java .......................... 8.0.28 -> 8.0.33
[INFO]   net.sf.opencsv:opencsv .................................... 2.0 -> 2.3
[INFO]   net.sf.saxon:Saxon-HE ............................... 9.8.0-14 -> 12.5
[INFO]   org.apache.ant:ant ................................ 1.10.13 -> 1.10.14
[INFO]   org.apache.commons:commons-compress ................. 1.26.0 -> 1.26.2
[INFO]   org.apache.commons:commons-email ........................ 1.5 -> 1.6.0
[INFO]   org.apache.commons:commons-lang3 ....................... 3.1 -> 3.15.0
[INFO]   org.apache.commons:commons-text ........................ 1.3 -> 1.12.0
[INFO]   org.apache.httpcomponents:httpclient ................ 4.5.13 -> 4.5.14
[INFO]   org.apache.httpcomponents:httpcore .................. 4.4.14 -> 4.4.16
[INFO]   org.apache.httpcomponents:httpmime .................. 4.5.13 -> 4.5.14
[INFO]   org.apache.james:apache-mime4j ....................... 0.8.9 -> 0.8.11
[INFO]   org.apache.logging.log4j:log4j-api ............. 2.20.0 -> 3.0.0-beta2
[INFO]   org.apache.logging.log4j:log4j-core ............ 2.20.0 -> 3.0.0-beta2
[INFO]   org.apache.tika:tika-core ....................... 2.9.2 -> 3.0.0-BETA2
[INFO]   org.apache.zookeeper:zookeeper ........................ 3.8.4 -> 3.9.2
[INFO]   org.apache.zookeeper:zookeeper-jute ................... 3.8.4 -> 3.9.2
[INFO]   org.glassfish.jersey.containers:jersey-container-servlet ...
[INFO]                                                         2.40 -> 4.0.0-M1
[INFO]   org.glassfish.jersey.core:jersey-client ............. 2.40 -> 4.0.0-M1
[INFO]   org.glassfish.jersey.core:jersey-server ............. 2.40 -> 4.0.0-M1
[INFO]   org.glassfish.jersey.inject:jersey-hk2 .............. 2.40 -> 4.0.0-M1
[INFO]   org.glassfish.jersey.media:jersey-media-multipart ... 2.40 -> 4.0.0-M1
[INFO]   org.slf4j:slf4j-api ........................... 2.0.10 -> 2.1.0-alpha1
[INFO]   org.slf4j:slf4j-simple ........................ 2.0.10 -> 2.1.0-alpha1
[INFO]   org.yaml:snakeyaml ........................................ 2.0 -> 2.2
[INFO]   xml-apis:xml-apis .................................... 1.4.01 -> 2.0.2
```

---

## System Software

_System Software that is unique to Merritt.  This page is designed to document how were will review and evaluate the status of our active versions._

### MySQL
MySQL versions are evaluated across CDL.  

### Zookeeper  
> [!NOTE]  
> Review every 3mos.  Last review: 2024-07

- [Available Zookeeper Versions](https://zookeeper.apache.org/releases.html)
- [Merritt Client Version in Use](https://github.com/search?q=repo%3ACDLUC3%2Fmrt-core2+zookeeper+language%3A%22Maven+POM%22&type=code&l=Maven+POM)
- [Merritt Server Version in Use](#)
- Mailing lists for Zookeeper (https://zookeeper.apache.org/lists.html)

---

### Java Version
> [!CAUTION]  
> Not yet captured on DevOps tracking chart. Review every 6os.  Last review: 2024-07

- We are running Amazon Corretto distributions.  On Mac, we use homebrew provided openjdk.
- [Compiler Compatibility Version](https://github.com/search?q=repo%3ACDLUC3%2Fmrt-core2%20maven.compiler.release&type=code)
  - [Upgrade to Java 21](https://github.com/CDLUC3/mrt-doc/issues/1898) 
- [JRE Version](#)
  - The Java JRE is used on dev, stage and prod boxes 
- [SDK Version](#)
  - The Java SDK is used on development machines, and CI/CD machines.
  - The use of the Java SDK on a Stage or Production box should be discussed.

_Note that D2D also uses Java, but there is no interdependencies between our Java versions._

- https://endoflife.date/amazon-corretto
- 11: 9/2027

### Tomcat
> [!NOTE]  
> Review every 6os.  Last review: 2024-07

- https://endoflife.date/tomcat
- 9: not specified (requires java 8)
- 10.1: not specified (requires java 11)
- Mailing lists for Tomcat (https://tomcat.apache.org/lists.html)
- [Docker Stack Version](https://github.com/CDLUC3/merritt-docker/blob/main/mrt-inttest-services/merritt-tomcat/Dockerfile)

```
bash /dpr2/tomcat/bin/catalina.sh version
```
- Available: 9 and 10
- Note: Tomcat is manually installed at /dpr2/local (no package management)

---

### CI/CD

#### Jenkins
- Hopefully we depecate Jenkins in favor of AWS CodeBuild
- Mailing lists for Jenkins (https://www.jenkins.io/mailing-lists/)

#### AWS Code Build
- So far, we are using the default versions of Java SDK, Maven and docker that are provided by default in AWS Code Build.  No specific versions have been configured.

---

### Maven
> [!NOTE]  
> Review every 6mos.  Last review: TBD.
> The next review should occur when DEV boxes migrate to AL2023

- Maven is used on development machines, and CI/CD machines.
- The use of maven on a Stage or Production box should be discussed.
- Mailing lists for Maven (https://maven.apache.org/mailing-lists.html)
- [Maven Version](#)
- [Docker Stack Veersion](https://github.com/CDLUC3/merritt-docker/blob/main/mrt-inttest-services/merritt-maven/Dockerfile)

---

### Maven Plugins
> [!NOTE]  
> Review every 6mos.  Last review: TBD.
> The next review should occur when DEV boxes migrate to AL2023

- [Registry of Plugins Used by Merritt](https://github.com/CDLUC3/mrt-core2/blob/main/parprop/pom.xml)
- [docker-maven-plugin](https://github.com/fabric8io/docker-maven-plugin/releases)
- [jacoco-maven-plugin](https://github.com/jacoco/jacoco/releases)
- [maven-assembly-plugin](https://github.com/apache/maven-assembly-plugin/tags)
- [maven-compiler-plugin](https://github.com/apache/maven-compiler-plugin/releases)
- [maven-surefire-plugin](https://github.com/apache/maven-surefire/releases)
- [maven-failsafe-plugin](https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-failsafe-plugin)
- [maven-javadoc-plugin](https://mvnrepository.com/artifact/org.apache.maven.plugins/maven-javadoc-plugin)

---

### Docker

> [!CAUTION]  
> Not yet captured on DevOps tracking chart. Review every 6os.  Last review: TBD
> Next review should take place when migrating DEV boxes to AL2023

- Docker should be installed on DEV boxes and on CI/CD boxes
- [Docker Version](#)
- [Docker Compose Version](#)

--- 

## Ruby

> [!NOTE]  
> Review every 3mos.  Last review: 2024-07

### Current Merritt Version
- [Ruby Version](#)
- Rails Version: [6.1.x](https://github.com/CDLUC3/mrt-dashboard/blob/main/Gemfile.lock)
- No posted 4/2026
- [Ruby EOL](https://endoflife.date/ruby)
- [Rails EOL](https://endoflife.date/rails)

---
## JQuery

> [!NOTE]  
> Review every 12mos.  Last review: 2024-07

- https://endoflife.date/jquery
- https://stackoverflow.com/questions/41605120/what-is-the-difference-between-jquery-version-1-version-2-and-version-3

### JQuery Use
- [Merritt UI](#)
- [Merritt Admin Tool](https://github.com/search?q=repo%3ACDLUC3%2Fmrt-admin-lambda+jquery+language%3AHTML&type=code&l=HTML)
- [Manifest Tool](https://github.com/CDLUC3/mrt-doc/blob/main/manifest/index.html)
- [Merritt Docker](https://github.com/CDLUC3/merritt-docker/blob/c1262afdb9cba271b876972c7a687d434dd29e0b/mrt-services/docker.html#L4)

---

## Vulnerability Review - sign into AWS SSO to access

- _This links to a tool created by our Systems Administrators_

## Other TODOs

