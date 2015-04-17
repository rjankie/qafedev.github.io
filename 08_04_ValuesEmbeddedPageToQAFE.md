#QAFE Book

![qafelogo](http://www.qafe.com/wp-content/themes/qafe2013/img/logo.png)

### Passing values from an embedded page to QAFE
*_NOTE: This document is for QAFE developers still using server-side processing in the engine._*

In QAFE it is possible to embed a webpage in qafe using the _<embed>_-tag. There might be a case, where certain values have to be passed back to QAFE application. This guide will explain how to achieve this using server-side processing.

Take for example this sample application including a refresh-button, a two textfields and a frame. The frame contains a textfield and a button. When the button within the frame is pressed, the value of the textfield within the frame is passed to the QAFE application data store. By pressing the refresh button, the textfields will be filled in based on the value within the frame.

![jspexample](https://github.com/qafedev/qafedev.github.io/raw/master/assets/images/passing-values-frame-example.png)

First up, make sure that the following dependencies are available in the pom.xml:
- javax.servlet.servlet-api (used version 2.3)
- com.qualogy.qafe.platform.qafe-core (used version 3.3.0)
- com.qualogy.qafe.platform.qafe-presentation (used version 3.3.0)

The QAFE dependencies can be found on repository.qafe.com and in the WEB-INF/lib-folder of the QAFE Engine. Also make sure that the servlet using the servlet API and the corresponding servlet mapping is added to the web.xml.

At the start-up of the application, a window session ID is generated for unique identification of the QAFE application within a page. This session ID and an application ID is available in the DOM of the HTML page. It can be used to access the QAFE application from inside a frame.

The frame is already loaded up on startup. When the button within the frame is clicked, a request is done to a servlet with the window session ID and the value of the textfield being passed. The window session ID can be retrieved from the DOM of the HTML page. There are two divs within HTML-body containing the application ID and the window session ID. 

Within the servlet, the application context of the QAFE application is retrieved based on the application ID specified in the _application-config.xml_. Based on the window session ID and the application ID, the ID of the application store is created. If a window ID is specified, the value will be stored in the user store, otherwise the global store is used. When clicking on the refresh-button in the QAFE application, the data is filled in the text fields.

These dependencies have to be added to the pom.xml:
```xml
<dependencies>
	<!-- Dependency is needed for the servlet to work properly -->
	<dependency>
		<groupId>javax.servlet</groupId>
		<artifactId>servlet-api</artifactId>
		<version>2.3</version>
		<scope>provided</scope>
	</dependency>

	<!-- Both dependencies are needed for storing the data in Application Store -->
	<dependency>
		<groupId>com.qualogy.qafe.platform</groupId>
		<artifactId>qafe-core</artifactId>
		<version>3.3.0</version>
		<scope>provided</scope>
	</dependency>

	<dependency>
		<groupId>com.qualogy.qafe.platform</groupId>
		<artifactId>qafe-presentation</artifactId>
		<version>3.3.0</version>
		<scope>provided</scope>
	</dependency>
</dependencies>
```

The corresponding application-config.xml is the following:
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<applications xmlns="http://qafe.com/schema"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://qafe.com/schema http://www.qafe.com/schema/application-context.xsd">

    <configuration name="external.properties" value="external.properties" />
    <configuration name="mode.development" value="true" />
    <configuration name="load.on.startup" value="qafeApp.helloWorld" />
    <configuration name="mode.mdi" value="false"/>

    <application name="System App" id="system_app">
        <application-mapping-file location="qafe-default-system-app.xml" />
    </application>

    <application name="test" id="qafeApp">
        <application-mapping-file location="test" />
    </application>
</applications>

```

The corresponding QAML-file is the following:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<application-mapping xmlns="http://qafe.com/schema"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://qafe.com/schema http://www.qafe.com/schema/application-mapping.xsd">

	<presentation-tier>
		<view>
			<window id="helloWorld" displayname="Hello World" width="300"
				height="250">
				<rootpanel id="rootPanelId">
					<verticallayout>
						<button id="refreshButtonId" displayname="Refresh" />
						<textfield id="txtGlobal" />
						<textfield id="txtUser" />
						<frame id="testIntegrationFrame" src="testIntegration.jsp"
							width="200" height="200" />
					</verticallayout>
				</rootpanel>
				<events>
					<event>
						<listeners>
							<listenergroup>
								<component ref="refreshButtonId" />
								<listener type="onclick" />
							</listenergroup>
							<!-- Check every 5 seconds whether the value has been changed using 
								a timer-event -->
							<!-- <listenergroup> <component ref="helloWorld" /> <listener type="ontimer" 
								> <listener-parameter name="time-out" value="5000" /> <listener-parameter 
								name="repeat" value="0" /> </listener> </listenergroup> -->
						</listeners>
						<set component-id="txtGlobal" ref="dummyKey" src="global" />
						<set component-id="txtUser" ref="inputField" src="user" />
					</event>
				</events>
			</window>
		</view>
	</presentation-tier>
</application-mapping> 
```

The code of the frame is the following:
```html
<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
	<input name="inputFieldName" id="inputField">
	<button name="btnSendInputField" onclick="getInputField()">
		Send Input Field
	</button>
	
	<script type="text/javascript">
		function getInputField() {
			var windowSessionIdDiv = window.parent.document.getElementById('winId');
			var inputFieldDiv = document.getElementById('inputField');
			var windowSessionId = windowSessionIdDiv.getAttribute("value");  
			var inputField = inputFieldDiv.value;
			
			document.location.href="test?&windowSessionId=" + windowSessionId + "&inputField=" + inputField;
		}
	</script>
</body>
</html>
```

The servlet code is the following:
```java
package test;

import java.io.IOException;
import java.util.Enumeration;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.qualogy.qafe.bind.core.application.ApplicationContext;
import com.qualogy.qafe.core.application.ApplicationCluster;
import com.qualogy.qafe.core.datastore.ApplicationLocalStore;
import com.qualogy.qafe.presentation.handler.executors.EventItemExecuteHelper;

public class TestServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
	private ApplicationContext applicationContext;
	// AppId in the application-context you want to use.
	private static final String APPLICATION_ID = "qafeApp";

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp)
			throws ServletException, IOException {
		// Retrieve the request parameters from the servlet request 
		Map<String, Object> parameters = new HashMap<String, Object>();
		Enumeration paramNames = req.getParameterNames();
		while (paramNames.hasMoreElements()) {
			String paramName = (String)paramNames.nextElement();
			String paramValue = req.getParameter(paramName);
			parameters.put(paramName, paramValue);
		}
		
		// document.location.href="test?windowSessionId=" + windowId + "&inputField=" + inputField;
		String windowSessionId = (String) parameters.get("windowSessionId");
		String inputField = (String) parameters.get("inputField");
		
		ApplicationContext qafe = getApplicationContext();
		String globalDataStoreId = EventItemExecuteHelper.generateLocalStoreId(windowSessionId, qafe, null);
		// helloWorld is the window ID specified in the QAML-file
		String windowDataStoreId = EventItemExecuteHelper.generateLocalStoreId(windowSessionId, qafe, "helloWorld");
		// Will be stored as a global variable
		ApplicationLocalStore.getInstance().store(globalDataStoreId, "dummyKey", "dummyValue");
		// Will be stored as a user variable
		ApplicationLocalStore.getInstance().store(windowDataStoreId, "inputField", inputField);
		
	}
	
	
	/**
	 * Retrieve the proper application context within the QAFE application based on the ID defined in the application-config
	 * See also the application-config.xml
	 * @return ApplicationContext
	 */
	protected ApplicationContext getApplicationContext() {
		if (applicationContext == null) {
			Iterator itrAppContext = ApplicationCluster.getInstance().iterator();
			while (itrAppContext.hasNext()) {
				ApplicationContext appContext = (ApplicationContext)itrAppContext.next();
				if (APPLICATION_ID.equals(appContext.getId().toString())) {
					applicationContext = appContext;
				}
			}	
		}
		return applicationContext;
	}	
}
```
