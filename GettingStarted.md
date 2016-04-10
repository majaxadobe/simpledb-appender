# Introduction #

You can either integrate simpledb-appender into an existing project using SLF4J, or use the standalone tools to log the STDOUT/STDERR output of other apps to log to SimpleDB.

# Download and build #
You can download the JAR from the Downloads page, or you can build with maven.

# Using the standalone tools #

Please see the scripts in /etc for an example of a program to log from STDIN to SimpleDB. Copy these scripts to your system. Create a directory "lib" and drop the simpledb-appender JAR, and the dependency JARs in the lib folder. Run stdin2simpledb.sh

The usage is

stdin2simpledb.sh path-to-logback.xml

# Integrating to existing SLF4J #

Download the simpledb-appender (from the downloads tab) and add it to your classpath. Configure logback.xml to use simpledb-appender.

# Configuring logback.xml #

The most trivial configuration looks like this:

```
<configuration>
  <appender name="SIMPLEDB" class="com.kikini.logging.simpledb.SimpleDBAppender">
    <domainName>your_simpledb_domain</domainName>
    <accessId>your_aws_access_id</accessId>
    <secretKey>your_aws_secret_key</secretKey>
  </appender>

  <root level="INFO">
    <appender-ref ref="SIMPLEDB"/>
  </root>
</configuration>
```

The domain name, access id, and secret key are all required. There are also some optional properties:

  * timeZone: The timezone to use for the time field
  * loggingPeriodMillies: How frequently to batch-write to SimpleDB. The default is 10000 (10 seconds)
  * host: A host identifier such as IP, DNS name, or instance-id
  * componentName: The name of the component/application, to differentiate multiple loggers on a single host

# Dependencies #

Until I create a TAR/ZIP with all the dependencies (need to make sure the licenses are respected), here is a list of dependencies:

```
commons-codec-1.3.jar
logback-core-0.9.15.jar
commons-httpclient-3.1.jar
simpledb-appender-0.8.jar
jcl-over-slf4j-1.5.8.jar
slf4j-api-1.5.8.jar
joda-time-1.6.jar
typica-1.6.jar
logback-classic-0.9.15.jar
```