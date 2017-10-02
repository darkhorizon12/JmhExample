# JmhExample

1. Create Jmh Project in Command Line
  - Maven install
  - Enter below statement in Console
    $ mvn archetype:generate 
          -DinteractiveMode=false 
          -DarchetypeGroupId=org.openjdk.jmh 
          -DarchetypeArtifactId=jmh-java-benchmark-archetype 
          -DgroupId=com.jayjaylab.sample  // package name
          -DartifactId=jmh-sample -Dversion=1  // project name
    
    $ cd jmh-sample   // move to project folder
    
    $ mvn clean install // maven build
    
    $ java -jar target/benchmarks.jar  // execute application
    
2. Create Jmh Project in Eclipse
  - pom.xml
  ```
     <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
        <modelVersion>4.0.0</modelVersion>

        <groupId>org.sample</groupId>
        <artifactId>test</artifactId>
        <version>1.0</version>
        <packaging>jar</packaging>

        <name>JMH benchmark sample: Java</name>

        <!--
           This is the demo/sample template build script for building Java benchmarks with JMH.
           Edit as needed.
        -->

        <dependencies>
            <dependency>
                <groupId>org.openjdk.jmh</groupId>
                <artifactId>jmh-core</artifactId>
                <version>${jmh.version}</version>
            </dependency>
            <dependency>
                <groupId>org.openjdk.jmh</groupId>
                <artifactId>jmh-generator-annprocess</artifactId>
                <version>${jmh.version}</version>
                <scope>provided</scope>
            </dependency>
        </dependencies>

        <properties>
            <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>

            <!--
                JMH version to use with this project.
              -->
            <jmh.version>1.19</jmh.version>

            <!--
                Java source/target to use for compilation.
              -->
            <javac.target>1.8</javac.target>

            <!--
                Name of the benchmark Uber-JAR to generate.
              -->
            <uberjar.name>benchmarks</uberjar.name>
        </properties>

        <build>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.1</version>
                    <configuration>
                        <compilerVersion>${javac.target}</compilerVersion>
                        <source>${javac.target}</source>
                        <target>${javac.target}</target>
                    </configuration>
                </plugin>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-shade-plugin</artifactId>
                    <version>2.2</version>
                    <executions>
                        <execution>
                            <phase>package</phase>
                            <goals>
                                <goal>shade</goal>
                            </goals>
                            <configuration>
                                <finalName>${uberjar.name}</finalName>
                                <transformers>
                                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                        <mainClass>org.openjdk.jmh.Main</mainClass>
                                    </transformer>
                                </transformers>
                                <filters>
                                    <filter>
                                        <!--
                                            Shading signed JARs will fail without this.
                                            http://stackoverflow.com/questions/999489/invalid-signature-file-when-attempting-to-run-a-jar
                                        -->
                                        <artifact>*:*</artifact>
                                        <excludes>
                                            <exclude>META-INF/*.SF</exclude>
                                            <exclude>META-INF/*.DSA</exclude>
                                            <exclude>META-INF/*.RSA</exclude>
                                        </excludes>
                                    </filter>
                                </filters>
                            </configuration>
                        </execution>
                    </executions>
                </plugin>
            </plugins>
            <pluginManagement>
                <plugins>
                    <plugin>
                        <artifactId>maven-clean-plugin</artifactId>
                        <version>2.5</version>
                    </plugin>
                    <plugin>
                        <artifactId>maven-deploy-plugin</artifactId>
                        <version>2.8.1</version>
                    </plugin>
                    <plugin>
                        <artifactId>maven-install-plugin</artifactId>
                        <version>2.5.1</version>
                    </plugin>
                    <plugin>
                        <artifactId>maven-jar-plugin</artifactId>
                        <version>2.4</version>
                    </plugin>
                    <plugin>
                        <artifactId>maven-javadoc-plugin</artifactId>
                        <version>2.9.1</version>
                    </plugin>
                    <plugin>
                        <artifactId>maven-resources-plugin</artifactId>
                        <version>2.6</version>
                    </plugin>
                    <plugin>
                        <artifactId>maven-site-plugin</artifactId>
                        <version>3.3</version>
                    </plugin>
                    <plugin>
                        <artifactId>maven-source-plugin</artifactId>
                        <version>2.2.1</version>
                    </plugin>
                    <plugin>
                        <artifactId>maven-surefire-plugin</artifactId>
                        <version>2.17</version>
                    </plugin>
                </plugins>
            </pluginManagement>
        </build>

    </project>
```
  - Create Target Class
    Note that create main method like this
      <pre><code>public static void main(String[] args) throws Exception{
        Options opts = new OptionsBuilder()
                  .include(".*")
                  .warmupIterations(10)
                  .measurementIterations(10)
                  .forks(1)
                  .build();
              new Runner(opts).run();
	  }
 	</code></pre>
 - To build
    Run As -> Run As -> Maven Build (goals => enter 'clean install')
 - To execute 
    Run As -> Java Application (select 'Main - org.openjdk.jmh')
