#QAFE Book

![qafelogo](http://www.qafe.com/wp-content/themes/qafe2013/img/logo.png)

### Passing values from an embedded page to QAFE
*_NOTE: This document is for QAFE developers still using server-side processing in the engine._*
In QAFE it is possible to embed a webpage in qafe using the _<embed>_-tag. There might be a case, where certain values have to be passed back to QAFE application. This guide will explain how to achieve this.

Take for example the sample application below.
![jspexample](https://github.com/qafedev/qafedev.github.io/raw/master/assets/images/passing-values-frame-example.png)

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
			var windowIdDiv = window.parent.document.getElementById('winId');
			var inputFieldDiv = document.getElementById('inputField');
			var windowId = windowIdDiv.getAttribute("value");  
			var inputField = inputFieldDiv.value;
			
			document.location.href="test?&windowSessionId=" + windowId + "&inputField=" + inputField;
		}
	</script>
</body>
</html>
```

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
