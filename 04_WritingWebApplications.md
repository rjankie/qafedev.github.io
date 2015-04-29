#QAFE Book

![qafelogo](assets/images/logo.png)


## 4 Adding events your applications



<!-- toc -->

* [4 Adding events your applications](#4-adding-events-your-applications)
  * [Multilingual Support](#multilingual-support)
  * [Styling](#styling)
  * [Business Actions](#business-actions)
  * [Services](#services)
  * [ResourceTier](#resourcetier)
    * [Database](#database)
    * [drivermanager-datasource resource.](#drivermanager-datasource-resource)
    * [jndi-datasource resource.](#jndi-datasource-resource)
    * [Statements file.](#statements-file)
    * [Function Call Example:](#function-call-example)
    * [Procedure Call Example](#procedure-call-example)
    * [Calling procedures with Oracle Object type as IN/OUT:](#calling-procedures-with-oracle-object-type-as-inout)
  * [Java](#java)
    * [javaclass resource.](#javaclass-resource)
    * [spring resource.](#spring-resource)
  * [Adapters](#adapters)
  * [Transactions](#transactions)
  * [Exceptions](#exceptions)

<!-- toc stop -->


Events are used in order to associate actions to components. All the events that need to be defined are included in the *events* tag in QAML file.

> There are global and local events. We focus now on local events. Local to the window that is.

```XML
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
      <events>
        <event id="myEventId">
              <listeners>
                    <listenergroup>
                          <component ref="myButton" />
                          <listener type="onclick" />
                    </listenergroup>
              </listeners>
              <!-- built in function declarations will go here -->
        </event>
    </view>
  </presentation-tier>
</application-mapping>
```

| attribute | description |
|-------------------------|
| id |Id of the event which can be used as a reference to trigger this event from another part of the code|
|description|To hold information regarding the event|
|src-id |Variable declaration to hold the id of the component  which triggered the event. |
|src-name| Variable declaration to hold the name of the component which triggered the event. |
|src-value| Variable declaration to hold the value of the component which triggered the event.|
|src-listener-type |Variable declaration to hold the listener type which triggered the event.|



The attributes described above are optional for`<event></event>` tag.

Example Code:
```XML
<events>
	<event id="myEventId" src-id="idVar" src-name="nameVar" src-value="valueVar" 	*
         src-listener-type="listenerTypeVar">
	      <listeners>
	            <listenergroup>
	                  <component ref="myComponentId" />
                    <listener type="onclick" />
	            </listenergroup>
	      </listeners>          
		    <dialog type="info" *>
          	<title value="My Title" />
              <message value="${idVar}+${nameVar}+${valueVar}+${listenerTypeVar}">
              </message>
	      </dialog>
	</event>
</events>
```

The ``event`` tag must have ``listeners`` tag which holds the listener groups associated with that event. The components which needs to trigger that event is referenced in the `<listenergroup>` tag inside the listeners. Multiple components can refer to a single listener. The `<listener>` designates the type of action that triggers the associated event. A  listeners element can have more than one listener groups. The type of actions that a listener can handle are shown in the table below.

<table>
  <tr>
    <td>onclick </td>
    <td>When onclick is selected, the component will react on single mouse click.</td>
  </tr>
  <tr>
    <td>onenter</td>
    <td>When onenter is selected, the component will react the keyboard press of the "ENTER" key.</td>
  </tr>
  <tr>
    <td>onexit</td>
    <td>When onexit is selected, the component will react when the current component loses focus (by keyboard input or mouse!).</td>
  </tr>
  <tr>
    <td>onchange</td>
    <td>When onchange is selected, the component will trigger the event when the component value is changed upon user input.</td>
  </tr>
  <tr>
    <td>onfocus</td>
    <td>When onfocus is selected, the component will react when it receives the focus either by mouse or by keyboard.</td>
  </tr>
  <tr>
    <td>onkeydown</td>
    <td>When onkeydown is selected, the keyboard input will trigger an event when the key is down. See below for supported keys. Additional listener parameters can be added to listen to a specific key by adding the following element in the body of the listener:

  <listener type="onkeydown">
    <listener-parameter name="key" value="KEY_DOWN" />
  </listener></td>
  </tr>
  <tr>
    <td>onkeypress</td>
    <td>When onkeypress is selected, the keyboard input will trigger an event when the key (printable char) is pressed. Additional listener parameters can be added to listen to a specific key by adding the following element in the body of the listener:

  <listener type="onkeypress">
    <listener-parameter name="key" value="A" />
  </listener></td>
  </tr>
  <tr>
    <td>onkeyup</td>
    <td>When onkeyup is selected, the keyboard input will trigger an event when the key is released.  See below for supported keys. Additional listener parameters can be added to listen to a specific key by adding the following element in the body of the listener:

  <listener type="onkeyup">
    <listener-parameter name="key" value="KEY_UP" />
  </listener></td>
  </tr>
  <tr>
    <td>onload</td>
    <td>When the onload is selected, the window will execute this event. This only works on windows, so the component ref should refer to a window, otherwise this onload event doesn't have any effect.</td>
  </tr>
  <tr>
    <td>onmouse-down</td>
    <td>When the onmouse-down is selected, the event will be triggered on pressing the mouse button. The internal variable $MOUSE can be used to get the X and Y coordinates with respectively $MOUSE.x and $MOUSE.y</td>
  </tr>
  <tr>
    <td>onmouse-move</td>
    <td>When the onmouse-move is selected, the event will be triggered on moving  the mouse. The internal variable $MOUSE can be used to get the X and Y coordinates with respectively $MOUSE.x and $MOUSE.y</td>
  </tr>
  <tr>
    <td>onmouse-exit</td>
    <td>When the onmouse-exit is selected, the event will be triggered when the mouse exits the boundaries of the component. The internal variable $MOUSE can be used to get the X and Y coordinates with respectively $MOUSE.x and $MOUSE.y</td>
  </tr>
  <tr>
    <td>onmouse-enter</td>
    <td>When the onmouse-enter is selected, the event will be triggered when the mouse enters the boundaries of the component. The internal variable $MOUSE can be used to get the X and Y coordinates with respectively $MOUSE.x and $MOUSE.y</td>
  </tr>
  <tr>
    <td>onmouse-up</td>
    <td>When the onmouse-up is selected, the event will be triggered when the mouse button is released. The internal variable $MOUSE can be used to get the X and Y coordinates with respectively $MOUSE.x and $MOUSE.y</td>
  </tr>
  <tr>
    <td>ontimer</td>
    <td>
When the ontimer is selected, the event will be triggered when the time-out parameter, specified in milliseconds, is reached.
Additional listener parameters can be added to control the behaviour
by adding the following elements in the body of the listener:


- time-out (milliseconds): 
execute the event when this parameter is reached, for example:
		<listener-parameter name="time-out" value="5000" />

- repeat (integer, default is 1):
0 means repeat continuously, 1 means repeat once only; for example:
		<listener-parameter name="repeat" value="1" /> </td>
  </tr>
  <tr>
    <td>onunload</td>
    <td>When the onunload is selected, the event will be triggered when the window closes. This listener type only applies to windows. See also "onload".</td>
  </tr>
  <tr>
    <td>onfinish</td>
    <td>This trigger is activated/invoked when using file-upload component. When the file is uploaded to the local memory this event is triggered.</td>
  </tr>
  <tr>
    <td>onfetchdata</td>
    <td>This event is associated with sorting property of datagrid column. If the sortable attribute of datagrid column is true then this event must be defined followed by the business action to fill the datagrid with data in order to make it functional.</td>
  </tr>
</table>


Supported keys (non-printable chars) for onkeydown and onkeyup:

- *F1, F2, F3,F4,F5,F6,F7,F8,F9,F10,F11,F12*
- *KEY_SPACE,KEY_INSERT,KEY_DELETE,KEY_HOME,KEY_END,KEY_PAGEUP,KEY_PAGEDOWN,KEY_UP,KEY_DOWN,KEY_LEFT,KEY_RIGHT,KEY_BACKSPACE,KEY_ENTER,KEY_ESCAPE,KEY_TAB,KEY_NUMLOCK*
- *KEY_ALT* (can be used in combination with another key, like *KEY_ALT + A*)
- *KEY_CTRL* (can be used in combination with another key, like *KEY_CTRL + F2*)  
- *KEY_SHIFT* (can be used in combination with another key, like *KEY_SHIFT + KEY_LEFT*)  

Example Code:
```XML
<events>
	<event id="myEventId">
	      <listeners>
	            <listenergroup>
	                  <component ref="myComponentId" />
	                  <listener type="onclick" />
	            </listenergroup>
	      </listeners>
	      <!-- built in function declarations will go here -->
	</event>
</events>
```

#####  Built-Ins

Built-Ins include business actions, control statements and other dynamic changes. BuildIns are usually declared in event definitions. The process that needs to be executed when an event occurs is specified using built in functions.

######  Business Actions

Reference to a defined business action. It can be specified using the `<business-action>` tag. This results in the execution of the business action. Parameters can be passed to a business action using the `<in>` tag and output can be collected as a result of the business action execution in the `<out>` tag. Multiple `<in>` tags are used to pass more than one parameters. The `<in>` and `<out>` tags can have the following attributes

<table>
  <tr>
    <td>name</td>
    <td>Name of the input variable which will be used as reference in the business action definition</td>
  </tr>
  <tr>
    <td>ref </td>
    <td>Id of the referencing component</td>
  </tr>
  <tr>
    <td>src </td>
    <td>Source of the component. Default value is pipe. You can also specify source as component, message or user</td>
  </tr>
  <tr>
    <td>adapter </td>
    <td>Reference id of the adapter class defined. As a result of this the variable will be casted to that particular class.</td>
  </tr>
  <tr>
    <td>class </td>
    <td>Can be used to directly specify the class of the parameter</td>
  </tr>
  <tr>
    <td>expr </td>
    <td>Python language expressions can be used to evaluate value for the variable</td>
  </tr>
  <tr>
    <td>type </td>
    <td>Supported types are integer, int, long, double, character, char, boolean and string.</td>
  </tr>
  <tr>
    <td>use-when-not-set </td>
    <td>Used to set default value</td>
  </tr>
  <tr>
    <td>value </td>
    <td>Specify value of the input variable if not using any reference.</td>
  </tr>
</table>


Example Code:
```XML
<event id="myEventId">
      <listeners>
            <listenergroup>
                  <component ref="myComponentId" />
                  <listener type="onclick" />
            </listenergroup>
      </listeners>
      <business-action ref="myBusinessActionId">
            <in name="parameterName" ref="myComponentId" src="component" />
            <out name="businessActionReturnName" ref="businessActionReturnId" />
      </business-action>
      .
      .
</event>
```

QAFE has an internal variable $COUNT which can be set to datastore such that the subsequent business action call executing a select statement should give the count of records as output. Clearing $COUNT from datastore and executing the same business action would give the result set after select query execution. Following is the syntax to be followed.
```XML
<event>
  <listeners>
    <listenergroup>
			<component ref="<componentid>"/>
			<listener type="<listenerType>"/>
		</listenergroup>
	</listeners>
	<store name="$COUNT" value="yes"/>
	<!-- Business action for a select query →
	<business-action ref="<xyzBusinessAction>">
		<in name="<aInput>" ref="<someCompoonent>"/>
		<out name="<resultOfBusinessAction>"
			 ref="<refVariableInBusinessAction>"/>
	</business-action>
	<store-clear name="$COUNT" target="pipe"/>
	<business-action ref="<xyzBusinessAction>">
		<in name="<aInput>" ref="<someCompoonent>"/>
		<out name="<resultOfBusinessAction>"
       			 ref="<refVariableInBusinessAction>"/>
	</business-action>
</event>        
```

######  Control Statements

Besides business actions, control statements can be used as part of event execution. Control statements can also be part of business definition. Supported control statements are if, for and switch.

IF:

The if condition is declared by using the <if> tag and is evaluated against the expression within the <expression> tag. The expression tag can have <placeholder/> elements which can be used to declare and initialize variables that need to be used for evaluation. The attributes that can be used for expression and placeholder tags are as follows

<table>
  <tr>
    <td>adapter</td>
    <td>Reference id of the adapter class defined. As a result of this the variable will be casted to that particular class.</td>
  </tr>
  <tr>
    <td>class</td>
    <td>Can be used to directly specify the class of the parameter</td>
  </tr>
  <tr>
    <td>expr</td>
    <td>Python language expressions can be used to evaluate value for the variable</td>
  </tr>
  <tr>
    <td>ref</td>
    <td>Id of the referencing component</td>
  </tr>
  <tr>
    <td>src</td>
    <td>Source of the component. Default value is pipe. Other options are component, message or user</td>
  </tr>
  <tr>
    <td>type</td>
    <td>Supported types are integer, int, long, double, character, char, boolean and string.</td>
  </tr>
  <tr>
    <td>value</td>
    <td>Can directly specify a value.</td>
  </tr>
  <tr>
    <td>name</td>
    <td>Name of the input variable which will be used as reference in the expression tag. Expression tag itself does not support this attribute.</td>
  </tr>
</table> 

Example Code - IF:
```XML
<event id="myEventId">
	<listeners>
    <listenergroup>
   	  <component ref="myComponentId" />
      <listener type="onclick" />
    </listenergroup>
  </listeners>

      <if>
   	     <expression expr="${myPlaceHolderVarName} == 'valueToCheckedWith'">
         	     <placeholder name="myPlaceholderVarName" ref="myComponentId" />
         </expression>
         <results>
	          <result value="true">
         	  	   <!-- Action to be taken as result is true -->
         	  </result>
         	  <result value="false">
         	  	   <!-- Action to be taken as result is false -->
         	  </result>
         </results>
      </if>
      .
      .

<event>
````

SWITCH:

Switch cases can be used in the QAML code using the `<switch>` tag. The expression tag can be used to evaluate data in the same way as in the <if> tag. Only difference is that, instead of comparing the expression to a true or false value, the result of expression in *switch *can be compared to the actual value that is expected. The arguments that can be used in *expression* and *palceholder* tag are same as explained in *if *tag

Example Code - SWITCH
```
<switch>
	  <expression expr="${myPlaceHolderVarName}">
	   <placeholder name="myPlaceholderVarName" ref="myComponentId" />
	  </expression>
	  <results>
            <result value="actualvalueOne">
              <!-- Action to be taken as part of this result -->
            </result>
            <result value="actualvalueTwo">
                  <!-- Action to be taken as part of this result -->
            </result>
			.
			.
    </results>
</switch>
```


FOR:

For loops are specified using  `<iteration>` tag. The logic that need to be iterated is specified within the <iteration> tag. Following are the attributes for the iteration tag

<table>
  <tr>
    <td>begin</td>
    <td>Begin count for the iteration.</td>
  </tr>
  <tr>
    <td>condition</td>
    <td>Condition that needs to be checked to continue iteration</td>
  </tr>
  <tr>
    <td>end</td>
    <td>End count for the iteration</td>
  </tr>
  <tr>
    <td>increment</td>
    <td>increment step for the iteration</td>
  </tr>
  <tr>
    <td>items-ref</td>
    <td>Reference of the component on which iteration has to be performed</td>
  </tr>
  <tr>
    <td>items-src</td>
    <td>Source of the component. Default value is pipe. Other options are component, message or user</td>
  </tr>
  <tr>
    <td>var</td>
    <td>Temporary variable which can be used as handle to the reference component to access the component items</td>
  </tr>
  <tr>
    <td>var-index</td>
    <td>Can be used to specify a name which will give the index of the iteration</td>
  </tr>
</table>


Example Code - ITERATION:

Usage syntax example in Event Body:
```XML
<iteration items-ref="result" begin="0" increment="1" var="tmp" var-index="abc">
  <if>
    <expression expr="${currentAddress} == ‘someVAlue’">
      <placeholder name="currentAddress"
                   ref="resultFromBA[${abc}].columnName"/>
    </expression>
    <results>
      <result value="true">
	       <business-action id="aBusinessAction">
           <in name="inputVar" ref="tmp.columnName" />
        </business-action>
        <dialog>
          <title value="Success"/>
          <message value="Index is ${abc}"/>
        </dialog>
      </result>
    </results>
  </if>
</iteration>
```

Usage syntax example in Business Tier:
```XML
<business-action id="forloop-begin2-end4-increment2">
      <iteration begin="2" end="4" increment="2" tems-ref="alist" var="tmp">
            <service ref="serviceId" method-ref="refOfMethodInService">
                  <in name="name" ref="tmp.name" />
            </service>
      </iteration>
</business-action>
```

PYTHON EXPRESSIONS:

It is possible to evaluate data by expressions in python format.The python syntax can be specified in the *expr *attribute of *expression *tag. Some python expression evaluations frequently used in QAML are as follows.

Usage of _or_
```XML
<expression expr="${valueOne} == 'true' or ${valueTwo} == 'true'"/>
```
Usage of _and_
```XML
<expression expr="${valueOne} == 'true' and ${valueTwo} == 'true'"/>
```
Usage of _not_
```XML
<expression expr="(not (${valueOne} == None)) "/>
```
Further python expressions can be refered [here](http://docs.python.org/reference/expressions.html).

#####  Reusable events

The events that are defined can be reused. The attribute *id *of an event is used to refer the event from another part of the code. The event will be executed irrespective of the listener specified in the referencing event.

```XML
<event id="myEventOne">
	<listeners>
		<listenergroup>
	    	<component ref="myButtonOne"/>
		    <listener type="onclick"/>
		</listenergroup>
	</listeners>
	<dialog>
    	<title value="Title of Dialog Box"/>
	    <message value="Message shown in the dialog box"/>
   </dialog>
</event>

<event id="myEventTwo">
	<listeners>
    	<listenergroup>
	    	<component ref="myButtonTwo" />
	        <listener type="onclick" />
        </listenergroup>
   </listeners>
   <event ref="myEventOne"/>

</event>
```
In the above code the event with id as myEventOne is referenced as a part of event execution in myEventTwo.

#####  common Built-Ins

######  <set>

The `<set>` tag is used to set values to a particular component dynamically. Following are the attributes for set tag

<table>
  <tr>
    <td>action</td>
    <td>This attribute can have ''set'' or ''add'' value. if action=''set'' then it means to assign the value to an existing variable if the variable is present else will create and assign the value and if action=''add'' it adds variable with the value to the target. Target specifies the datastore. eg; pipe, user, message or component. Default value is ''set''</td>
  </tr>
  <tr>
    <td>adapter</td>
    <td>Reference id of the adapter class defined. As a result of this the variable will be casted to that particular class.</td>
  </tr>
  <tr>
    <td>class</td>
    <td>Can be used to directly specify the class of the parameter</td>
  </tr>
  <tr>
    <td>component-id</td>
    <td>Id of the component to which the value has to be set</td>
  </tr>
  <tr>
    <td>expr</td>
    <td>Python language expressions can be used to evaluate value for the variable</td>
  </tr>
  <tr>
    <td>name</td>
    <td>name of the component</td>
  </tr>
  <tr>
    <td>ref</td>
    <td>Id of the referencing component</td>
  </tr>
  <tr>
    <td>src</td>
    <td>Source of the component. Default value is pipe. You can also specify source as component, message or user</td>
  </tr>
  <tr>
    <td>type</td>
    <td>Supported types are integer, int, long, double, character, char, boolean and string.</td>
  </tr>
  <tr>
    <td>value</td>
    <td>Can directly specify a value.</td>
  </tr>
</table>


A simple usage example is as follows:

<set component-id="myComponent"  value="aValue"/>

Rules applied while setting a value to a component:

* If `component-id` attribute references to: 

    * an editable component (like a TextField): the value will be set

    * a non-editable component (like a Label):  the ``displayname`` will be set

    * a container component (like a Panel): the ``displayname`` will be set

* If `name` attribute references to the  :

    * editable component (like a TextField): the value will be set

    * non-editable component (like a Label): the ``displayname`` will be set 

    * container component (like a Panel): the value  or ``displayname`` of all child components with the name attribute will be set. 

######  ``<store>``

The store tag acts like the concept of session in an application. The store tag in QAFE can retain data in local and global scope according to the target specification. If the target is specified as pipe then it represents local scope for the data stored and the variable will only be available within an event. If the target is specified as user, then the data will be available across the events and across windows in the same application. Following are the attributes of `<store>`

<table>
  <tr>
    <td>action</td>
    <td>This attribute can have ''set'' or ''add'' value. if action=''set'' then it means to assign the value to an existing variable if the variable is present else will create and assign the value and if action=''add'' it adds variable with the value to the target. Target specifies the datastore. eg; pipe, user, message or component. Default value is ''set''</td>
  </tr>
  <tr>
    <td>adapter</td>
    <td>Reference id of the adapter class defined. As a result of this the variable will be casted to that particular class.</td>
  </tr>
  <tr>
    <td>class</td>
    <td>Can be used to directly specify the class of the parameter</td>
  </tr>
  <tr>
    <td>expr</td>
    <td>Python language expressions can be used to evaluate value for the variable</td>
  </tr>
  <tr>
    <td>field</td>
    <td>Only used for action=''delete'', it refers to a data member with a certain value which is determined either by the value or ref attribute</td>
  </tr>
  <tr>
    <td>name</td>
    <td>name of the variable in which the value is maintained</td>
  </tr>
  <tr>
    <td>ref</td>
    <td>Id of the referencing component</td>
  </tr>
  <tr>
    <td>src</td>
    <td>Source of the component. Default value is pipe. You can also specify source as component, message or user</td>
  </tr>
  <tr>
    <td>target</td>
    <td>Target datastore to retain the variable and value. you can specify values to target attribute as user, pipe, component or message</td>
  </tr>
</table>


usage is as follows:

`<store name="varName" ref="myComponent" src="pipe" target="pipe"/>`

######  ``<store-clear>``

When store tag is used to maintain data in global scope, the QAML developer have to make sure that the data is removed after use. This can be done using the store-clear tag. Clearing of the data from the global scope can be triggered through appropriate events like onunload.

If the name of the data variable is not specified then the tag will clear all the stored global data.

Usage is as follows:

`<store-clear name="GlobalDataVariableName" target="pipe"/>`

Following are the attributes of `<store-clear>`

<table>
  <tr>
    <td>name</td>
    <td>name of the variable to be cleared.</td>
  </tr>
  <tr>
    <td>target</td>
    <td>Target datastore where the variable was stored. Default value of target is user.</td>
  </tr>
</table>


#####  Other Builtins

######  `<call-script>`

The [`<call-script>`][call-script] tag is used to call script functions available to your web-application. This tag accepts two attributes the `function` and the `type`. The former is required the latter is not. The `type` attributed indentifies what language the script is in, currently the only supported language is JavaScript. JavaScript is also the default when the `type` attribute is ommited. The `function` attribute identifies the function to be called. If the function accepts parameters, these can be specified using [`<param>`][param] child elements. These elements accept, among others, a `type` and a `value` attribute. Of both the value is a string. The string value of the `type` attribute must be one of: `integer`, `int`, `long`, `double`, `character`, `char`, `boolean` and `string`.

```XML
<call-script type="javascript" function="myJavaScriptFunction">
	<param type="string" value="Hello World!"/>
	<param type="integer" value="42"/>
</call-script>
```

[call-script]: http://www.qafe.com/static/documentation/api/application-context_files/schemas/application-mapping_xsd/elements/call-script.html "call-script"
[param]: http://www.qafe.com/static/documentation/api/application-context_files/schemas/application-mapping_xsd/elements/param.html "param"

######  `<clear>`

The `<clear>` tag is used to clear the values of the component. The reference of the component whose value needs to be cleared is mentioned as ``ref`` attribute of the tag as shown below. 

Clearing varies with respect to the component it is being cleared. In most of the components clear will remove the existing values. But in Listbox or Dropdown clear will only deselect all the selections made so far.
```XML
<clear ref="myComponentId"/> 
```
   

Rules applied for clearing:

* If the ``ref`` attribute references to the id of : 

    * an editable component (like a TextField): the value will be cleared

    * a non-editable component (like a Label): won't do anything

    * a containment component (like a Panel): the value of all editable components will be cleared 

* If the ``ref`` attribute references to the name of :

    * an editable component (like a TextField): the value will be cleared

    * a non-editable component (like a Label): the ``displayname`` will be cleared 

    * a container component (like a Panel): the ``value`` or ``displayname`` of all child components with the name attribute will be cleared 

###### `<closewindow>`

The `<closewindow>` tag is used to close a window. The ``ref``attribute of the tag is used to specify the window id corresponding to the window that needs to be closed. Usage is as follows
```XML
<closewindow ref="myWindowId"/>
```
######  `<comments>`

As the name suggests, the tag is used to introduce comments in the code. Usage is as follows
```XML
<comments><![CDATA[Text that is needed to be the comment]]></comments>
```
######  `<copy>`

This tag is used to copy value of one component to another. Usage is as shown.
```XML
<copy from="sourceComponentId" to="destinationComponentId"/>
```

######  `<dialog>`

This tag is used to display dialog box if and when required. The tag must have the title and message properties defined. The message can have values of component by using variables declared in ``placeholder`` or ``store`` tags.

```XML
<dialog>
	<title value="Title of Dialog Box"/>
	<message value="Message shown in the dialog box"/>
</dialog>
```
######  `<error-handler>`

This tag is used as an exception handler. Services which are expected to cause exceptions are declared inside the `<error-handler>` tag. An Error handler tag can have the following attributes.

<table>
  <tr>
    <td>error-ref</td>
    <td>This refers to the id of the error tag defined in the integration tier.</td>
  </tr>
  <tr>
    <td>id</td>
    <td>Id of the error-handler tag itself</td>
  </tr>
  <tr>
    <td>final-action</td>
    <td>This attribute can have values rethrow to rethrow the error or swallow to ignore the error. </td>
  </tr>
</table>

```XML
<error-handler error-ref="errorReference" id="myErrorHandlerId">
	<service method-ref="myMethodInServiceId" ref="myServiceId">
		<!-- In and out parameters goes here -->                     
	</service>
</error-handler>
```

######  `<focus>`

 This tag is used to set cursor focus on a particular component
```XML
<focus ref="myComponentId"/>
````

######  `<openwindow>`

Further windows can be opened as internal window to QAFE or as an external window using this tag. Attributes of `<openwindow>` tag are as follows.

<table>
  <tr>
    <td>external</td>
    <td>Can be true or false and decides weather the window has to be an external or internal window.</td>
  </tr>
  <tr>
    <td>placement</td>
    <td>This attribute can take values as centered, center-cascade or tiled. Windows will be placed accordingly. For external windows tiled placement is not valid.</td>
  </tr>
</table>


Javascript features such as height, width, top, left position of the window can be specified in the values attribute of the `<params>` tag, which is a property of `<openwindow>`. The values attribute should be specified as a comma separated string of parameters, no spaces are allowed.

Dependent on external true or false a different set of parameters can be used. See for an example below.

The following parameters can be used for internal (external=false) windows:

<table>
  <tr>
    <td>height</td>
    <td>Height of the content of the window in pixels.</td>
  </tr>
  <tr>
    <td>width</td>
    <td>Width of the content of the window in pixels.</td>
  </tr>
  <tr>
    <td>left</td>
    <td>The horizontal window position in pixels from the left of the browser window.</td>
  </tr>
  <tr>
    <td>top</td>
    <td>The vertical window position in pixels from the top of the browser window.</td>
  </tr>
  <tr>
    <td>modal</td>
    <td>This will disable other opened windows (true or false).</td>
  </tr>
</table>


The following parameters can be used for external (external=true) windows:

<table>
  <tr>
    <td>height</td>
    <td>Height of the content of the window in pixels. This does not include menubar/statusbar.</td>
  </tr>
  <tr>
    <td>width</td>
    <td>Width of the content of the window in pixels.</td>
  </tr>
  <tr>
    <td>left</td>
    <td>(use screenX for Netscape 4): The horizontal window position in pixels from the left of your monitor screen.</td>
  </tr>
  <tr>
    <td>top</td>
    <td>(use screenY for Netscape 4): The vertical window position in pixels from the top of your monitor screen.</td>
  </tr>
  <tr>
    <td>menubar</td>
    <td>Specifies if the menubar (File, Edit, View, etc) is displayed, (yes|no|1|0).</td>
  </tr>
  <tr>
    <td>scrollbars</td>
    <td>Specifies if the scrollbars are displayed, (yes|no|1|0).</td>
  </tr>
  <tr>
    <td>toolbar</td>
    <td>Specifies if the button images (back, forward, reload, stop, etc) bar is displayed, (yes|no|1|0).</td>
  </tr>
  <tr>
    <td>status</td>
    <td>Specifies if the status bar is displayed, (yes|no|1|0).</td>
  </tr>
  <tr>
    <td>resizable</td>
    <td>Specifies if the window is resizable, (yes|no|1|0). Watch for spelling, many people mistankingly use "resizeable" instead, which will not work.</td>
  </tr>
</table>


The `<data-param>` tag can be used to pass data to the window being opened. The data can be complex as in referring to a component like datagrid, which returns list of Hashmap as per the selected rows, or can be simple like a static int or string value. The *name* attribute of the tag can be used in the destination window as value to the *ref* attribute in the `<set>` tag to fetch the data with the attribute *src='user' *in the* `<set>`*

```XML
<openwindow external="true" placement="center-cascade">
	<params value="width=600, resizable=false, scrollbars=0, toolbar=yes"/>
	<title value="Google" />
	<url value="http://www.google.com"/>
	<data-param name="varName" ref="componentId" src="component" />
	<data-param name="staticValue" value="someValue" />
</openwindow>
```
In the destination window the data passed using the above code snippet can be fetched as follows

```XML
<set component-id="componentId" ref="varName" src="user"/>
```
######  `<set-panel>`

The `<set-panel>` tag is used to dynamically replace an existing panel with another one defined using `<panel-definition>` tag. Usage is as follows.
```XML
<set-panel src="sourcePanel" target="targetPanel"/>
```

``sourcePanel`` is the id of the replacing panel and targetPanel is the id of the panel being replaced.

######  `<set-property>`

In order to dynamically set some properties of components the `<set-property>` tag can be used. The properties that can be dynamically set are ``currentpage, displayname, editable, enabled, width, height, selected-row, title, tooltip, visible`` and ``width``. Depending on the property, the value attribute of the `<set-property>` tag can be an integer, a boolean or a string. More than one component can be set with same property using a single `<set-property>` tag Usage is as follows.

```XML
<set-property property="editable" value="true">
	<component ref="myComponentOne"/>
  <component ref="myComponentTwo"/
  .
  .
</set-property>
```
######  `<toggle>`

This tag is almost same as `<set-property>`, but instead of specifying a particular property this tag uses the type of the component, identifies the property that can be toggled and toggles them automatically. E.g.; If toggle tag is used on a button which has been enabled then after toggle it will be disabled. Multiple components can be mentioned simultaneously to toggle. Usage is as follows.
````XML
<toggle>
  <component ref="myComponentId"/>
</toggle>
```
######  `<validate>`

This tag is used to validate the value of a referenced component against a regular expression. The message shown as a result of validation is specified using the message attribute in the `<validate>` tag.

```XML
<validate message="Validation failure message" >
  <component ref="myComponentId"/>
	<regexp>
		<![CDATA[<!-- Regular expression goes here -->]]>
	</regexp>
</validate>
```
######  `<change-style>`

This tag is used to dynamically change the style of a particular component. The id of the component is given as the reference and the `<style-action>` tag is used to set the class for the component. The class has to be defined in the css file. Usage is as follows.

```XML
<change-style>
  <component ref="myComponentId"/>
  .
  .
  <style-action action="set" style="styleClassName"/>
</change-style>
```
######  `<show-panel>`

This tag is used to have a pop-up behavior using contents defines using panel-definition tag. QAFE developers can specify x and y coordinates as attributes to the built-in and the panel will open up in that position. If x and y coordinates are not mentioned then the panel will open up at a position with respect to the component  triggering the event. It is also possible for the qafe developers to use attributes to manage the auto-hide and modal properties of the panel.

Following table lists the attributes that a show panel built-in can have.

<table>
  <tr>
    <td>src</td>
    <td>reference to the id of the panel definition which has to pop up.</td>
  </tr>
  <tr>
    <td>auto-hide</td>
    <td>if set to true the panel will disappear when clicked outside. Default value is true (optional)</td>
  </tr>
  <tr>
    <td>modal</td>
    <td>If set to true keyboard or mouse events that do not target the show panel or its children will be ignored. Default value is false (optional)
When modal=true the window or the already opened show panel from which this show-panel is called will get a different background color to show the active panel.
</td>
  </tr>
  <tr>
    <td>x</td>
    <td>x coordinate where the panel should pop-up. If not specified or specified alone show panel will appear where the mouse click happens with respect to the component triggering the event with this built-in. (optional)</td>
  </tr>
  <tr>
    <td>y</td>
    <td>y coordinate where the panel should pop-up. If not specified or specified alone show panel will appear where the mouse click happens with respect to the component triggering the event with this built-in. (optional)</td>
  </tr>
  <tr>
    <td>position</td>
    <td>This attribute can be used to specify the position where the show-panel should render the pop-up. Currently it supports only center position if specified. X and Y are attributes gets higher priority than position attribute.</td>
  </tr>
</table>


Usage of show-panel built-in is as follows
```XML
<presentation-tier>
  <view>
    <window id="windowId" displayname="Popup Panel Demo">
      <rootpanel>
        <verticallayout></verticallayout>
      </rootpanel>
    </window>

  <panel-definition id="panelDefinitionId">
    <verticallayout>
      <label displayname="Hello world."/>
    </verticallayout>
  </panel-definition>
</view>

<events>
  <event>
    <listeners>
      <listenergroup>
        <component ref="componentId"/>
        <listener type="<anEvent>"/>
      </listenergroup>
    </listeners>
    <show-panel ref="panelDefinitionId"
        x="<x-coordinate>"
        y="<y-coordinate>"
        auto-hide="<true/false>"
        modal="<true/false>"/>
  </event>
</events>

</presentation-tier>
```
######  `<close-panel>`

This tag is used to close a panel invoked using show-panel built-in. If show-panel built-in is called in an event with auto-hide as false then in order to remove it from the browser qafe developer need to use the close-panel built-in. Usage is as follows
```XML
<close-panel ref="panelDefinitionId"/>
```

###  Multilingual Support

For Internationalization (i18n) we use ``messages``. One can specify a combination of ISO Language Code and ISO Country Code (or only ISO Language Code) to define values for a specific language.

ISO Language Code: These codes are the lower-case, two-letter codes as defined by ISO-639. You can find full list of these codes in various sites, such as:  [http://www.loc.gov/standards/iso639-2/englangn.html](http://www.loc.gov/standards/iso639-2/englangn.html)

ISO Country Code: These codes are the upper-case, two-letter codes as defined by ISO-3166. You can find full list of these codes in various sites, such as: 

[http://www.iso.ch/iso/en/prods-services/iso3166ma/02iso-3166-code-lists/list-en1.html](http://www.iso.ch/iso/en/prods-services/iso3166ma/02iso-3166-code-lists/list-en1.html)

 The messages are stored in a seperate file or defined inline in the application-config.xml.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>

<messages xmlns="http://qafe.com/schema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://qafe.com/schema http://www.qafe.com/schema/application-messages.xsd">
  <bundle id="myBunlde">
        <message key="save">
            <text locale="nl_NL"><![CDATA[Opslaan]]></text>
            <text locale="en_US"><![CDATA[Save]]></text>
        </message>
    </bundle>
    <bundle id="mysecondbundle">
    </bundle>
</messages>
```
NOTE: don't use a "." in the message key attribute.

The messages element can contain several bundle elements with each of them having a set of message elements. The message element is identified by a key. The key can be used in components. The components that support the displayname property also have a ``message-key`` property. This message-key property can refer to a key in a message definition in a bundle. For a message several translations can be provided (in this example the Dutch and US English translations are shown). The locales are taken from the browser or are set by a user.

If you want to define the bundles inline in the ``application-config.xml``, then this is the way to do that:

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>

<applications xmlns="http://qafe.com/schema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://qafe.com/schema http://www.qafe.com/schema/application-context.xsd">
    <application id="app-0" name="myapp" auto-scan="true">
        <messages>
            <bundle id="mybunlde">
                <message key="save">
                    <text locale="nl_NL"><![CDATA[Opslaan]]></text>
                    <text locale="en_US"><![CDATA[Save]]></text>
                </message>
            </bundle>
            <bundle-file location="examples/bundle.xml"/>
        </messages>
    </application>
</applications>
```
Getting the value from the bundle in the component is done as follows:

* use the message-key attribute

* The format of the key is `<bundle_id>.<key>`

From the browser information regarding the language is sent to QAFE engine and this information is used for displaying the value. 

Note: Consider the scenario where the format of the locale is without country code. Since, that is not required to be sent from the browser, then the best match is searched for. eg; if the locale from the browser is 'nl' and the text element has locale attribute ``locale="nl_NL"``, then there is not an exact match, but QAFE will use this value since it starts with 'nl'.
```XML
<panel title="panelTitle" width="400" name="panelName">
    <autolayout cols="2">                 
        <button  message-key="mybundle.save" />
    </autolayout>
</panel>
```
###  Styling

UI appearance in QAFE can be controlled using style sheets. The ``class`` and  ``style`` attribute associated with the component tag can be used to serve this purpose. Users can define their own style sheets and include it in the QAML file using the `<styles></styles>`tag. The ``styles`` tag is specified in the presentation tier as follows.
```XML
<styles>
	<style location="../locationDirecory/styleSheet.css" 
         window-id="myWindowId"/>
</styles>
```

Style classes defined in a style sheet can be used in the ``class`` attribute of the components.

There are style classes which are commented out in qafe-gwt.css. Those classes can be uncommented, modified and used if we want to override the default style classes being used.

###  Business Actions

Every application has business logic. Business actions are used to specify the business logic that has to be executed. In QAFE, business actions forms the glue between the GUI components and the logic. The possible attributes for a business action are:

<table>
  <tr>
    <td>id</td>
    <td>The id of the business action. Must be unique.</td>
  </tr>
  <tr>
    <td>access (optional)</td>
    <td>This attributes is used when the business actions are published beyond the scope of your application e.g. as a webservice. Possible values are "public" and "private".</td>
  </tr>
  <tr>
    <td>description (optional)</td>
    <td>Description of the business action.</td>
  </tr>
</table>


A business action have one or more calls to services. The services are referenced using the service tag. The possible attributes for a service reference are:

<table>
  <tr>
    <td>ref</td>
    <td>The id of the service to use.</td>
  </tr>
  <tr>
    <td>method-ref</td>
    <td>The id of a method in the specified service.</td>
  </tr>
  <tr>
    <td>in (optional)</td>
    <td>The in parameter.</td>
  </tr>
  <tr>
    <td>out (optional)</td>
    <td>The out parameter.</td>
  </tr>
</table>


Basic example:
```
<business-tier>
      <business-actions>
	     <business-action id="createEmployeeAfterProcessing">  
          <transaction managed="no" />
          <service ref="myDatabaseService" 
         	        method-ref="createEmployeeAfterProcessing">
                      <in name="employeeData" />
           </service>
        </business-action>
        <business-action id="deleteEmployeeAfterCheck">
              <transaction managed="no" />
              <service ref="myJavaService"
                       method-ref="deleteEmployeeAfterCheck">
                       <in name="employeeId" />
                       <out name="deleteEmployeeOut" />
              </service>
            </business-action>
      </business-actions>
</business-tier>
```

In this example two business actions have been defined. This business actions are mapped to two services and nothing more will be executed. There are a lot of possibilities to manage business logic before and after calling a service. For this, you can use expressions and logic control functions. See the Built-Ins chapter.

Also in a business action you can have references for another business action using the ``<business-action ref=""/>`` tag.

* ``types``. It is possible to define certain types in QAML for storing specific data. These types can be reused in business actions. Possible attributes for a type are

<table>
  <tr>
    <td>id</td>
    <td>The id of the type. Must be unique.</td>
  </tr>
  <tr>
    <td>abstract (optional)</td>
    <td>If this type is abstract. The possible values are true or false.</td>
  </tr>
  <tr>
    <td>parent (optional)</td>
    <td>The parent structure of this type.</td>
  </tr>
  <tr>
    <td>attribute (optional)</td>
    <td>All the attributes that the type has.</td>
  </tr>
</table>


Basic example:
```XML
<business-tier>
  <types>
   <type id="Vehicle" abstract="true">
      <attribute name="name" default="" type="string" />
   </type>
   <type id="Donkey" parent="Vehicle" />
   <type id="Boat" parent="Vehicle">
      <attribute name="engine" type="Engine" />
    </type>
    <type id="Engine">
      <attribute name="brand" type="string" />
      <attribute name="power" type="string" />
   </type>
  </types>
</business-tier>
```
### Services

Thanks to the services we can define different possibilities to access the resources in our application. As elaborated in the 'Resources' chapter, we can use both database and java services. With the services we declare the methods and the input and output for each. The services have to be stored in the integration tier and is required to have at least one resource in the QAFE  application. 

Basic example:
```XML
<integration-tier>
		<services>
			<service id="myDatabaseService" resource-ref="databaseResource">
				<comments>This is a database service to manage the employees</comments>
				<method id="createEmployeeAfterProcessing" name="createEmployee"
					scrollable="true">
					<in name="employeeData" />
				</method>
				<method id="deleteEmployeeAfterCheck" name="deleteEmployee"
					scrollable="true">
					<in name="employeeId" />
					<out name="deleteEmployeeOut" />
				</method>
			</service>
			<service id="myJavaService" resource-ref="javaResource">
				<comments>This is a java service to manage the employees</comments>
				<method id="deleteEmployeeAfterCheck" name="deleteEmployee"
					scrollable="true">
					<in name="employeeId" />
					<out name="deleteEmployeeOut" />
				</method>
			</service>
		</services>
	</integration-tier>
```
In this example two services have been defined. One for the database resource (with two methods) and another one for the java resource (with one method). Please, notice that it is possible to have the same method name for different resources. In this way we can choose what service is going to be executed. The possible attributes for a service are:

<table>
  <tr>
    <td>id</td>
    <td>The id of the service. Must be unique.</td>
  </tr>
  <tr>
    <td>resource-ref</td>
    <td>The resource that is called when any method in the service is executed.</td>
  </tr>
  <tr>
    <td>description (optional)</td>
    <td>Description of the service.</td>
  </tr>
</table>


A service can have several methods. The possible attributes for a method are:

<table>
  <tr>
    <td>id</td>
    <td>The id of the method in the QAFE application. Must be unique per service.</td>
  </tr>
  <tr>
    <td>name (optional)</td>
    <td>The name of the method in the resource. Must be unique per service. If the name is empty the id will be used for finding the method in the resource.</td>
  </tr>
  <tr>
    <td>scrollable (optional)</td>
    <td>If you want to use the scrollable functionality. This attribute is related to the data pagination. If the method is scrollable the page settings are going to be taken into account when showing data in a component contained in the presentation tier.</td>
  </tr>
  <tr>
    <td>in</td>
    <td>The in parameter. When using spring beans as resources the name of the input parameters must be specified with indexes.</td>
  </tr>
  <tr>
    <td>out</td>
    <td>The out parameter.</td>
  </tr>
  <tr>
    <td>cache</td>
    <td>Optional attribute specifying the result of the method should be cached or not. This is meant to use with methods fetching some data from resource(java/database). 
Default value is -1 which means no caching.
0 - unlimited caching which means only once the method will be called and result will be stored to cache, after that every request will get result from cache without executing the method.
Any negative value - no caching, means each time it will go to the resource mentioned to execute the method.
Any positive value - This is time in milliseconds to store the executed result in cache.  After this time if you call this method then the cache will be updated with new data from resource. </td>
  </tr>
</table>


In the integration tier you can also use adapters and manage the errors (see Exceptions chapter). Adapters are used for manipulate the data of a component (see Adapters chapter).

Using $SQL as prefix:

In scenarios where the query or part of the query or the value for ``where`` attribute needs to be something which can be managed dynamically then QAML developer can prefix the name of input variables for methods in the service with $SQL and can use that variable name in the statement file where replacement is expected to happen. This can be useful when queries are dynamically generated according to logic or input from the end user.

Example:

In QAML File:
```XML
<service resource-ref="fireQueryResource" id="fireQueryService">
		<method id="fireQueryMethod1" name="fireQuery1">
			<in name="$SQLdynamic" ref="input1FromBA" />
			<out name="outputFromMethod1" />
		</method>
		<method id="fireQueryMethod2" name="fireQuery2">
			<in name="$SQLdeptNameVar" value="dname" />
			<in name="$SQLwhereClause" value="where deptno=" />
			<in name="$SQLdeptNumberVar" value="10"></in>
			<out name="outputFromMethod2" />
		</method>

		<method id="updateQuery2" name="updateQuery2">
			<in name="dname" value="ABC" />
			<in name="deptno" value="20" />
			<in name="$SQLdynamicPart" value="deptno=:deptno" />
		</method>
	</service>
```
In statements file:
```XML
<query id="fireQuery1"><![CDATA[$SQLdynamic]]></query>

<select id="fireQuery2" sql="select $SQLdeptNameVar from dept $SQLwhereClause :$SQLdeptNumberVar"/>

<update id="updateQuery2" table="dept" where="$SQLdynamicPart"/>
```
Important: In the above mentioned example the ``:$SQLdeptNumberVar`` is considered as a normal variable input and not as the part that needs to be replaced as there is a ‘:’ sign in front of it.

### 4.7. ResourceTier

Resources are defined as entry points of data in an application. By defining resources it is possible to access to a database or java services. It is important to understand that in this tier we define the resources and not the input/output parameters for each resource. These are going to be defined in the services in the integration tier. The kind of resources available are drivermanager, jndi and java.

#### Database

Mostly applications are designed to get data from database. It is normal to use a framework to manage the connections, data mapping, etc. In QAFE the accessing to one or more databases is easy. We only need to specify the datasource and the location of SQL statements (statements file). In this way you do not have to develop complex components to maintain connections or manage the communication between front-end and back-end. There are two options for accesing a database: using drivermanager or JNDI datasource.

#### drivermanager-datasource resource.

Use this option if you want to access to the database using drivermanager. The possible attributes are:

<table>
  <tr>
    <td>id</td>
    <td>The id of the datasource to be used in the integration tier.</td>
  </tr>
  <tr>
    <td>statements-file-url</td>
    <td>The XML file where SQL statements are stored. Multiple statements files can be specified separated by comma.</td>
  </tr>
  <tr>
    <td>dialect (optional)</td>
    <td>Possible dialects are: oracle, mysql, db2, mssqlserver and hsqldb.</td>
  </tr>
  <tr>
    <td>url</td>
    <td>Url where the datasource can be found.</td>
  </tr>
  <tr>
    <td>driver-classname</td>
    <td>Name of the driver class.</td>
  </tr>
  <tr>
    <td>username (optional)</td>
    <td>Name to use in the connection (if needed).</td>
  </tr>
  <tr>
    <td>password (optional)</td>
    <td>Password to use in the connection (if needed).</td>
  </tr>
</table>


Basic example:
```XML
<resource-tier>

      <resources>

	     <drivermanager-datasource id="MyDatasourceID" 
   	       statements-file-url="statements.xml" 
	         url="jdbc:oracle:thin:@127.0.0.1:1521:XE
	         username="apps" 
	         password="apps" 
	         driver-classname="oracle.jdbc.driver.OracleDriver" />
      </resources>
</resource-tier>
```
#### jndi-datasource resource.

Use this option if you want to access to a datasource defined using JNDI. The possible attributes are:

<table>
  <tr>
    <td>id</td>
    <td>The id of the datasource to use in the integration tier.</td>
  </tr>
  <tr>
    <td>statements-file-url</td>
    <td>The XML file where the SQL statements are stored.</td>
  </tr>
  <tr>
    <td>dialect (optional)</td>
    <td>The possible dialects are: oracle, mysql, db2, mssqlserver and hsqldb.</td>
  </tr>
  <tr>
    <td>jndiname</td>
    <td>The name of the datasource declared in the JNDI.</td>
  </tr>
</table>


Basic example:
```XML
<resource-tier>
      <resources>
            <drivermanager-datasource 
	              id="MyDatasourceID" 
	              statements-file-url="statements.xml" 
	              jndiname="myDatasourceInJNDI" />
      </resources>
</resource-tier>
```
#### Statements file.

In order to access the database, SQL statements are composed in a separate file in XML format. This has to be referenced in the resources tier. A basic example of the content of this file is as follows:
```XML
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
   <statements xmlns="http://qafe.com/schema"
	  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		 xsi:schemaLocation="http://qafe.com/schema 
			 http://www.qafe.com/schema/application-statements.xsd">
	      <select id="selectVendor">
		        SELECT * FROM VENDORS WHERE ID =  :VENDOR_ID
		</select>
	    <select id="selectVendors" table="VENDORS" />
	        <insert id="insertVendor" table="VENDORS" />
            <delete id="deleteVendor" table="VENDORS" />	
	    <call id="callProcedureId" sql="call myProcedure(?, ?)" />
	    <query id="queryId" sql="SELECT * FROM VENDORS" />
		<query id="queriesId">
		   SELECT * FROM VENDORS, PRODUCTS WHERE ID = :VENDOR_ID`
		</query>
	</statements>
```
#### Function Call Example:
	    Database:
```sql
     function get_ename(p_empno in emp.empno%type) return emp.ename%type
```

```XML
	<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
	<!-- Assume the following statements
	<statements xmlns="http://qafe.com/schema" `
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" `
		xsi:schemaLocation="http://qafe.com/schema `
	               http://www.qafe.com/schema/application-statements.xsd">
     <!-- The arguments will be retrieved from the database,     so the 
			input names defined in the integration-tier should be matched with the name 
			of the arguments;     note: in Oracle all names are uppercase -->
		<!-- <call id="getName1" call-name="get_ename" />
		<call id="getName2" sql="{? = call get_ename(?)}" /> -->
		     <!-- Assume the function "get_ename" is within a package -->
		<!-- <call id="getName1" call-name="package_name.get_ename" />
		<call id="getName2" sql="{? = call package_name.get_ename(?)}" />
   		<call  id="getName4" 
			sql="{? = call package_name.get_ename(:P_EMP_NO)}"  />
		</statements>  -->
	<resource-tier>
		<resources>
			<drivermanager-datasource id="dbResource"  
				statements-file-url="statements.xml"  
				url="jdbc:oracle:thin:@127.0.0.1:1521:XE"  username="apps"  
				password="apps"   driver-classname="oracle.jdbc.driver.OracleDriver" />
		</resources>
	</resource-tier>
	<integration-tier>
		<services>
			<service id="dbService" resource-ref="dbResource">
				<method id="getName1" name="getName1">
					   <!--  The name "P_EMPNO" should be matched with the argument defined 
						in the database   -->
					<in name="P_EMPNO" value="7389" />
					   		<!--  The name "result" defined in the ref attribute is the default 
						name for the return argument of a function -->
					<out name="result" ref="result" />
				</method>
				<method id="getName2" name="getName2">
					<in name="P_EMPNO" value="7389" />
					<out name="result" ref="result" />
				</method>
				<method id="getName3" name="getName2">
					   	<!--  The name "1" is the index for the second argument, zero-based 
						note: the return argument of a function is zero     -->
					<in name="1" value="7389" />
					<out name="result" ref="0" />
				</method>
				<method   id="getName4"   name="getName4">
					                                <!--  The name "P_EMPNO" should be matched  
						                                        with the argument defined in the 
						database     Ref "0" is the index for the first argument (zero-based) as 
						result of the function call                 -->
					<in   name="P_EMPNO"   value="7389"   />
					<out   name="result"   ref="0"   />
				</method>
			</service>
		</services>
	</integration-tier>
```

#### Procedure Call Example

Database:
```sql
    * procedure get_ename(p_empno in emp.empno%type, p_ename out emp.ename%type)
```
```XML
 
	  <?xml version="1.0" encoding="UTF-8" standalone="yes"?>

	     
	<!--  <statements xmlns="http://qafe.com/schema" 
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
		xsi:schemaLocation="http://qafe.com/schema 
		               http://www.qafe.com/schema/application-statements.xsd">
-->
		     <!-- The arguments will be retrieved from the database,          so 
			the input names defined in the integration-tier should be matched with the 
			name of the arguments;     note: in Oracle all names are uppercase -->

<!-- 		<call id="getName1" call-name="get_ename" />

		`    
		<call id="getName2" sql="{call get_ename(?,?)}" />
 -->
		     <!-- Assume the procedure "get_ename" is within a package `     <call 
			id="getName1" call-name="package_name.get_ename" /> `     <call id="getName2" 
			sql="{call package_name.get_ename(?,?)}" /> `        <call id="getName4" ` 
			*` sql="{call package_name.get_ename(:P_EMP_NO,:P_ENAME)}" /*> `    -->

		     
	<!-- </statements>-->
	<resource-tier>
		<resources>
			<drivermanager-datasource id="dbResource"  
				statements-file-url="statements.xml"  
				url="jdbc:oracle:thin:@127.0.0.1:1521:XE" ` username="apps"  
				password="apps"   driver-classname="oracle.jdbc.driver.OracleDriver" />
		</resources>
</resource-tier>
	<integration-tier>
		<services>
			<service id="dbService" resource-ref="dbResource">
				<method id="getName1" name="getName1">
					   	<!--  The name "P_EMPNO" (IN) should be matched with the argument 
						defined in the database   -->
					<in name="P_EMPNO" value="7389" />
					   	<!--  The name "P_ENAME" (OUT) defined in the ref attribute should 
						be matched with the argument defined in the database -->
					<out name="result" ref="P_ENAME" />
				</method>
				<method id="getName2" name="getName2">
					<in name="P_EMPNO" value="7389" />
					<out name="result" ref="P_ENAME" />
				</method>
				<method id="getName3" name="getName2">
					   		<!--  The name "0" is the index for the first argument, zero-based -->
						<in name="0" value="7389" />
		   				<!--  The name "1" defined in the ref attribute is the index for the 
						second argument -->
						<out name="result" ref="1" />
				</method>
				<!--  The name "P_EMPNO" (IN) should be matched    with 
					named variable :P_EMPNO and "P_ENAME (OUT) should be matched  with named 
					variable :P_ENAME                                 -->
				<method   id="getName4"   name="getName4">
					<in   name="P_EMPNO"   value="7389"   />
					<out   name="result"   ref="P_ENAME"   />           
				</method>      
			</service>
		</services>
	</integration-tier>

```

#### Calling procedures with Oracle Object type as IN/OUT:

Suppose the database contains a procedure with Object TYPE as in/out variable.

database type.
```
create or replace TYPE lne_typ AS OBJECT 
( line_id		  NUMBER(38)
, header_id		  NUMBER(38)
, project_id        NUMBER(38,0)
, project_name      VARCHAR(30)
, start_date        DATE
 );

PROCEDURE modifyObject (p_obj_lne1 in out lne_typ);
```

In statements file add this entry.
```XML
<call id="callModifyObject" sql="call QAFE_COMPLEX_DATA.modifyObject(?)"/>
```

##### Passing input as a Map containing the attributes.

For example:
```XML
   <textfield  name="LINE_ID" group-name="LNE_TYP" />
   <textfield  name="HEADER_ID" group-name="LNE_TYP" />
   <textfield id="project_id" name="PROJECT_ID" group-name="LNE_TYP"  />
   <textfield  name="PROJECT_NAME" group-name="LNE_TYP" />
   <textfield  id="start_date" name="START_DATE" group-name="LNE_TYP"  type="date"/>
```
Call a business action invoking this call statement passing the map as input:

```XML
<business-action ref="callModifyObject">
		<in name="mydata" ref="LNE_TYP" src="component" />
		<out name="result" ref="result" />
	</business-action>
	<set group-name="LNE_TYP" ref="result[0]" />
	 ..
	<business-action id="callModifyObject">
		<transaction managed="no" />
		<service ref="callWithObject" method-ref="callModifyObject">
			<in name="mydata" ref="mydata" />
			<out name="result" ref="result" />
		</service>
	</business-action>
	 ..
	<method id="callModifyObject" name="callModifyObject">
		<in name="0" ref="mydata" />
		<out name="result" ref="0" />
	</method>

	##### Passing input attributes separately.

	<business-action ref="passMultipleInputs">
		<in name="START_DATE" ref="start_date" src="component" />
		<in name="PROJECT_ID" ref="project_id" src="component" />
		<out name="result" ref="result" />
	</business-action>
	<set component-id="start_date" ref="result[0].START_DATE" />
	<set component-id="project_id" ref="result[0].PROJECT_ID" />
	 ..
	<business-action id="passMultipleInputs">
		<transaction managed="no" />
		<service ref="callWithObject" method-ref="passMultipleInputs">
			<in name="START_DATE" ref="START_DATE" />
			<in name="PROJECT_ID" ref="PROJECT_ID" />
			<out name="result" ref="result" />
		</service>
	</business-action>
	 ..
	<method id="passMultipleInputs" name="callWithObjectInput">
		<in name="START_DATE" ref="START_DATE" />
		<in name="PROJECT_ID" ref="PROJECT_ID" />
		<out name="result" ref="1" />
	</method>

```
Notes: It is not mandatory to use all the attributes in the map while calling the business action. For the attributes which are not passed value null will be set and send to the procedure.

It is not necessary to use the group-name as the Object name but the name attribute in the inputs should match with the attribute names in the Object in database.

---------------------------------------------------------------------------------------

There are two possible ways to construct queries in statements XML file. We can either write the SQL itself or we can declare the table to work with. This is because we can assign a complete data structure of key-value pairs to the statement in the integration tier. This allows direct assigning and fetching of data from and to complex components like datagrids. It implies that if an event on a data structure (like a datagrid) is assigned to a business action, the SQL associated with the service referenced in the business action is going to be executed and can manipulate more than one record at a time (eg; massive inserts). Example is as shown below

```XML
<select id="selectVendor">SELECT * FROM VENDORS WHERE ID = :VENDOR_ID</select>
```
or as an attribute:
```XML
<select id="selectVendor" sql="SELECT * FROM VENDORS WHERE ID = :VENDOR_ID”/>
```

While processing there is no differences between the two ways. By using the first option you can declare the statement in a CDATA structure. This is useful if you intend to use complex statements with several quotes in it. For example:

```XML
<![CDATA[SELECT * FROM PRODUCTS WHERE PRICE > 100]]>
```
Note: If both the options are specified for the same query, the first one will be taken into account by QAFE.

A more generic approach for construction of queries in a statements XML file is by using a query as shown below:

```XML
<query id="queriesId">

`	SELECT * FROM VENDORS, PRODUCTS WHERE ID = :VENDOR_ID`

</query>
```

The same rules which apply to the before mentioned statements, apply to the query. However, the SQL statement can now be of type INSERT, UPDATE, DELETE, etc. 

It is also possible to execute stored procedure calls. The syntax for calling a stored procedure is
```XML
<call id="callProcedureId" sql="call myProcedure(?, ?)" />
```
IN/OUT parameters should match the arguments in the the stored procedures themselves.

Note: In case of an Oracle database, OUT parameters can also be of type SYS_REFCURSOR, which will be handled like a standard (select) query.

Statement type definitions can be found on the following page: [http://www.qafe.com/static/documentation/api/application-statements.html](http://www.qafe.com/static/documentation/api/application-statements.html)

##### Usage of ``<update>`` in statement file:

 The <update> is used in a statement qaml file to perform update operation on tables in the database. <update> `can be used including following attributes 

<table>
  <tr>
    <td>id</td>
    <td>The id attribute of <update> is used as value for the name attribute of the <method> mentioned within the integration tier.</td>
  </tr>
  <tr>
    <td>table</td>
    <td>To specify the name of the table on which update should be performed.</td>
  </tr>
  <tr>
    <td>sql</td>
    <td>used to provide valid sql statements.</td>
  </tr>
  <tr>
    <td>where</td>
    <td>used to provide conditions for where clause in sql query.</td>
  </tr>
</table>


* With id and table attribute for example
```XML
<update id="myUpdate" table="TABLE_NAME"/>
```
Referencing the above mentioned example and in case of parameters being passed to an update tag, it can be of simple data types and complex data types like Map or Collection. In case of simple data types the reference variable name used must be same as that of the column name to be updated. If Map data type is used as input parameter then the key in the map should be same as that of the column name to be updated.

In case of Collection object being passed as input paramter the key of Map inside the Collection object should be same as that of the column name to be updated

* With sql query Attribute. If sql attribute is used in update tag, then the other attributes of the tag are ignored and the sql statement mentioned inside the attribute is executed.

> SQL statement provided in the attribute must be valid.

_		    eg;_
```XML
<update id="myUpdate" sql="update TABLE_NAME set <COLUMN_NAME>=:<refVarName>, <COLUMN_NAME>=:<refVarName> .... where <COLUMN_NAME>=:<refVarName>"/>
```
In case of parameters being passed as simple data types, the reference variable mentioned in ``<method>`` and the variable name used in the sql statement should be same.

If Map data type is used as input parameter then the key in the map should match with the variable name in the sql statement.

In case of Collection object being passed as input parameter the key of Map inside the Collection object should match with the reference variable name in the sql statement.

* With value being provided directly, for example:;
```XML
<update id="myUpdate" sql="update TABLE_NAME set <COLUMN_NAME>=<value>, <COLUMN_NAME>=<value>, ...." where="<COLUMN_NAME>=<value>"/>
```
* With table and where attribute mentioned, for example
```XML
<update id="myUpdate" table="QAFE_TEST_UPDATE" where="ID > :ID and ADDRESS=:ADDRESS"/>
```
When where attribute is used along with table name specified in update tag without sql attribute, the reference variable mentioned in ``<method>`` to pass parameters to <update> must match column names in the table.

##### - Usage of ``<select>`` in statement file:

 The ``<select>`` is used in a statement qaml file to perform select operation on tables in the database. ``<select>``can be used including following attributes 

<table>
  <tr>
    <td>id</td>
    <td>The id attribute of ``<select>`` is used as value for the name attribute of the <method> mentioned within the integration tier.</td>
  </tr>
  <tr>
    <td>table</td>
    <td>To specify the name of the table on which select should be performed.</td>
  </tr>
  <tr>
    <td>sql</td>
    <td>used to provide valid sql statements. If table and sql is provided sql gets the priority.</td>
  </tr>
</table>


* sql can also be specified as text between select tag like ``<select>select * from TBALE_NAME</select>`` (here after referred as sql text)

* If sql and table are specified together in ``<select>`` then only one will be considered based on priority. First will check sql, then sql as text between ``<select>`` , if they are empty will take table attribute.

* In case of parameters being passed to a select tag from integration tier, it can be of simple data types or a Map. In case of simple data types the reference variable name used must be same as that of the column name in ``TABLE_NAME``. If Map data type is used as input parameter then the keys in the map should be same as that of the column names in ``TABLE_NAME``.

* With id and table attribute, for example
```XML
 <select id="mySelect" table="TABLE_NAME"/>
```
Referencing the above mentioned example without passing any input parameter from integration tier will result in  ``query *select * from TABLE_NAME``

If parameters are passed as inputs that will be used to create the where clause. For example,   if you pass column1  as the input with value ABC then the query formed will be:
```XML

select * from TABLE_NAME where column1='ABC*'
```
* With sql query Attribute. If sql attribute is used in select tag, then the other attributes of the tag are ignored and the sql statement mentioned inside the attribute is executed, for example:
```XML
<select id="mySelect" sql="slect COLUMN_NAME from TABLE_NAME where COLUMN_NAME=:refVarName"/> `
```
- refVarName `should be passed as the input to replace the placeholder.    

##### - Usage of <delete> in statement file:

 The ``<delete>`` is used in a statement qaml file to perform select operation on tables in the database. <delete> `can be used including following attributes 

<table>
  <tr>
    <td>id</td>
    <td>The id attribute of <delete> is used as value for the name attribute of the <method> mentioned within the integration tier.</td>
  </tr>
  <tr>
    <td>table</td>
    <td>To specify the name of the table on which select should be performed.</td>
  </tr>
  <tr>
    <td>sql</td>
    <td>used to provide valid sql statements. If table and sql is provided sql gets the priority.</td>
  </tr>
  <tr>
    <td>where</td>
    <td>sql provided in where will be used with select in case only table is mentioned. When sql or sql text is given where will be ignored.</td>
  </tr>
</table>


* If sql and table are specified together in <delete> then only one will be considered based on priority. First will check sql, then sql as text between ``<select>`` , if they are empty will take table attribute.

* In case of parameters being passed to a delete tag from integration tier, it can be of simple data types or a Map. In case of simple data types the reference variable name used must be same as that of the column name in ``TABLE_NAME``. If Map data type is used as input parameter then the keys in the map should be same as that of the column names in ``TABLE_NAME``.

* With id and table attribute, for example
```XML
<delete id="myDelect" table="TABLE_NAME"/>
```
Referencing the above mentioned example without passing any input parameter from integration tier will result in ``query *delete from TABLE_NAME``

If parameters are passed as inputs that will be used to create the where clause. For example, if you pass column1  as the input with value ABC then the query formed will be

* ``delete from TABLE_NAME where column1='ABC*'``

* With sql query Attribute. If sql attribute is used in delete tag, then the other attributes of the tag are ignored and the sql statement mentioned inside the attribute is executed. For example, 
```XML
<delete id="myDelect"` `sql="delete from TABLE_NAME where COLUMN_NAME=:refVarName"/> `
```
`- refVarName `should be passed as the input to replace the placeholder.

* VPD. In QAFE is possible to access to the database via VPD (Virtual Private Database).

### Java

You can reuse your java classes in QAFE or add new ones. To access java classes in QAML, they must be packaged in one (or more) jar file(s) ) and must be compiled with the same JDK version used for the QAFE runtime environment.

#### javaclass resource.

 This tag is used to specify java data source as resource. The possible attributes are:

<table>
  <tr>
    <td>id</td>
    <td>The id of the datasource to be used in the integration tier.</td>
  </tr>
  <tr>
    <td>classname</td>
    <td>The name of the class to be used.</td>
  </tr>
  <tr>
    <td>jarfile-location (optional)</td>
    <td>The location of the jar file. The jar file can be included in the classpath environment variable or a valid URL can be declared. If this attribute can be empty is the the class being used is already in the classpath.</td>
  </tr>
</table>


Example:
```XML
<javaclass id="MyJavaResource" classname="MyJavaClass" />
```
#### spring resource.

This tag is used to specify the Spring context as resource. The possible attribute is:

<table>
  <tr>
    <td>use-web-config</td>
    <td>Default value false.
When set to true Spring Context loaded in web.xml will be used to get the spring beans.
In this case config-files attribute values will be ignored.</td>
  </tr>
  <tr>
    <td>config-files</td>
    <td>The name of the Spring configuration xml files, separated by comma. These files have to be in the WEB-INF directory.</td>
  </tr>
</table>


* _spring-bean_ element.

This tag is used to specify the bean in the Spring context. The possible attributes are:

<table>
  <tr>
    <td>id</td>
    <td>The id of the bean to be used in the integration tier.</td>
  </tr>
  <tr>
    <td>bean</td>
    <td>The name of the bean defined in the Spring context.</td>
  </tr>
</table>


Example:
```XML
<spring config-files="spring-context1.xml, spring-context2.xml">
  <spring-bean id="myBean" bean="beanName">
  <spring-bean id="myBean2" bean="beanName2">
</spring>
<spring use-web-config="true”>
	<spring-bean id="myBean" bean="beanName">
	<spring-bean id="myBean2" bean="beanName2">
</spring>
```

### 4.8. Adapters

The concept of adapters have been implemented in QAFE to have more control over structural input and output within QAFE framework. It is possible to use an adapter for placing an integration service result in one ore more business domain objects. If the service result is a list, the list will be recursively adapted. The adapters are declared in the integration tier.

The possible attributes for an adapter are:

<table>
  <tr>
    <td>id</td>
    <td>The id of the adapter. Must be unique.</td>
  </tr>
  <tr>
    <td>type</td>
    <td>The type of the adapter.</td>
  </tr>
  <tr>
    <td>adapt-all (optional)</td>
    <td>Adapt-all equal to true indicates that all fields which are specified in the class or adapt-name will be adapted.</td>
  </tr>
  <tr>
    <td>adapt-name (optional)</td>
    <td>The object name, defined in the (Oracle) database, used for the adapter</td>
  </tr>
  <tr>
    <td>class (optional)</td>
    <td>The java class used for the adapter. Must be fully qualified class name.</td>
  </tr>
  <tr>
    <td>extends (optional)</td>
    <td>Used to specify that adapter extends another one.</td>
  </tr>
</table>


Basic example:
```XML
<integration-tier>
      <services>
   	     <service resource-ref="vehicle" id="vehicleResource">
   	           <method id="fetchVehicles" name="fetchVehicles">
   	                 <out name="vehicle_out" adapter="car"/>
   	           </method>
   	     </service>
      </services>
      <adapters>
      <adapter id="car" type="CarAdapter" class="com.qualogy.qafe.vehicle.car">
                  <attribute name="carName" ref="name"/>
                  <attribute name="carType" ref="type"/>
                  <attribute name="passengersCount" ref="capacity"/>
         </adapter>
      </adapters>
</integration-tier>
```
The above code snippet adapts the output object of fetchVehicles method. The key of output  mentioned using ref attribute in the adapter attribute tag will be adapted to the text mentioned as value for name in the attribute tag.

An adapter can consist of attributes or nested adapters. The possible attributes for an attribute tag are:

<table>
  <tr>
    <td>name (optional)</td>
    <td>The name of the attribute.</td>
  </tr>
  <tr>
    <td>ref (optional)</td>
    <td>Reference of the attribute.</td>
  </tr>
  <tr>
    <td>adapter (optional)</td>
    <td>The adapter of the attribute.</td>
  </tr>
  <tr>
    <td>default (optional)</td>
    <td>Use this to specify default value</td>
  </tr>
  <tr>
    <td>type (optional)</td>
    <td>Used to refer the type of the attribute. Can specify another class as type.</td>
  </tr>
</table>


Possible attributes for a nested adapter are:

<table>
  <tr>
    <td>id (optional)</td>
    <td>The id of the adapter. Must be unique.</td>
  </tr>
  <tr>
    <td>type</td>
    <td>The type of the adapter.</td>
  </tr>
  <tr>
    <td>adapt-all (optional)</td>
    <td>Adapt-all true indicates that also fields that are not specified are adapted.</td>
  </tr>
  <tr>
    <td>class (optional)</td>
    <td>The java class used for the adapter. It must include the complete package.</td>
  </tr>
  <tr>
    <td>extends (optional)</td>
    <td>If this adapter extends another one.</td>
  </tr>
  <tr>
    <td>attribute</td>
    <td>The attribute of the nested adapter.</td>
  </tr>
</table>

```XML
<adapters>
	<adapter type="" id="" adapt-all="false" class="" extends="">
		<attribute name="" ref="" adapter="" default="" type=""/>
		<adapter type="" attribute="" adapt-all="false" class="" extends="" id=""/>
	</adapter>
</adapters>
```

The adapter can also be used if you want to pass a list of objects to stored-procedures, see the following example:

- Define the EMP_OBJECT object in the database:
```
`CREATE OR REPLACE TYPE EMP_OBJECT AS OBJECT (
    "EMPNO" NUMBER(4,0),
    "ENAME" VARCHAR2(10 BYTE),
    "JOB"   VARCHAR2(9 BYTE),
    "MGR"   NUMBER(4,0),
    "HIREDATE" DATE,
    "SAL"    NUMBER(7,2),
    "COMM"   NUMBER(7,2),
    "DEPTNO" NUMBER(2,0),
    CONSTRUCTOR FUNCTION  EMP_OBJECT  RETURN SELF AS RESULT
);
```
- Define the EMP_ARRAY collection of the EMP_OBJECT object in the database:

``CREATE OR REPLACE TYPE EMP_ARRAY AS TABLE OF EMP_OBJECT;``

- Define the SUMSALARYOFEMPLOYEES* *stored-procedure to use the collection and/or object as in and/or out parameters:

```
CREATE OR REPLACE PROCEDURE SUMSALARYOFEMPLOYEES(
	EMPLOYEES IN OUT EMP_ARRAY, EMPLOYEE IN OUT EMP_OBJECT, TOTALSALARY OUT NUMBER) IS
BEGIN
  TOTALSALARY := 0;
  FOR i IN 1..EMPLOYEES.COUNT LOOP
    TOTALSALARY := TOTALSALARY + NVL(EMPLOYEES(i).SAL, 0);    
  END LOOP;
END;
```
- Implement the QAML code:

```XML
<integration-tier>
      <services>
         <service resource-ref="myResource" id="myService">
   			<method id="sumSalaryOfEmployees" name="sumSalaryOfEmployees">
			    <in name="EMPLOYEES" adapter="arrayOfEmp"/>
		        <in name="EMPLOYEE"/>
				<out name="EMPLOYEES" ref="EMPLOYEES adapter="arrayOfCompactEmp"/>
				<out name="EMPLOYEE" ref="EMPLOYEE"/>
				<out name="TOTALSALARY" ref="TOTALSALARY"/>	
			</method>
		 </service>
	</services>

    <adapters>
       <adapter id="arrayOfEmp" adapt-name="EMP_ARRAY" adapt-all="true"/>
       <adapter id="arrayOfCompactEmp" adapt-name="EMP_ARRAY" adapt-all="false">
	     <attribute name="EMPNO" ref="EMPNO"/>
         <attribute name="ENAME" ref="ENAME"/>
       </adapter>
      </adapters>
</integration-tier>
```

```XML
<call id="sumSalaryOfEmployees" call-name="sumSalaryOfEmployees" `
```

``sql="{call sumSalaryOfEmployees(?,?,?)}"/>``

Note:

There is no need to define an adapter for the "EMP_OBJECT" object, because the name of the object can be retrieved by the metadata. So it’s done implicitly.

### Transactions

As per the need, the transaction behavior can be set in the following order:

* In order to apply transaction behavior to all the applications (globally) a classpath property called 'global.transaction.behavior' can be declared. Add the following line to the application-config.xml file enables this.

``	<configuration name="global.transaction.behaviour" value="global.transaction.behaviour"/>``

* In order to apply transaction behavior to one application a configuration property called 'global.transaction.behaviour' in the application context file (this property is equal to the global property, but will be only applicable within the context of an application)

* In order to apply transactional behaviour to one business action, management properties on each individual business action have to be declared. An extra tag in the body of the business-action specifies what kind of transaction is needed:

```XML
<business-action id="localTransaction">
      <transaction managed="local" />
      ...
</business-action>
```
    * Applying to one business action again but using wild-cards in the context file. In the below mentioned example the specified transaction behavior applies to all business actions. Patterns to match certain business actions can be specified following this method.

```XML
<application>
	<transaction>
        	<business-action id="" managed="local"  isolation="default"
 	                  propagation="required" timeout="0" />
		</transaction>
</application>	
```

The managed attribute can have the following values:

<table>
  <tr>
    <td>global</td>
    <td>For using global transaction.</td>
  </tr>
  <tr>
    <td>local</td>
    <td>For using local transaction.</td>
  </tr>
  <tr>
    <td>none</td>
    <td>For using no transaction at all.</td>
  </tr>
</table>


The isolation attribute can have the following values:

<table>
  <tr>
    <td>default</td>
    <td>To specify the default isolation level of the underlying datastore. All other levels correspond to the JDBC isolation levels</td>
  </tr>
  <tr>
    <td>read_uncommited</td>
    <td>A constant indicating that dirty reads, non-repeatable reads and phantom reads can occur. Depending on this attribute's value, a row changed by one transaction is allowed to be read by another transaction before any changes in that row have been committed (a "dirty read"). If any of the changes are rolled back, the second transaction will have retrieved an invalid row.</td>
  </tr>
  <tr>
    <td>read_commited</td>
    <td>A constant indicating that dirty reads are prevented. Non-repeatable reads and phantom reads can occur. This attribute only prohibits a transaction from reading a row with uncommitted changes in it.</td>
  </tr>
  <tr>
    <td>repeatable_read</td>
    <td>A constant indicating that dirty reads and non-repeatable reads are prevented; phantom reads can occur. This attribute prohibits a transaction from reading a row with uncommitted changes in it and it also prohibits reads from a row after a second transaction alters the row (so you will get different values the second time, which is a "non-repeatable read").</td>
  </tr>
  <tr>
    <td>serializable</td>
    <td>A constant indicating that dirty reads, non-repeatable reads and phantom reads are prevented. This attribute includes the prohibitions in "repeatable_read" and further prohibits transaction reads from rows that satisfy a WHERE condition after a second transaction inserts a row that satisfies the same WHERE condition (retrieving an additional "phantom" row in the second read).</td>
  </tr>
</table>


The propagation attribute can have the following values (*propagation options similar to EJB CMT*):

<table>
  <tr>
    <td>required (i)</td>
    <td>Supports the current transaction and creates a new one if none exists. 

This is commonly used as a default setting of the transaction definition and defines a transaction synchronization scope.</td>
  </tr>
  <tr>
    <td>supported (i)</td>
    <td>Support a current transaction; execute non-transactionally if none exists. 

For transaction managers with transaction synchronization, PROPAGATION_SUPPORTS is slightly different from no transaction at all, as it defines a transaction scope where synchronization might be applied. As a consequence, the same resources (a JDBC Connection, a Hibernate Session, etc) will be shared for the entire scope specified. Note that the exact behavior depends on the actual synchronization configuration of the transaction manager ! 

In general, use this option with care! In particular, do not rely on propagation="required" or propagation="requires_new" within a propagation="supported" scope (which may lead to synchronization conflicts at runtime). If such nesting is unavoidable, make sure to configure your transaction manager appropriately (typically switching to "synchronization on actual transaction").</td>
  </tr>
  <tr>
    <td>mandatory (i)</td>
    <td>Support a current transaction; throw an exception if no current transaction exists. 

Note that transaction synchronization within a propagation="mandatory" scope will always be driven by the surrounding transaction.</td>
  </tr>
  <tr>
    <td>not_supported (i) (ii)</td>
    <td>Do not support a current transaction. Execution always happens non-transactionally.

Note that transaction synchronization is not available within a propagation="not_supported" scope. Existing synchronizations will be suspended and resumed appropriately.</td>
  </tr>
  <tr>
    <td>never (i)</td>
    <td>Do not support a current transaction. Throw an exception if a current transaction exists. 

Note that transaction synchronization is not available within a propagation="never" scope.</td>
  </tr>
  <tr>
    <td>nested (iii)</td>
    <td>Execute within a nested transaction if a current transaction exists, else behaves like propagation="required".</td>
  </tr>
</table>


(i) Analogous to the Enterprise Java Bean transaction attribute of the same name.

(ii) Actually transaction suspension will not work out-of-the-box on all transaction managers. The JTA Transaction Manager of the Spring Framework for example requires the javax.transaction.TransactionManager to be made available to the transaction (which is server-specific in standard J2EE).

(iii) Actually creation of a nested transaction will only work on specific transaction managers. Out of the box, it only works on the JDBC DataSourceTransacationManager of the Spring Framework when working with a JDBC 3.0 driver. Some JTA providers might also support nested transactions.

### Exceptions

Unfortunately, every software application has to deal with exceptions, errors, bugs, warnings, etc. Any exception that is thrown from a service method call is already captured and shown to the user on the front-end. This is of course not always desirable. For that reason QAFE includes a special mechanism for handling exceptions.

First of all, assume the following method in a Java class:
```java

package com.qualogy.qafe.gwt.standalone;
   public class MyClass {
		public int nextInt(){
			throw new IllegalArgumentException("MyClass illegal....");
		}
	    public float nextFloat(){`
			return 0;
		}
}
```

As one can see, the nextInt method throws an IllegalArgumentException. When this method is invoked from QAFE, the exception will be shown on the screen in a default error box.

If you want your own exception handling, then there are a couple of steps to take in order to make this work:

First of all, in the integration-tier, there is a possibility to define errors that can occur because a service method was invoked. The way to do this is as follows:

Basic example:
```XML
<integration-tier>

.........

	<errors>
	<error id="nameAlreadyRegistered" exception="java.lang.Exception">
	<log>The name was already registered</log>
	</error>
	<error id="illegalArgumentException" 
		   exception="java.lang.IllegalArgumentException">
	<log>Message from QAFE illegalargument thrown</log>
	</error>
    </errors>
</integration-tier>
```
As we can see here, there is a section "errors" in the integration-tier. This contains several error definitions. Each error has an id, exception and log entry (as child element). The log message is for internal use only: When an error is thrown and the error type is found as declared in the exception attribute, QAFE then adds all the information of the declared log attributes (if any) in the log file.

The attributes for an error are:

<table>
  <tr>
    <td>id</td>
    <td>ID of the error, it must be unique.</td>
  </tr>
  <tr>
    <td>exception</td>
    <td>Contains the fully qualified Java class name or error code in case of a database. Like an Oracle database it can be ORA-XXXXX (X is a digit)</td>
  </tr>
</table>


So now QAFE knows about the java.lang.IllegalArgumentException. 

The next step is to define the part where the error-handling will result in actions.

In the body of an *event *and *business-action* it's possible to define the <error-handler> tag.

Here's an example:
```XML
<events>
	<event>
		<listeners>
			<listenergroup>
				<component ref="mylabel" />
				<listener type="onclick" />
			</listenergroup>	
		</listeners>
		<business-action ref="nextInt" />
	   <error-handler id="illegalArgument" error-ref="illegalArgumentException"
	                                 final-action="swallow">
                <dialog type="error">
        	        <title value="My Error Dialog"/>
            	    <message ref="$ERROR_MESSAGE"/>
                 </dialog>
	    </error-handler>
	</event>
</events>
```

This is an event that listens to an "onclick" event and executes a business-action. Assume that the other tiers are also implemented to execute the nextInt method. This business-action will eventually invoke the method that we defined in the Java class above.

The interesting part here is the error-handler. The error-handler has an id (any value) and an error-ref. This error-ref refers to one of the ``errors`` defined in the* integration-tier*.

The final action is actually quite interesting. The possible values are ``swallow`` or ``rethrow``. When swallow is selected, the exception that is thrown (defined by the ``error-ref``) will be absorbed by QAFE and the body of the error-handler will be added to the list of built-ins that are to be executed on the client.  In case of rethrow, the body of the error-handler will not be processed, and the error will be shown in the default error dialog if no error-handlers can handle the error in the parent event, if present.

Of course the body of an error-handler can call business-actions etc. Depending on which type (``event`` or ``business-action``) it is, so the possibilities are similar to the body of the type (``event`` or ``business-action``). In this case, all the possible built-ins belonging to an event can be used here.

In this example, an exception is thrown by the business-action call. Since the exception can be found in the error definition, the ``error-handler`` with ``id="illegalArgument`` will be executed. This exception is ``swallowed`` and the body of this error-handler (dialog built-in) will be added to the methods that needs to be executed on the client. 

In the datastore the value of the error message is stored, so using the ``$ERROR_MESSAGE`` key you will get the error message back.

The end result is that you will see a dialog with "My Error Dialog" as the title and ``$ERROR_MESSAGE`` as the message. 

The attributes for an error-handler are:

<table>
  <tr>
    <td>id</td>
    <td>ID of the error, it must be unique.</td>
  </tr>
  <tr>
    <td>error-ref</td>
    <td>References to an Error</td>
  </tr>
  <tr>
    <td>final-action</td>
    <td>Can be swallow or rethrow</td>
  </tr>
</table>


To summarize:

* Exception handling can be done in an event or a business-action, if no error-handlers are  found for the error in the same event or business-action, then it will rethrow to the parent event or business-action, if present, otherwise it is handled by the default exception handling

* An event or a business-action which is invoking another event or business-action can handle the error of the calling event or business-action, the other way around is not possible

* Only one error-handler can be processed within an event or business-action

* The body (built-ins) of an error-handler will not be processed if it is a rethrow

* When an error-handler is found for the error, it will process the built-ins defined in the body of the error-handler, afterward it will continue the flow after the error-handler

* When an error-handler is found for the error, the built-ins between the error-handler and the built-in which caused the error will not be processed
