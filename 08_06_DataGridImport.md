#QAFE Book

![qafelogo](http://www.qafe.com/wp-content/themes/qafe2013/img/logo.png)

### Importing data into a datagrid
In the datagrid component, there is a possibility to import data from a CSV file. 
In this how-to, we will explain how to do that.

#### Creating a datagrid
In QAML, first create a datagrid in the presentation tier.
As seen in the example below, we have a button and a datagrid with 2 columns defined in a verticallayout

On the datagrid we see a multitude of attributes where the following two are used to determine import behaviour. 

* import - allow importing of data into the datagrid
* * possible values are "true" and "false". The default value is "false"
* import-action - specifies if the imported data needs to replace existing data, or be appended.
* * possible values are "set" and "add". The default value for this attribute is "set".


```xml
<application-mapping xmlns="http://qafe.com/schema"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://qafe.com/schema http://www.qafe.com/schema/application-mapping.xsd">
	<presentation-tier>
		<view>
			<window id="window1" displayname="Hello World" width="500"  height="700">
				<rootpanel id="myRootPanel" width="400" height="300">
					<verticallayout>
						<button id="loadDatagridBtn" displayname="Load data" />
					    <datagrid id="datagrid" import="true" import-action="add">
					    	<column displayname="id" name="ID" id="idCol"/>
					    	<column displayname="name" name="NAME" id="nameCol" />
					    </datagrid>
					</verticallayout>
				</rootpanel>
				<events>	
					
				</events>
			</window>
		</view>
	</presentation-tier>
</application-mapping> 
```
