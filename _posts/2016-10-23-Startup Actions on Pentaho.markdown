---
title:  "Startup Actions on Pentaho: PentahoHttpSession VS UserSession"
date:   2016-10-23 10:43
categories: [Startup Actions, Action Sequences]
tags: [Startup Actions, Action Sequences]
--- 

There might be easier ways to deal with some actions that you need to execute during the user login or during the start of the server itself, anyhow action sequences are still very usefull. For sure you already know about te content of this post, anyhow I needed to write it as an introduction for the next post I will be writting. 

Pentaho created a tools called [Startup Rule Engine](http://pedroalves-bi.blogspot.com/2014/02/startup-rule-engine-new-sparkl-app-in.html) and was covered in Pedro Alves blog, some time ago, but that's what will covered in this blogpost. 

When we have a multitenant environment, or we need to have one, we can use an Action Sequence to run some actions. Mondrian is capable of handling multitenancy in different ways, and one of ways is to use a DSP (Dynamic Schema Processor), information that can be found in the help pages of Pentaho, [here](https://help.pentaho.com/Documentation/6.1/0R0/070/Multi-Tenancy). When using a DSP, most probably, you will need to create session variables that can be used latter in the DSP. Besides that, Startup Rule Engine, Action Sequences can easily be set and used to set the sessions variables you will need.

Please keep in mind that a session variable is already set when a user is logged in, and you can use it, but the value will be the username of the user that logged in. So it can be used when you are able to use it to do the filtering of the data that will be available. Otherwise you need to use some alternatives.  An example is when a user does the login we can trigger a SQL query that will set the session variable with the tenant id to be used, where the tenant id is not the username. That can later be used on Mondrian. 

The example bellow, is from an xaction that depending on the user that logged in, sets the value for a session variable named "REGION". If the logged in user it's "joe", the xaction will create a session variable with the values "APAC" so we will be able to restrict the access to user "joe", to be only able to see "APAC" results, while "suzy" will could have access to "EMEA" and the remaining users will have access to all.  

```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<action-sequence> 
	  <name>session-region.xaction</name>
	  <title>%title</title>
	  <version>1</version>
	  <logging-level>debug</logging-level>
	  <documentation> 
	    <author>%author</author>  
	    <description>%desciption</description>  
	    <help/> 
	  </documentation>

	  <inputs> 
	    <user type="string"> 
	      <sources> 
	        <session>name</session> 
	      </sources> 
	    </user>  
	  </inputs>

	  <outputs> 
	    <user type="string">
	      <destinations>
	        <session>user</session>
	      </destinations>
	    </user>
	    <REGION type="string">
	      <destinations>
	        <session>REGION</session>
	      </destinations>
	    </REGION>
	  </outputs>

	  <resources/>
	  
	  <actions> 
	    <action-definition> 
	      <component-name>JavascriptRule</component-name>
	      <action-type>JavaScript To Build User Specific Result Set</action-type>
	      <action-inputs> 
	        <user type="string"/> 
	      </action-inputs>
	      <action-outputs> 
	        <REGION type="string"/> 
	      </action-outputs>
	      <component-definition> 
	        <script><![CDATA[function getRegions( user ) {
						    var region = "none";
				      	if( user == 'joe' ) { region = "APAC"; } 
				      	else if( user == 'suzy' ) { region = "EMEA"; } 
	              else { region = "EMEA, APAC, ..."; }
				      	return res;
				      }
	            getRegions( user );
	        ]]></script> 
	      </component-definition> 
	    </action-definition>
	 
	  </actions> 
	</action-sequence>
```

Once you have the action sequence ready, you need to upload it to the repository to be able to execute it, and if you execute from the Pentaho User Console when using the user "Suzy", you will get something like: 

	Action Successful
	-----------------
	user=suzy
	REGION=EMEA

Now that you have the action sequence in place, you need to make sure to execute it when the user logs in. For that we need to edit the file: ../pentaho-solutions/system/sessionStartupActions.xml. There you will find something like: 

```xml
	<!-- Start of PentahoHttpSession startup actions. -->
	<bean class="org.pentaho.platform.engine.core.system.SessionStartupAction">
		<property name="sessionType" value="org.pentaho.platform.web.http.session.PentahoHttpSession"/>
		<property name="actionPath" value="/public/your_xaction_path_and_file.xaction"/>
		<property name="actionOutputScope" value="session"/>
	</bean>
	<!-- End of PentahoHttpSession startup actions. -->				
	<!-- Start of UserSession startup actions. -->
	<bean class="org.pentaho.platform.engine.core.system.SessionStartupAction">
		<property name="sessionType" value="org.pentaho.platform.engine.core.system.UserSession"/>
		<property name="actionPath" value="/public/your_xaction_path_and_file.xaction"/>
		<property name="actionOutputScope" value="session"/>
	</bean>
	<!-- End of UserSession startup actions. -->
    <!-- Start of Global startup actions. -->
    <!-- <bean class="org.pentaho.platform.engine.core.system.SessionStartupAction">
    	<property name="sessionType" value="org.pentaho.platform.engine.security.session.TrustedSystemStartupSession"/>
        <property name="actionPath" value="/public/your_xaction_path_and_file.xaction"/>
        <property name="actionOutputScope" value="global"/>
    </bean> -->
    <!-- End of Global startup actions. -->
```

You can see 3 groups starting by the tag <bean>. By default, they are commented and you can uncomment them if you want to have startup actions to run when the user performs a login or when Pentaho is starting. Each one of this groups is identified by the type and very important the action sequence path to be executed. The actionPath needs to be set, pointing to the folder/file of the xaction we want to be execute.  You will see session type is what makes the bid difference from group to group, and it can get the following values:

- org.pentaho.platform.web.http.session.PentahoHttpSession
- org.pentaho.platform.engine.core.system.UserSession
- org.pentaho.platform.engine.security.session.TrustedSystemStartupSession 

Let's see them as just: 

- PentahoHttpSession: runs the specified action sequence when a session is created using HTTP. Useful to run some actions, as creating a session variable to be used on a DSP when the user is logged in from a browser.

- UserSession: runs the specified action sequence when a user session is created. Useful to create or run actions like creating a session variable when a session is created, not specifically from the browser, but per instance when the scheduler need to run a report on behalf of the user.

- TrustedSystemStartupSession: runs the specified action sequence when the Pentaho Server is starting. Useful when you want to execute some operations at the start of the system. Please note that this is executed on the system, so it is globally applied, not for a specific user. 

if you upload the action sequence that is included as example and if you remove the comments from both PentahoHttpSession and UserSession, the session variable will be created when a session is created for a user, independently if it was created from the browser by the user, or by the Pentaho scheduler. 

Please keep in mind that you can also use the Startup Rule Engine to perform what was explainded here for the xaction.  

**Note**: Before making use of a multitenant environment when using Pentaho Community Edition, make sure to completely read the terms and conditions of the software licenses. 

Follow me at [Twitter](https://twitter.com/migfgaspar)

[Startup Actions, Action Sequences]: #

