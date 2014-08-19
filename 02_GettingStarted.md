#QAFE Book

![qafelogo](http://www.qafe.com/wp-content/themes/qafe2013/img/logo.png)

## 2. Getting Started with QAFE

### 2.1 Setting up the Development Environment
QAFE Development is done in Eclipse. For the purpose of having a quickstart, we’ve provided an installer which installs Eclipse (Based on Oracle Enterprise Pack for Eclipse, OEPE 12, based on Eclipse 4.3) with all the necessary plug-ins. These installers are to be found on http://www.qafe.com/download


#### 2.1.1 Hardware requirements

To run QAFE the hardware requirements depends significantly on the number of users, applications expected and on the maximum number of required concurrent requests that the system will experience during peak hours. In addition, more CPU power, HD or memory will be needed as more QAFE components are added to the system configuration.

| Scenario      | CPU  | Memory  |Harddisk |
|---------------|------|---------|---------|
| QAFE Tutorial | Any modern CPU| 2 Gb |1,7 Gb |
| Developing in QAML| Any modern CPU | 1,5 Gb |50 Mb Per render engine (GWT, etc.) this amount of diskspace is used(1). |
| Running QAFE apps | Depending on the chosen server | 2 Gb | 50 Mb Per render engine (GWT, etc.) this amount of diskspace is used (2) |

> (1) Per render engine (GWT, etc.) this amount of diskspace is used.

> (2) Per render engine (GWT, etc.) this amount of diskspace is used.


#### 2.1.2 Software requirements


| Scenario |Operating System (3) |Java Virtual Machine |Web/App server (4) |Database |
|----------|-----------------|---------------------|---------------|---------|
|QAFE Tutorial | Compatible with JVM |JDK7 or higher |Cross-platform |Oracle XE|
|Developing in QAML |Compatible with JVM |JDK7 or higher |Cross-platform |Optional|
|Running QAFE apps| Compatible with JVM |JDK7 or higher |Cross-platform|Optional|

> (3) Any Operating System able to run Java 7 or higher (Linux, Windows, Solaris, Unix)

> (4) For running the QAFE tutorial the Apache Tomcat is desired. For developing or running QAFE applications a web or J2EE based application server running servlet specification v2.3 or higher (see also http://www.qafe.com/developer-docs/supported-platforms/)

### 2.2 Creating your first QAFE Application

Once you have an Eclipse installation on your machine with the plugins mentioned above, you are ready to create a simple QAML Project. In order to start working on QAML projects and using the QAML Builder plug-in you have to download an instance of the QAFE Platform and obtain a license key.

As a pre-requisite, QAFE Platform should be extracted to a location which can be pointed to from within a QAFE project. This location will be used to serve as Path to QAFE in following steps

Start creation of QAML Project by following the steps:


1. Choose from the menu  `File -> New -> QAFE Project`. A wizard will popup

![newproject](https://raw.githubusercontent.com/qafedev/qafedev.github.io/master/assets/images/newqafeproject.png)
1. Enter the name of your QAFE project and the name of your application (in this case `qafe-demo`)
1. The path to QAFE should point to the extracted version of QAFE Platform as mentioned above. The QAFE Platform is installed in the `<USER_HOME>/qafe` folder.
1. Click Finish to create the new QAFE Project.
1. To make sure that you have the correct Project structure, Right click on `qafe-demo -> Choose Maven -> Choose Update Project Configuration`.
1. Expand directory structure `qafe-demo -> src -> main -> webapp -> WEB-INF -> “qafe-demo”`. This is the folder where the QAFE source files will be (with extension .qaml)
1. In this folder on can find the `helloworld.qaml`

### 2.3 Running  your first QAFE Application

1. Before running your QAML application, you’ll need to build your application. This will create a directory with an assembly of your QAML application in QAFE Platform as well as a war which can be deployed on your server. To build your newly created QAML Project, Right Click on the project `qafe-demo`  > Choose `QAFE` and then `Build QAFE Application` as shown below.(After successful build, if you don’t see the target directory, do a refresh on the project)

> SHOW NEW POPUP

1. The QAML Builder plug-in also offers an option to run and test your application inside eclipse on a Jetty server.To run the project Right click on `qafe-demo` > Choose `Run As` -> Choose `QAFE Application` as shown below

> SHOW NEW POPUP

1. Now an instance of Jetty server is started with your QAML application assembly deployed on it. Default browser of your machine will be invoked with QAFE Application. You can also use `http://localhost:7070/qafe-demo/QAFEGWTWeb.jsp` and open QAFE Application in other browsers.
1. In the QAFE Application on browser under  Programs menu (upper left) you can see the qafe-demo through which is it possible to invoke the Hello World Qafe Application defined in `helloworld.qaml`


Congratulations: You now have run your first QAFE application.  In the next chapter we’ll dive into the details and use features in QAFE Applications.
