---
title:  "Logging user authentication events"
date:   2017-10-09 09:59
categories: [Security, Logging]
tags: [Security, Logging]
---

When we want to enable security logging, we need to follow the instructions in here. It happens that when we do this the log messages are so many that we get lost in so many messages that we almost need log analysis tools. If we just want to grab some of the events there is not too much we can do, except writing a small peace of code, or use the one I wrote.

Pentaho uses Spring Security, that comes with application listeners and allows us to grab some events and log to output file as we need. So, given that was not the first time I saw this requests from Pentaho users, I decided to extend Pentaho code.

The code does not change any of the code, neither does require any compilation of Pentaho code. We just need apply some beans configurations to extend functionality.

**Step 1)** First, we need to add the following line in file: **…pentaho-server/pentaho-solutions/system/pentaho-spring-beans.xml**

```xml
    <import resource="applicationContext-pentaho-security.custom-user-authenticate-events.xml" />
```

**Step 2)** Next step is to set the properties that will dictate the behavior in the code, and what should be logged. Add the following lines to the file: **…pentaho-server/pentaho-solutions/system/applicationContext-pentaho-security.custom-user-authenticate-events.xml**
```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:util="http://www.springframework.org/schema/util" xmlns:pen="http://www.pentaho.com/schema/pentaho-system" xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.3.xsd http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.3.xsd http://www.pentaho.com/schema/pentaho-system http://www.pentaho.com/schema/pentaho-system.xsd" default-lazy-init="true">
        <bean id="loggerListener" class="io.github.mfgaspar.pentaho.authentication.events.loggerListener" scope="singleton">
            <property name="enablePentahoLoggerListener" value="false"/>
            <property name="enableNoAuthenticationEvents" value="true"/>
            <property name="enableBadCredentialEvents" value="true"/>
            <property name="enableSessionCreatedEvents" value="true"/>
            <property name="enableSessionDestroyedEvents" value="true"/>
            <property name="enableTraceServletEvents" value="false"/>
            <property name="enableTraceOtherEvents" value="false"/>
            <property name="messageTemplate" value="(%s|%s|%s|%s)"/>
            <property name="noAuthenticationEventsId" value="NO-AUTHENTICATION"/>
            <property name="badCredentialEventsId" value="AUTHENTICATION-FAILED"/>
            <property name="sessionCreatedEventsId" value="SESSION-START"/>
            <property name="sessionDestroyedEventsId" value="SESSION-END"/>
        </bean>
    </beans>
```

**Step 3)** The value of the properties can be set to define what will be logged into the log file. Note that you don’t really need to change them, or set them. The available properties are:

- **enableNoAuthenticationEvents**: Set it to true will log some events, when a user requests a resource without authentication. Default value is true
- **enableBadCredentialEvents**: Set it true to log informations about failed login attempts. Default value is true
- **enableSessionCreatedEvents**: Set it to true to log information when user logs in (session is created for user). Default value is true
- **enableSessionDestroyedEvents**: Set it to true to log information when user logs out or session expired (session is destroyed). Default value is true
- **enableTraceServletEvents**: Set it to true when you want servlet events to be logged (log level in log4j should be set to TRACE). Default value is false
- **enableTraceOtherEvents**: Set it to true when you want all other events logged in file. Should be used only for debugging (log level in log4j should be set to TRACE). Default value is false
- **messageTemplate**: Sets the format of the message to be send to log file, used for the folowing events: NoAuthenticationEvents, BadCredentialEvents, SessionCreatedEvents and SessionDetroyedEvents. The message will include the Event ID, the IP address of remote host, the Username and de Session ID. Default message format is ```(%s|%s|%s|%s)```. You can check the syntax in here
- **noAuthenticationEventsId**: Sets the event ID that will identify the event on the log file. Default value is “NO-AUTHENTICATION”
- **badCredentialEventsId**: Sets the event ID that will identify the event on the log file. Default value is “AUTHENTICATION-FAILED”
- **sessionCreatedEventsId**: Sets the event ID that will identify the event on the log file. Default value is “SESSION-START”
- **sessionDestroyedventsId**: Sets the event ID that will identify the event on the log file. Default value is “SESSION-END”


**Step 4)** You also need to add the folowing lines into: **…/pentaho-server/tomcat/webapps/pentaho/classes/log4j.xml**.

```xml
    <log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/" debug="false">
        <appender name="AUTHENTICATELOG" class="org.apache.log4j.RollingFileAppender">
            <param name="File" value="../logs/authenticate_events.log"/>
            <param name="Append" value="false"/>
            <param name="MaxFileSize" value="500KB"/>
            <param name="MaxBackupIndex" value="1"/>
            <layout class="org.apache.log4j.PatternLayout">
                <param name="ConversionPattern" value="%d %-5p [%c] %m%n"/>
            </layout>
        </appender>
        <category name="...">
            <priority value="INFO"/>
            <appender-ref ref="AUTHENTICATELOG"/>
        </category>
    </log4j:configuration>
```

**Step 5)** Place this jar file in **…/pentaho-server/tomcat/webapps/pentaho/WEB-INF/lib** folder. Source code is available here.

**Step 6)** Restart te server. Once the users starts to login, logout or even failing to authenticate, the file **…/pentaho-server/tomcat/logs/authenticate_events.log** will start to be populated. The following lines are an example of the output.

	2017-10-09 04:37:26,696 INFO  [...] (NO-AUTHENTICATION|0:0:0:0:0:0:0:1|anonymousUser|905E6FADA39FE79B23E48C01190776C8)
	2017-10-09 04:37:43,715 INFO  [...] (SESSION-START|0:0:0:0:0:0:0:1|admin|905E6FADA39FE79B23E48C01190776C8)
	2017-10-09 04:44:13,051 INFO  [...] (NO-AUTHENTICATION|192.168.56.1|anonymousUser|661BE4C2563FFEA09BB80E300579490F)
	2017-10-09 04:44:15,216 INFO  [...] (SESSION-START|192.168.56.1|admin|661BE4C2563FFEA09BB80E300579490F)
	2017-10-09 04:44:23,479 INFO  [...] (SESSION-END|192.168.56.1|admin|661BE4C2563FFEA09BB80E300579490F)
	2017-10-09 04:44:23,575 INFO  [...] (NO-AUTHENTICATION|192.168.56.1|anonymousUser|833B9C0DE7490AECFB36EB04940C4339)
	2017-10-09 04:44:31,121 INFO  [...] (NO-AUTHENTICATION|192.168.56.1|anonymousUser|23BD39EB76D0D973AD867CB92CF1E916)
	2017-10-09 04:44:38,975 INFO  [...] (AUTHENTICATION-FAILED|192.168.56.1|suzy|23BD39EB76D0D973AD867CB92CF1E916)
	2017-10-09 04:44:44,160 INFO  [...] (SESSION-START|192.168.56.1|suzy|23BD39EB76D0D973AD867CB92CF1E916)
	2017-10-09 04:44:51,137 INFO  [...] (SESSION-END|192.168.56.1|suzy|23BD39EB76D0D973AD867CB92CF1E916)
	2017-10-09 04:44:51,155 INFO  [...] (NO-AUTHENTICATION|192.168.56.1|anonymousUser|5A7E00C559286B545690AB88197D997F)

All files available [here](https://github.com/mfgaspar/mfgaspar.github.io-samples/tree/master/pentaho/security.events.authenticate/artifacts).



Follow me at [Twitter](https://twitter.com/migfgaspar)

[Live Insights]: #
