#QAFE Book[TEST DOCUMENT]

![qafelogo](http://www.qafe.com/wp-content/themes/qafe2013/img/logo.png) 

## 1. Introduction: QAFE Core Platform

### 1.1 What is QAFE?
QAFE is a product from [Qualogy Consultancy B.V](http://www.qualogy.com) and is an acronym for "Quality Application Framework for Enterprises". 

![qualogylogo](http://www.qualogy.com/wp-content/themes/qua/images/q_logo.png)

QAFE is the result of years of experience, best practices and theory. They all come together in this product. This product is meant for large enterprises and foresees in future developments.

### 1.2 Why QAFE ?
QAFE was born as a result of the fact that application development became harder while technologies evolved. 
One major technology which became an industry standard is Oracle Forms. Oracle Forms is implementation of the Client-Server Paradigm, where data entry from desktop clients to a centralized database is the core application. 
Since Oracle Forms was a solid solution, worldwide companies implemented their core process in Oracle Forms. Next to being a solid solution, building applications in Oracle Forms technology was fast. Productivity of Oracle Forms Developers was high. 

Technology evolved and just around the year 2000 Oracle announced to go the Web and Java. Internet was gaining momentum, even Enteprises started to believe in internet technology. The stack that Oracle was foreseeing for the next generation applications was Java based, which is different than the base for Oracle Forms (which is PL/SQL). 
In the same time, somehow to create a simple application was too hard. It took days, and the progress that was made was just not that of what was expected. Furthermore, for some developers the change of mindset from relational/data oriented thinking to object oriented was needed. This was a hard one. Some Oracle Forms Developers made it, but most of them didn't. Still the relational/data oriented developer (currently called "Oracle Classic Developers") had a strong argument against the new object oriented way of development, the productivity being higher than the developer using object orientation. 
Furthermore, besides the fact that there was a change of mindset, several other influences were found, that made software development in current days just too cumbersome. On the presentation technologies there were a lot of developments happening at the same time. 


| Scenario      | CPU  | Memory  |Harddisk |
|---------------|------|---------|---------|
| QAFE Tutorial | Any modern CPU| 2 Gb |1,7 Gb |
| Developing in QAML| Any modern CPU | 1,5 Gb |50 Mb Per render engine (GWT, etc.) this amount of diskspace is used. |
| Running QAFE apps | Depending on the chosen server | 2 Gb | 50 Mb Per render engine (GWT, etc.) this amount of diskspace is used |


```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<applications  xmlns={_}{+}http://qafe.com/schema+_
xmlns:xsi={_}{+}http://www.w3.org/2001/XMLSchema-instance+_ xsi:schemaLocation="http://qafe.com/schema
http://www.qafe.com/schema/application-context.xsd">
             
  <application name="apps" id="system_app" >
    <application-mapping-file location="qafe-default-system-app.xml"/>                       
  </application>
             
 
  <application name="Hello World" id="<b>myApp</b>" >
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


List:

- $OFFSET
- $COUNTPAGESAVAILABLE
- $AMOUNT_PAGESAVAILABLE
- $PAGESIZE
- $SORT_COLUMN
- $SORT_ORDER

```java
package com.qualogy.qafe.gwt.standalone; 
    public class MyClass { 
public int nextInt(){
throw new IllegalArgumentException("MyClass illegal....");
}
    public float nextFloat(){
return 0;
}
} 
```
