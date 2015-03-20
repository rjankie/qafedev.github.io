#QAFE Book

![qafelogo](http://www.qafe.com/wp-content/themes/qafe2013/img/logo.png)

### Utilizing the full browser screen for each panel

When creating your QAFE application, there are several approaches if it comes to panels when your application is set to SDI (Single Document Interface) mode.
One option is to have one main landing page and having every other panel-definition open in a pop-up screen using the `show-panel` built-in.
However, when using the `show-panel` method and your panel-definition content exceeds the screen size, the content is cut-off from the browser. As no scrollbars are made available by default.
You can opt to change this by applying some CSS `overflow: scroll;` to the pop-up screen container.

Another option is to use the main container to render your panels in and thus utilizing the entire browser window for your panel-definition. For this, you can use the set-panel built-in instead of the show-panel built-in.
The way the set-panel built-in works is by having a `panel-ref` element available within the rootpanel of your main window (e.g. landing page).
The `panel-ref` element acts as a virtual panel utilizing the space provided by the parent container. On which you can render the content of one panel-definition at any given time.

Observe the following example:
```
<window id="helloWorld" displayname="Hello World" width="300" height="250">
	<rootpanel id="rootPanelId">
		<verticallayout>
			<button id="showPanel" displayname="Show Panel" />
			<button id="setPanel" displayname="Set Panel" />
			<panel-ref ref="" id="sourcePanelId" />
		</verticallayout>
	</rootpanel>
	<events>
		<event>
			<listeners>
				<listenergroup>
					<component ref="showPanel" />
					<listener type="onclick" />
				</listenergroup>
			</listeners>
			<show-panel src="testPanel" auto-hide="false" modal="true" position="center"/>
		</event>
		<event>
			<listeners>
				<listenergroup>
					<component ref="setPanel" />
					<listener type="onclick" />
				</listenergroup>
			</listeners>
			<set-panel src="testPanel" target="sourcePanelId" />
		</event>
	</events>
</window>

<panel-definition id="testPanel">
	<verticallayout>
		<textfield/>
		<button/>
	</verticallayout>
</panel-definition>
```
At the bottom we have a `panel-definition` element which we are be able to show as a pop-up and also set to a `panel-ref` element using the defined button onclick event handlers.
The `ref` attribute of the `panel-ref` component can be used to specify a default panel-definition to render within the virtual panel, as explained above. In this example we chose to leave this empty.