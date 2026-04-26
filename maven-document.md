🚀 Maven + JFrog Artifactory Complete Hands-On Guide
📌 Objective
Install Java & Maven
Create a Maven project
Understand pom.xml structure
Use tree to track file changes
Configure JFrog Artifactory
Download dependencies via Artifactory
Deploy JAR/WAR artifacts
Understand dependency flow (local → remote)
🧰 1. Install Java & Maven
Install Java
sudo apt update
sudo apt install openjdk-17-jdk -y
java -version
Install Maven
sudo apt install maven -y
mvn -version
📦 2. Create Maven Project
mvn archetype:generate \
  -DgroupId=com.example.demo \
  -DartifactId=my-app \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DinteractiveMode=false

cd my-app
🌳 3. Project Structure
sudo apt install tree -y
tree
Output
my-app/
├── pom.xml
└── src
    ├── main/java/com/example/demo/App.java
    └── test/java/com/example/demo/AppTest.java
🔄 4. Maven Lifecycle
Compile
mvn compile
tree target
Test
mvn test
Package
mvn package

Output:

target/
└── my-app-1.0-SNAPSHOT.jar
Install
mvn install
tree ~/.m2/repository
📦 5. pom.xml Overview
Basic Structure
<project>
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.example</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>jar</packaging>
</project>
🔹 Key Elements
groupId
Organization name
Example: com.company
artifactId
Project name
Example: payment-service
version
1.0-SNAPSHOT → Development
1.0.0 → Release
packaging
jar → Java app
war → Web app
📚 6. Dependencies
<dependencies>
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.2</version>
    <scope>test</scope>
  </dependency>
</dependencies>
Behavior
Maven checks Artifactory
If not found → remote repo → download
⚙️ 7. Build Configuration
<build>
  <plugins>
    <plugin>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.11.0</version>
      <configuration>
        <source>17</source>
        <target>17</target>
      </configuration>
    </plugin>
  </plugins>
</build>
📤 8. Deployment Configuration
<distributionManagement>
  <repository>
    <id>jfrog-artifactory</id>
    <url>https://your-domain/artifactory/libs-release-local</url>
  </repository>
  <snapshotRepository>
    <id>jfrog-artifactory</id>
    <url>https://your-domain/artifactory/libs-snapshot-local</url>
  </snapshotRepository>
</distributionManagement>
Deploy Command
mvn clean deploy
🔐 9. settings.xml Configuration

Location:

~/.m2/settings.xml
🔹 Authentication
<servers>
  <server>
    <id>jfrog-artifactory</id>
    <username>your-username</username>
    <password>your-password</password>
  </server>
</servers>
🔹 Mirror (Download Control)
<mirrors>
  <mirror>
    <id>jfrog-mirror</id>
    <mirrorOf>*</mirrorOf>
    <url>https://your-domain/artifactory/virtual-maven</url>
  </mirror>
</mirrors>
🔹 Repository Profile
<profiles>
  <profile>
    <id>jfrog</id>
    <repositories>
      <repository>
        <id>central</id>
        <url>https://your-domain/artifactory/virtual-maven</url>
      </repository>
    </repositories>
  </profile>
</profiles>

<activeProfiles>
  <activeProfile>jfrog</activeProfile>
</activeProfiles>
🏗️ 10. Artifactory Repository Types
Repository Type	Purpose
libs-release-local	Store release artifacts
libs-snapshot-local	Store snapshot artifacts
remote-maven-central	Proxy to Maven Central
virtual-maven	Combined access
🔄 11. Dependency Download Flow
Maven →
   settings.xml (mirror) →
      Artifactory virtual repo →
         → libs-release-local
         → libs-snapshot-local
         → remote repo (internet)
            → download
            → cache in Artifactory
📤 12. Artifact Upload Flow
mvn deploy →
pom.xml (distributionManagement) →
settings.xml (authentication) →
Artifactory (libs-release / snapshot)
🌳 13. Track Changes Using tree
Before build
tree
After compile
tree target
After package
tree target
After install
tree ~/.m2/repository
🧪 14. Practice Scenarios
Scenario 1: Clear Local Repo
rm -rf ~/.m2/repository
mvn clean package
Scenario 2: Offline Mode
Disable internet
Use only Artifactory
Scenario 3: Authentication Failure
Change credentials
Observe error
Scenario 4: Custom Library
Upload manually to Artifactory
Use in project
🧠 15. Key Learnings
Maven lifecycle
pom.xml structure
Dependency resolution
Artifact deployment
Artifactory integration
Repository types
Authentication setup
🎯 Interview Summary

pom.xml defines project metadata, dependencies, build, and deployment configuration, while settings.xml handles authentication and redirects dependency downloads to Artifactory using mirrors.

🚀 Next Steps
CI/CD with Jenkins + Maven
Multi-module Maven projects
Secure secrets (Vault, AWS Secrets Manager)
Versioning strategies
