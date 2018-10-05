to change to root user use sudo sub
and to login use sudo su and su-

Maven
Java build tool which is poweful
Maven is a enterprise project management tool which has POM, a dependency MS,..
Maven 2 is latest which is a software framework that incorporates
1)a set of build  standars
2)a repository model -> plugisn -> remote repository
3)meta data driven engine

*standard directory layout - difult directory layout
*Separation of concerns - one output per project
*standard naming convention
universal resue using maven plugins ex: complie, jar plugin 
installation Maven
Download Apache Maven Zip file
requirements
JDK
Set 2 envi variables

 Archetype: Its a plugin to generate a project template
 *mvn archetype:generate* to generate project template
 choose from given options 
 choose version number
 Enter GroupID : we have to provide a unique groupID
 org.sonatype.mavenbook
 artifactID:  name of the project
 Snapshot: version number
 Package: same as groupid
 archetype has 4 goals
 1)generate
 2)create
 3)create-from-project
 4)crawl
 
 after creating build the generated project, for that 
 maven life cycle:
 The process of building and distributing a particular artifact
 there are 3 built-in life cycles
 default life cycle -> all the process of building provide
 clean life cycle-> to clean the project build directories 
 site life cycle->to generate documentation 
 
* How to use build life cycle?
 The diff phases available in build life cycle:
Validate: validates the project is correct and all necessary information is available
 compile:
 test:
 package:
 verify:
 install:
 deploy:
 *
 when we choose package phase, it will runs through all phases
 to perform each phase use 
 *mvn phase name*
 every maven project will have POM(Project Object Model) which contains project settings.
 There are 3 types of repositories
 Local, remote and central 
 we will have local repository in .m2 folder 
 central repository is pulgins website
 and now we are going to setup remote mvn repository. There are some tools like nexus, artifactory(jfrog), apache archiva
 to install rpm files in linux, we have to use 
*sudo rpm -ivh filename*
to install tar files, we have to use
*sudo tar zxvf filename* and (apache tomcat )after installation move this 
mv apache-tomcat /opt/tomcat7/ (mkdir tomcat7) 
always set the path of tomcat in under resources in server.xml
we can set path of any software in ~/.bash_profile
always use publicip to connect tomcat server

after loging into server under manager deploy nexus war file.
The default credentials are admin and admin123
we are more intrested in release and snapshot repositories (under repositories section) 
 
 the artifacts that are generated during development are stored in snapshot repository
 to store final product we use release repository
 now we have to deploy artifcats in remote repository(central repository)
to deploy the artifacts in remote repository we should  
 update the pom.xml with
<distributionManagement>
<repository>
<id>releases</id>
<url>http://34.224.40.137:8080/nexus-2.14.7-01/content/repositories/releases</url>
</repository>
<snapshotRepository>
<id>snapshots</id>
<url> http://34.224.40.137:8080/nexus-2.14.7-01/content/repositories/snapshots</url>
</snapshotRepository>
</distributionManagement>

Now goto .m2 folder and create a settings.xml file and provide authentication details
 
<settings>
<servers>
<server>
<id>releases</id>
<username>admin</username>
<password>admin123</password>
</server>
<server>
<id>snapshots</id>
<username>admin</username>
<password>admin123</password>
</server>
</servers> 
 
 now we will deploy the project using 
 *mvn deploy* it will store in release repository
 if we want to deploy snapshot of project then we should change vesrion parameter in POM.xml file
 it should be like this
 <1.0-SNAPSHOT> and *mvn deploy* (if it is release, then it should be <1.0>)
 The differnce between snapshot and release version is , snapshot of project will have time stamp and release will not have time stamp
 we can see all this information at /root/sonatype/nexus/storage
 In real time, we create a parent project and that will have all sub projects 
 for installing all subproject we no need to perform operation for ecah project, just we can install parent project which installs all the sub projects.
 This kind of projects are called "Multi-module projects"
 for creating parent project we need to use something like this
 *mvn archetype:generate -DarchetypeGroupId=org.codehaus.mojo.archetypes -DarchetypeArtifactId=pom-root -DarchetypeVersion=RELEASE -DartifactId=simpleparentproject -Dpackage=org.sonatype.mavenbook -Dversion=1.0*
 from this project we need to create sub projects 
*mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=org.sonatype.mavenbook -DartifactId=simplejava_su-Dpackage=org.sonatype.mavenbook -Dversion=1.0
we need to add <distributionManagement> details in parent pom.ml file.
deploying parent project it will incude sub project artifacts too.

Maven repository
after complete development, the product is ready to release then we have to perform thses tasks
1) change the SNAPSHOT version to Release version (In pom.xml file)
2)Compile ->test->package->deploy(Storing in remote repository)
Once artifact is generated, after sucessfull testing then artifact will be released to customers
3)VCS part, we will creat tags to uniquly identify the release
after this, the developers work on next update or goals 
4) we should edit the pom.xml file to update SNAPSHOT version

*we can automate these steps using maven release plugin* 
maven releaseplugin has to create tags , for that we need to provide <scm></scm>(git repo details)
we have to provide maven release plugin 



<scm>
<connection>scm:git:https://github.com/kothamaddisiva/mvn_practise.git</connection>
<developerConnection>scm:git:https://github.com/kothamaddisiva/mvn_practise.git</developerConnection>
</scm>

<build>
<plugins>
<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-release-plugin</artifactId>
<version>3.5.3</version>
</plugins>
</build>
<distributionManagement>
<repository>
<id>releases</id>
<url>http://34.224.40.137:8080/nexus-2.14.7-01/content/repositories/releases</url>
</repository>
<snapshotRepository>
<id>snapshots</id>
<url> http://34.224.40.137:8080/nexus-2.14.7-01/content/repositories/snapshots</url>
</snapshotRepository>
</distributionManagement>
Maven
build 
checkout->complie->run testcases->packages(Artifacts)
Dependences-> transitive dependences
2 types of dependencies 
Internal and extrnal
Internal dependencies will be local repositories
and extral dependencies are third party dependencies

types of scope
Complie: all phases, they are packaged with generated artifact
Provided:compile and test phases, they don't get bundled with in the generated artifacts
runtime:they get bundled within the generated artifact
test:during test phases
system:dependencies with the system scope are not retrieved from repository, instead it will be avilable in local path


Advantage of maven is dependence management 
co-ordinates for dependences 
Deploy: The deployment is just deplying artifacts in repository. its not related enviornment 
3 repositories 
local -> .m2 folder
internal -> nexus
extral-> central 





help:effective-pom

Inheritance
super pom -> parent->child

aggregation (mentioning the modlues information in parent pom file)



www.avajava.com/tutorials/categories/maven
