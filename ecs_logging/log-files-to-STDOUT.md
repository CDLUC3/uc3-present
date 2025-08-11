# Log4j and Tomcat Overrides to Send Logs to STDOUT

- [ECS Logging in Merritt](README.md)

---

All Merritt tomcat services are derivied from a common [tomcat Docker image](https://github.com/CDLUC3/merritt-docker/blob/main/mrt-inttest-services/merritt-tomcat/Dockerfile).

```Dockerile
# Base image for all Merritt tomcat usage

FROM public.ecr.aws/docker/library/tomcat:9-jdk21-corretto

# RUN apt-get update -y && apt-get -y upgrade
RUN yum -y update

EXPOSE 8080 8009

ENV CATALINA_OPTS="-Dfile.encoding=UTF8 -Dorg.apache.tomcat.util.buf.UDecoder.ALLOW_ENCODED_SLASH=true -XX:+UseG1GC"

# https://serverfault.com/questions/683605/docker-container-time-timezone-will-not-reflect-changes
ENV TZ=America/Los_Angeles
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
```

This Dockerfile was enhanced to ensure that all useful log entries are streamed to STDOUT.

```Dockerfile
COPY valve.txt /tmp
COPY modify_server.jsh /tmp
RUN cp /usr/local/tomcat/conf/server.xml /tmp/server.xml
RUN jshell -s /tmp/modify_server.jsh > /usr/local/tomcat/conf/server.xml
COPY log4j2-ecs.xml /
```

## valve.txt - The default AccessLogValve in server.xml will be replaced with this text

```xml
        <Valve className="org.apache.catalina.valves.AccessLogValve"
               directory="/dev"
               prefix="stdout"
               suffix=""
               pattern="%h %l %u %t &quot;%r&quot; %s %b"
               buffered="false"
               fileDateFormat="" />
```

## modify_server.jsh - a Java Shell script is run to extract and replace the default Valve entry

```jsh
String fname = "/tmp/server.xml";
String valve = "/tmp/valve.txt";
StringBuffer buf = new StringBuffer();
Files.lines(Paths.get(valve)).forEach(line -> {
  buf.append(line);
  buf.append("\n");
});

boolean bvalve = false;
Files.lines(Paths.get(fname)).forEach(line -> {
  if (line.matches(".*AccessLogValve.*")) {
    bvalve = true;
    System.out.println(buf.toString());
  }
  if (bvalve) {
    if (line.matches(".*\\/>")) {
      bvalve = false;
    }
  } else {
    System.out.println(line);
  }
});
/exit
```

## log4j2-ecs.xml - A default log4j file sends all Merritt log4j output to STDOUT

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="INFO">
  <Appenders>
    <Console name="Text-Console" target="SYSTEM_OUT">
      <EcsLayout
          eventDataset="tomcat"
          includeMarkers="true"
          stackTraceAsArray="false"
          includeOrigin="true">
      </EcsLayout>
    </Console>
    
  </Appenders>

  <Loggers>
    
    <Root level="${env:LOGLEVEL:-info}">
        <AppenderRef ref="Text-Console" level="DEBUG"/>
    </Root>
    
    <Logger name="FileLog" level="DEBUG" additivity="false">
        <AppenderRef ref="Text-Console" />
    </Logger>
    
    <Logger name="FixJson" level="INFO" additivity="false">
        <AppenderRef ref="Text-Console" />
    </Logger>
    
    <Logger name="JSONLog" level="INFO" additivity="false">
        <AppenderRef ref="Text-Console" />
    </Logger>
        
  </Loggers>
</Configuration>
```

---

[NEXT](aws-firelens.md)