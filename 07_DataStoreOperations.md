#QAFE Book

![qafelogo](http://www.qafe.com/wp-content/themes/qafe2013/img/logo.png)

## 7. Data store operations


<!-- toc -->

* [7. Data store operations](#7-data-store-operations)
  * [7.1 How to store and access data in QAFE applications](#71-how-to-store-and-access-data-in-qafe-applications)
  * [7.2 Pipe storage](#72-pipe-storage)
  * [7.3 User storage](#73-user-storage)
  * [7.4 Global storage](#74-global-storage)

<!-- toc stop -->


### 7.1 How to store and access data in QAFE applications

In order to be able to use and manipulate data in a QAFE application, it needs to be stored somewhere. There are different ways of storing data, these will be dealt with in this chapter.

The table below shows the different *store types*. It indicates the scope of the store. For example; the *window* scope means that as long as the window is active, the variable stored in this store type is also available. The column *built-in* indicates which event built-in you have to use in order to assign a value to a variable stored in this store type.

Store type | Scope | Built-in
---|---|---
pipe | event | store
user | window | store
global | application | store
component | window | set

 In order to store a value in QAFE you will have to provide:
 - a name for the variable
 - the store type you want to use (pipe, user, global, component)
 - a value for the variable

In case of a store built-in, the QAML code could look like this:

```XML
<store name="myVar" target="pipe" value="HelloWorld!"></store>
```

In the example above the name of the variable is myVar, it is stored in the pipe and the value is the string HelloWorld!.

### 7.2 Pipe storage

The following code provides an example of an event in which a variable is stored in the pipe and retrieved from it.
```XML
<event id="storeEventPipe">
    <listeners>
        <listenergroup>
            <component ref="storeButtonPipe"/>
            <listener type="onclick"></listener>
        </listenergroup>
    </listeners>
    <store name="myVar" target="pipe" ref="storeTextFieldPipe" src="component"></store>
    <set component-id="pipeStoreResult" ref="myVar" src="pipe"></set>
</event>
```
Since the scope of the pipe is the event, the variable "myVar" is cleared as soon as the event ends. In the case above the value of "myVar" is still available in the UI component "pipeStoreResult".

Note: instead of assigning a value to "myVar" directly, we have given it the value of a UI component. In this case the component is a text field, but it could be many other components types as well.

Also note that in the example above we use the set built-in. In this built in a value is stored in a (UI) component. Since the pipe store is the default source, the tag 'src="pipe"' could have been left out of the QAML code.

### 7.3 User storage

The scope of the user storage is the window. This store procedure can thus be used in different events.

```XML
<event id="storeEventUser">
    <listeners>
        <listenergroup>
            <component ref="storeButtonUser"/>
            <listener type="onclick"></listener>
        </listenergroup>
    </listeners>
    <store src="component" ref="storeTextFieldUser" target="user" name="myVar"></store>
</event>
<event id="showStoreEvent">
    <listeners>
        <listenergroup>
            <component ref="getStoresButton"/>
            <listener type="onclick"></listener>
        </listenergroup>
    </listeners>
    <set component-id="userStoreResult" src="user" action="set" ref="myVar"></set>
</event>
```
In the above example the variable "myVar" is assigned the value of the "storeTextFieldUser" textfield when the user clicks the button "storeButtonUser".
When the user clicks the button "getStoresButton", the label "userStoreResult" is assigned the value of "myVar" form the user store.

### 7.4 Global storage

The QAML code example below shows a value that is stored in the global store. This variable can be used in another window.

```XML
<presentation-tier>
  <view>
    <window id="globalStoreWindow1" displayname="Global Store Demo" width="495" height="119">
      <rootpanel>
        <horizontallayout>
          <button displayname="Store Global" id="storeButtonGlobal" />
          <textfield id="storeTextFieldGlobal"></textfield>
          <button displayname="New Window" id="showGlobalStoreWindowButton"/>
        </horizontallayout>
      </rootpanel>
    </window>
    <window id="globalStoreWindow2" displayname="Global Value" width="469" height="115">
      <rootpanel>
        <horizontallayout>
          <label displayname="Global store value"/>
          <label id="globalStoreResult"/>
          <button id="showGlobalStoreButton" displayname="Update Global Store"/>
        </horizontallayout>
      </rootpanel>
    </window>
  </view>
  <events>
    <event id="storeGlobalEvent">
      <listeners>
        <listenergroup>
          <component ref="storeButtonGlobal"/>
          <listener type="onclick"></listener>
        </listenergroup>
      </listeners>
      <store src="component" ref="storeTextFieldGlobal" target="global" name="myVar"></store>
      </event>
    <event id="showNewWindowEvent">
      <listeners>
        <listenergroup>
          <component ref="showGlobalStoreWindowButton"/>
          <listener type="onclick"></listener>
        </listenergroup>
      </listeners>
      <openwindow>
        <ref value="globalStoreWindow2"></ref>
      </openwindow>
    </event>
    <event id="showGlobalStoreValueEvent">
      <listeners>
        <listenergroup>
          <component ref="showGlobalStoreButton"/>
          <listener type="onclick"></listener>
        </listenergroup>
      </listeners>
      <set component-id="globalStoreResult" src="global" ref="myVar"></set>
    </event>
  </events>
</presentation-tier>
```
In this example the data in the textfield "storeTextFieldGlobal" is stored in the global store when the button "storeButtonGlobal" is clicked.
The button "showGlobalStoreWindowButton" creates a new window when clicked. In this new window when the button "showGlobalStoreButton" is clicked, the data that was stored before is shown in the label "globalStoreResult".
