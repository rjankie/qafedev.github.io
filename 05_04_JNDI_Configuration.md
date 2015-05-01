# JNDI Configuration
When an application is deployed on multiple machines using other databases, it is useful to let the system decide which database to use. JNDI is introduced to handle that section. This page will describe how to set up a QAFE application for this and how to configure Jetty, Tomcat and Weblogic for a proper application.

<!-- toc -->

* [Resource Tier](#resource-tier)
* [Jetty](#jetty)
* [Tomcat](#tomcat)
* [Weblogic](#weblogic)

<!-- toc stop -->

## Resource Tier
Before using JNDI in the QAFE application, the JNDI connection has to be specified in the resource-tier. When using a database resource directly, the resource-tier contains a drivermanager-datasource as seen in the example below.

```XML
<resource-tier>
	<resources>
		<drivermanager-datasource id="ModuleDataBaseService"
		statements-file-url="hsd0004f.fmb-statements.xml"
		url="jdbc:oracle:thin:@localhost:1521:XE"
		username="hdemo65"
		password="hdemo65"
		driver-classname="oracle.jdbc.OracleDriver"/>
	</resources>
</resource-tier>
```


The drivermanager-datasource has to be replaced with a jndi-datasource. By using JNDI the database connection looks like this:

```XML
<resource-tier>
	<resources>
		<jndi-datasource id="ModuleDataBaseService"
		statements-file-url="hsd0004f.fmb-statements.xml"
		jndiname="jdbc/oracleQAFE"/>
	</resources>
</resource-tier>
```


## Jetty
An additional file *jetty-env.xml* needs to be added to the src/main/WEB-INF-folder of the project. Jetty will pick this file up automatically. This file should contain the DB connections as described below.

```XML
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE Configure PUBLIC "-//Jetty//Configure//EN" "http://www.eclipse.org/jetty/configure.dtd">
<Configure class="org.mortbay.jetty.webapp.WebAppContext">
	<New id="oracleQAFE" class="org.mortbay.jetty.plus.naming.Resource">
		<Arg></Arg>
		<Arg>jdbc/oracleQAFE</Arg>
		<Arg>
			<New class="oracle.jdbc.pool.OracleDataSource">
				<Set name="URL">jdbc:oracle:thin:@localhost:1521:XE</Set>
				<Set name="User">hdemo65</Set>
				<Set name="Password">hdemo65</Set>
			</New>
		</Arg>
	</New>
</Configure>
```

This configuration will work for Jetty 6. Jetty 7 and later uses different classes where *org.mortbay.jetty* has been changed into *org.eclipse.jetty*.

## Tomcat
To get JNDI working in Tomcat, a few changes have to be made. First, make sure that the JDBC driver found in the QAFE Engine (ojdbc5.jar) is available in Tomcat's *lib*-folder. Next up the context.xml of Tomcat in the *conf*-folder should be modified. A resource has to be added like in the example below.
```XML
<Resource name="jdbc/oracleQAFE" auth="Container"
		  type="javax.sql.DataSource" driverClassName="oracle.jdbc.OracleDriver"
		  url="jdbc:oracle:thin:@localhost:1521:XE"
		  username="hdemo65" password="hdemo65" maxActive="20" maxIdle="10" accessToUnderlyingConnectionAllowed="true"
		  maxWait="-1"/>
```

Additionally the web.xml needs to be modified by adding a resource-ref as seen in the example below.
 
```XML		 
<resource-ref>
	<description>QAFE Oracle Data Source</description>
	<res-ref-name>jdbc/oracleQAFE</res-ref-name>
	<res-type>javax.sql.DataSource</res-type>
	<res-auth>Container</res-auth>
</resource-ref>
```

Finally a small change needs to be done to the resource-tier. The JNDI name of the resource needs to be prefixed with *java:/comp/env/*, so for example *java:/comp/env/jdbc/oracleQAFE*

For more details on how to properly use JNDI in Tomcat, please contact the [JNDI page](https://tomcat.apache.org/tomcat-7.0-doc/jndi-datasource-examples-howto.html "Tomcat website") on the Tomcat website.

## Weblogic
To get JNDI working on Weblogic, data sources have to be created on the Weblogic server. When logged in on the server, open the Data source page under services. Create a new data source by clicking on the new-button and following the wizard. Make sure that *oracle.jdbc.pool.OracleDataSource* is used as driver class name. The data source should be connected to the correct cluster. Add the correct cluster as a target.
