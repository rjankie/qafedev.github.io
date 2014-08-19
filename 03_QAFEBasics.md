#QAFE Book

![qafelogo](http://www.qafe.com/wp-content/themes/qafe2013/img/logo.png)

## 3. QAFE Basics
In the previous chapter the software for QAFE Development was installed. Then a simple project project was created and the QAFE HelloWorld application was shown at runtime.

In this chapter we’ll dive into the anatomy of the QAFE HelloWorld Application.




```XML
<applications  xmlns=http://qafe.com/schema
xmlns:xsi=http://www.w3.org/2001/XMLSchema-instance xsi:schemaLocation="http://qafe.com/schema
http://www.qafe.com/schema/application-context.xsd">

  <application name="apps" id="system_app" >
    <application-mapping-file location="qafe-default-system-app.xml"/>
  </application>


  <application name="Hello World" id="myApp" >
    <application-mapping>
       <presentation-tier>
         <view>
          <window id="myWindow" displayname="Hello World">
            <rootpanel>
              <verticallayout>
           	   <button id="myButton" displayname="HellWorld"/>
           	 </verticallayout>
            </rootpanel>
          </window>
        </view>
      </presentation-tier>
    </application-mapping>
  </application>

</applications>
```


The applications that are needed are specified within the application tag in the XML. The application tag has an id and name attribute. The name attribute is shown in the system application menu shown in the virtual desktop page which is used to invoke the application. The definition of the application is done within the application-mapping tag.

In the code sample, there are in two applications: system_app and hello_world. The xml file mentioned as attribute value in the application-mapping-file tag with the id system_app represents the system application menu.

We advice you to place all your QAML files (definition of your applications based on XML) including the application-config.xml file outside the QAFE software deployment location. This may become handy when the QAFE software needs to be updated as it will make sure that your QAML files are not affected when QAFE is updated. After updation of QAFE (if any), you will only need to redefine the location of the application-config.xml in the overwritten web.xml file.

> NOTE: If you don't want to have the files in your WEB-INF directory, you can modify the application-config.xml to other file locations as long as the file is reachable through a valid URL.

If QAML contain resources that refer to their statement files, then the statements files need to be located in the same directory (since the statements files are always relative file locations).
If there is an application-config file which contains an application which on its turn has multiple filelocations for the application-mapping, make sure that the statements file in the resources are defined in a correct and concise manner.

Example:

```XML
<application id="system_app" name="sysapp">
	<application-mapping-file
	        location="file:///f:/test-web/qafe-default-system-app.xml"/>
</application>

```
