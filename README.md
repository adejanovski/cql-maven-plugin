# cql-maven-plugin

This plugin allows one to execute CQL statements on a cassandra cluster.


## Requirements

 * Java 8
 * Maven 3.3.3 (maybe older version work, but this has not been tested)
 * Cassandra 2.0 to 2.1 (driver is 2.1.6)

## Configuration

This plugin has a single goal (`execute`) and runs by default in the `pre-integration-test` phase.

#### Mandatory parameters :

 * `localDatacenter` : the datacenter name
 * `keyspace` : the cassandra keyspace on which statement will be applied
 * `fileset` : include / exclude CQL files

#### Optional parameters :

 * `username` : login, default is `cassandra`
 * `password` : password, default is `cassandra`
 * `contactPoint` : single contact point, default is `127.0.0.1`
 * `port` : CQL3 or native port, default is `9042`
 * `ignoreMissingFile` : should ignore when a file is not found, if `false` mojo will fail, default is `true`
 * `logStatements` : print every executed statement, default is `false`
 * `skip` : skip plugin execution, default is of course `false`

### Example

```xml
<plugin>
    <groupId>io.bric3</groupId>
    <artifactId>cql-maven-plugin</artifactId>
    <configuration>
        <username>ajax</username>
        <password>******************</password>
        <localDatacenter>DC1</localDatacenter>
        <contactPoint>cassandra.dc1.local</contactPoint>
        <port>9043</port>
        <ignoreMissingFile>true</ignoreMissingFile>
        <logStatements>true</logStatements>
    </configuration>
    <executions>
        <execution>
            <id>generate_keyspaces</id>
            <phase>install</phase>
            <goals>
                <goal>execute</goal>
            </goals>
            <configuration>
                <keyspace>system</keyspace>
                <fileset>
                    <directory>${project.build.directory}/classes/cassandra/all-keyspaces</directory>
                    <includes>
                        <include>*.cql3</include>
                    </includes>
                    <excludes>
                        <exclude>${cassandra.cql3.exclude}</exclude>
                    </excludes>
                </fileset>
            </configuration>
        </execution>
        <execution>
            <id>rebuild_model_keyspace_cql3</id>
            <phase>install</phase>
            <goals>
                <goal>execute</goal>
            </goals>
            <configuration>
                <keyspace>${cassandra.model.keyspace}</keyspace>
                <fileset>
                    <directory>${project.build.directory}/classes/cassandra/model</directory>
                    <includes>
                        <include>*.cql3</include>
                    </includes>
                    <excludes>
                        <exclude>${cassandra.cql3.exclude}</exclude>
                    </excludes>
                </fileset>
            </configuration>
        </execution>
    </executions>
</plugin>

```
