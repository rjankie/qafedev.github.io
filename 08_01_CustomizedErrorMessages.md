#QAFE Book

![qafelogo](http://www.qafe.com/wp-content/themes/qafe2013/img/logo.png)

### Customized Error Messages
When contacting a data source, it is possible for something to go wrong. For example a null check is not done properly or a database constraint is violated. It is possible to fetch these exceptions and properly handle the error to the user. Underneath is an example on how to handle this.

In the presentation tier underneath, an onclick-event is available which saves some data to the database. During the database handlings, something can go wrong. a simple error-handler is defined underneath.  The error-ref refers to an error-handler that is defined in the integration-tier. The id is specified for reuse-purposes. The final-action determines how to handle the error. One way is to re-throw the exception to the screen. Another option is to swallow the exception and do other actions. Inside the error-handler, a dialog message is shown to the user in this case. Developers are free to write their own QAML-code within the error-handler.
```XML
<presentation-tier>
	<events>
		<event>
			<listeners>
				<listenergroup>
					<component ref="saveButton" />
					<listener type="onclick" />
				</listenergroup>
			</listeners>
			<business-action ref="saveData">
				<in name="allRows" ref="allRows" />
				<out name="P_ID" ref="P_ID" />
			</business-action>
			<store name="currentId" ref="P_ID" />

			<error-handler error-ref="generalException" id="generalExceptionId" final-action="swallow">
				<dialog type="error">
					<title value="Exception Occured" />
					<message value="An exception has occured during saving. Please try again." />
				</dialog>
			</error-handler>
		</event>
	</events>
</presentation-tier>
```

The different errors can be defined in the integration tier using the error-tag. The id is the identifier of the error in the application, while the exception shows which exception is fetched. In this case, the general Java Exception is used.

```XML
<business-tier>
	<business-actions>
		<business-action id="saveData">
			<transaction managed="no" />
			<service ref="SQLResourceService" method-ref="saveData">
				<in name="allRows" ref="allRows" />
				<out name="P_ID" ref="P_ID" />
			</service>
		</business-action>
	</business-actions>
</business-tier>

<integration-tier>
	<services>
		<service resource-ref="SQLResourceService" id="SQLResourceService">
			<method id="saveContract" name="saveContract">
				<in name="allRows" ref="allRows" />
				<out name="P_ID" ref="P_ID" />
			</method>
		</service>
	</services>

	<errors>
		<error id="generalException" exception="Exception" />
	</errors>
</integration-tier>
```

It is possible to specify the exception in further detail. Oracle exceptions can be identified and caught like described in the first example. The second example refers to a specific Java exception, in this case the NullPointerException. Handling a specific exception is useful, when multiple specific scenarios can occur. The database-procedure could have been handled fine, but the actions in Java after that could lead to an exception. For both cases, other specific messages can be shown to the user.
```XML
<errors>
  <error id="constraintViolatedException" exception="ORA-02290"/>
  <error id="nullPointerException" exception="NullPointerException"/>
</errors>
```
