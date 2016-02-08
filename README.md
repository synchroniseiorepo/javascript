[Synchronise.IO](https://www.synchronise.io)
--------------

The Javascript client allows you to communicate with Synchronise on the web and SmartWatches.
The library communicates with Synchronise through web sockets. We have implemented socket connection using the open source library Socket.IO. It provides cross-platform compatibility.

----------


### Setup

Integrate Synchronise in your project by copy-pasting this code on your web page.
```
<script src="https://js.synchronise.io/1.0.min.js"></script>
```

----------


### Init
To get started with the javascript library you first need to initialise it. This is achieved by calling the following line. We have pre-filled it with your own Public Key so you can just copy paste it in your project.

```
Synchronise.init("[YOUR PUBLIC KEY]");
```    

You can find your Public Key on the export section of a Component.

![Find your public key](https://images.synchronise.io/public_key.png)

----------

### Connection / Disconnection
Your users could be on unstable networks. The library allows you to get alerts when the connection to Synchronise is lost. The library will always try to reconnect to Synchronise on the background. We have implemented mechanisms to queue and retry requests when the connection is re-established.

The library raises 2 type of events Connected and Lost. Both events can propagate to any function of your app using a callback mechanism. The events can trigger many times during the life cycle of your app. You should handle the Connection events in consequence.

```
Synchronise.Connection.Connected(function(){
    // Code to execute every time the connection is established (or re-established)
});
```     

```
Synchronise.Connection.Lost(function(){
    // Code to execute every time the connection is established (or re-established)
});
```    

### Component
The component class allows you to communicate with the Component service of Synchronise.

#### Run
The run method allows you to execute a component on our Cloud.

**Parameters:**

 - (String)id_component: The first parameter is the ID of the component.
   It is provided to you on the interface on www.synchronise.io
 - (JSON)parameters: A JSON Key-value for all of the parameters you want
   to send to the component. If there is no parameter to send simply   
   provide an empty JSON {}
 - (Object)callbacks: The list of callbacks triggered by the execution of the Component
	    - **success**: Is called if the execution of the Component has succeeded. The first parameter of the callback contains the data provided by the component (if any)
	    - **error**: The error callback is triggered if the component has timed out or if its execution has failed. The first parameter of the callback contains the err object describing the reason for failing
        - **progress**: The progress callback allows you to retrieve data from the component before its execution ends. This is useful if the component execution takes a long time. This paremeter is optional and will not be triggered if progress method is not called by the component on the cloud. The first parameter of the callback is the data coming from the component (if any)
        - **always**: The always callback allows you to know when the execution of the component is done. It is triggered whether the component succeeds or not. This is useful for example to stop know when to hide a loading image if you had put one on your interface while the component was executing. The always callback is not triggered by the progress callback

**Example:**
```
Synchronise.Component.run("ID-OF-THE-COMPONENT", {/* param1:"val1"... */}, {
    success: function(data){
    },
    error: function(error){
    },
    progress: function(data){
    },
    always: function(){
	    // Called every time success or error is called
    }
});
```
