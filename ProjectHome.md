## simpledb-appender lets you log from multiple machines to a central, reliable, and query-able location: Amazon SimpleDB ##

simpledb-appender is an implementation of the [Logback](http://logback.qos.ch/manual/appenders.html) [Appender interface](http://logback.qos.ch/apidocs/ch/qos/logback/core/Appender.html) for writing to a SimpleDB domain. Java projects using [SLF4J](http://www.slf4j.org/) with [Logback](http://logback.qos.ch/) can use simpledb-appender natively. Java projects using log4j or JCL can still use simpledb-appender, although [a bit more work is required](http://www.slf4j.org/legacy.html).

## How do I use it? ##
See the [Getting Started](http://code.google.com/p/simpledb-appender/wiki/GettingStarted) wiki

## How do I get help? ##
Ask questions on the [simpledb-logging group](http://groups.google.com/group/simpledb-logging)

## Why would I want to log to SimpleDB ##
There are a number of cool benefits to this:

  * Logs from many machines seamlessly stored in a single location
  * Never worry about rotating logs
  * Query your logs using SQL-like language. For Example:

```
SELECT * FROM opslogs WHERE time > '2010-01-13' AND time < '2010-01-14' AND host = '10.0.2.121' AND level = 'ERROR'
```

## I don't use Java, can I still use simpledb-appender? ##
Yes, you can redirect stdout/stderr into tools included in the simpledb-appender package.

## What about performance? ##
Logback is designed to be extremely efficient. simpledb-appender is also designed to have minimal impact on CPU resources. Logging events are buffered in memory and then later at user-configurable intervals (by default 10 seconds) are written to SimpleDB in bulk on a separate thread. In this way calls to the logging API return very quickly, and the time-consuming work is done in a way that doesn't block the main application. A JVM shutdown hook writes any unwritten log events when the application exits.

## How easy is it to configure? ##
Pretty simple. All you need to do is specify your AWS credentials and the name of the SimpleDB domain to write to:

```
<configuration>
  <appender name="SIMPLEDB" class="com.kikini.logging.simpledb.SimpleDBAppender">
    <DomainName>your_simpledb_domain</DomainName>
    <AccessId>your_aws_access_id</AccessId>
    <SecretKey>your_aws_secret_key</SecretKey>
  </appender>

  <root level="INFO">
    <appender-ref ref="SIMPLEDB"/>
  </root>
</configuration>
```
