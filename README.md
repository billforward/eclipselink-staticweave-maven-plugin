# eclipselink-staticweave-maven-plugin

## Usage

```
    <build>
        <plugins>
            <plugin>
                <artifactId>eclipselink-staticweave-maven-plugin</artifactId>
                <groupId>au.com.alderaan</groupId>
                <version>1.0.7</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>weave</goal>
                        </goals>
                        <phase>process-classes</phase>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```

### Configuration

|Setting     |Description                                                      | Default                          |
|------------|-----------------------------------------------------------------|----------------------------------|
| source     | Location of the source JPA classes and persistence.xml file.    | ${project.build.outputDirectory} |
| target     | Destination for the weaved classes.                             | ${project.build.outputDirectory} |
| persistenceXMLLocation | Override where to find persistence.xml.             |                                  |
| persistenceXMLGroupId | If the persistence.xml is not included in the project but in a dependency, set here the dependency's group id.             |                                  |
| persistenceXMLArtifactId | If the persistence.xml is not included in the project but in a dependency, set here the dependency's artifact id.             |                                  |
| persistenceXMLVersion | If the persistence.xml is not included in the project but in a dependency, set here the dependency's version.             |                                  |
| logLevel   | Log level (OFF SEVERE WARNING INFO CONFIG FINE FINER FINEST ALL)| OFF                              |
-------------------------------------------------------------------------------------------------------------------

## Updates

### Release 1.0.7

Release 1.0.7 adds support for `persistence.xml` files located in external dependencies. If this is the case use the 
`persistenceXMLGroupId`, `persistenceXMLArtifactId` and `persistenceXMLVersion` parameters in combination with `persistenceXMLLocation`.

```
    <plugin>
        <artifactId>eclipselink-staticweave-maven-plugin</artifactId>
        <groupId>au.com.alderaan</groupId>
        <version>1.0.7</version>
        <executions>
            <execution>
                <goals>
                    <goal>weave</goal>
                </goals>
                <phase>process-classes</phase>
                <configuration>
                    <logLevel>ALL</logLevel>
                    <persistenceXMLLocation>META-INF/persistence.xml</persistenceXMLLocation>
                    <persistenceXMLGroupId>my.group.id</persistenceXMLGroupId>
                    <persistenceXMLArtifactId>artifactId</persistenceXMLArtifactId>
                    <persistenceXMLVersion>1.0.0</persistenceXMLVersion>
                </configuration>
            </execution>
        </executions>
        <dependencies>
            <dependency>
                <groupId>org.eclipse.persistence</groupId>
                <artifactId>eclipselink</artifactId>
                <version>2.3.2</version>
            </dependency>
        </dependencies>
    </plugin>
```

### Release 1.0.4

Release 1.0.4 adds support for working around an issue that can affect Eclipselink < 2.3.3 related to 
classloading and ASM. In certain scenarios, the weaver is unable to load classes and fails to weave 
correctly. To enable this workaround set includeProjectClasspath to true in your configuration:

```
    <plugin>
        <artifactId>eclipselink-staticweave-maven-plugin</artifactId>
        <groupId>au.com.alderaan</groupId>
        <version>1.0.4</version>
        <executions>
            <execution>
                <goals>
                    <goal>weave</goal>
                </goals>
                <phase>process-classes</phase>
                <configuration>
                    <logLevel>ALL</logLevel>
                    <includeProjectClasspath>true</includeProjectClasspath>
                </configuration>
            </execution>
        </executions>
        <dependencies>
            <dependency>
                <groupId>org.eclipse.persistence</groupId>
                <artifactId>eclipselink</artifactId>
                <version>2.3.2</version>
            </dependency>
        </dependencies>
    </plugin>
```

### Release 1.0.3

Release 1.0.3 adds support for specifying the location/name of the persistence.xml file. 
E.g. `META-INF/my-persistence.xml`. 

```
    <plugin>
    ...
        <configuration>
            <persistenceXMLLocation>META-INF/my-persistence.xml</persistenceXMLLocation>
        </configuration>
    ...
    </plugin>    
```

### Release 1.0.2

Release 1.0.2 of the plugin has been updated to work correctly with Eclipselink 2.3.2. If you are using 
2.2.X stick with version 1.0.1 of the plugin. For 2.3.X versions of Eclipselink other than 2.3.2, simply 
override with a `<dependencies>` block in the plugin config. e.g.

```
    <build>
        <plugins>
            <plugin>
                <artifactId>eclipselink-staticweave-maven-plugin</artifactId>
                <groupId>au.com.alderaan</groupId>
                <version>1.0.3</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>weave</goal>
                        </goals>
                        <phase>process-classes</phase>
                        <configuration>
                            <logLevel>ALL</logLevel>
                        </configuration>
                    </execution>
                </executions>
                <dependencies>
                    <dependency>
                        <groupId>org.eclipse.persistence</groupId>
                        <artifactId>eclipselink</artifactId>
                        <version>2.3.1</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </build>

```

If you do this, remember that Eclipselink dependencies are not available in Central so you will 
probably need to add a reference to the Eclipselink Maven repo as well. e.g.

```
    <repositories>
        <repository>
            <id>eclipselink</id>
            <url>http://download.eclipse.org/rt/eclipselink/maven.repo/</url>
        </repository>
    </repositories>
```


