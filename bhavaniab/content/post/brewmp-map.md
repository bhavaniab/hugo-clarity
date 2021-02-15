+++
author = "Bhavani Anantapur Bache"
title = "Displaying map between two cities on BREW MP"
date = "2013-08-16"
description = "Displaying map between two cities on BREW MP"
featured = true
tags = [
    "RTOS",
    "Embedded Systems",
    "BrewMP",

]
categories = [
    "RTOS",
    "Embedded Systems",
    "BrewMP"
]
series = ["Embedded Systems"]
aliases = ["migrate-from-jekyl"]
thumbnail = "images/brewmap.jpg"
+++
Maps are one of the most important applications on the feature phones. The post describes development of an application to display map between two cities on BrewMP mobile platform. The UI has two text fields To and From, where source and destination are entered. Upon pressing the SELECT key, the application loads the  map between two cities.  Along with the basic UI, a surface media widget was initialized and associated with the root container. This was integrated with the backend Webkit rendering engine. When the select key was pressed, a query with the data from two text fields is passed on to the google maps, through the IWeb. The Iweb then gets the data from the google server, and the data is passed on to the webkit engine, which in turn renders the web page.
<h3>Overview of the User Interface</h3>
A simple user interface was developed which has two prop containers, each containing a static widget and a text field. Prop widgets are placed in root container, along with the surface media widget. The surface media widget is responsible for displaying web view.

<h3>Setting up of root container and Canvas</h3>
Using window manager, create a full screen window and associate the root widget with the window widget. Create an instance of root container and display canvas. Associate root container and display canvas with one another.
The ICanvas interface provides a drawing surface for widgets and other components to draw upon.  ICanvas is used in conjunction with IDisplay and IRootContainer. Typically at the startup of the application, you get an ICanvas.
IDisplay can be setup using 

`````C++
IDisplayCanvas_SetDisplay();
`````
Finally ICanvas can be set to Root Container using 

`````C++
IRootContainer_SetCanvas();
`````
A new instance of the display canvas class object may be created by calling ISHELL_CreateInstance() and passing in AEECLSID_DisplayCanvas.  This way the root container (and any widgets contained in it) can actually display to the screen.

Enable touch on the root container 
`````C++
IWidget_EnableTouch(pMe->piwRootwidget);
`````
<h3>Surface media widget in BREW MP</h3>
The surface media widget is a user interface element for intergrating surfaced media objects, such as video or camera,web view area within a widget tree. This media source is contained in within a SurfacedMediaModel, and its content is rendered to a surface or surface container specified by the widget. Typically, widget sets the media size based on its own extent, by calling ISurfacedMediaModel_SetSize().

To create a surface media widget, an instance of the surfaced media widget using AEECLSID_CSurfacedMediaWidget needs to be initialized. Configure the widget and set the extent using IWidget_SetExtent. Provide the destination surface container to the widget. Create an instance of ISurfacedMediaModel and call IWidget_SetModel on the widget. Surface media widget is associated with the model interface. This enables the widget to listen to and handle various touch events from the webpage.  The surface media widget is interfaced with the webview of the Webkit backend rendering engine to display web pages.

<h3>IWeb Application in BREW MP</h3>
The main purpose of the IWEB API is to make HTTP requests using a BREW application. IWEB manages its own connections and sockets thereby making it much easier to do web transactions. I Web is responsible for establishing the connection to the server and then trasfering data to the local system using HTTP “GET” method.  When the IWeb engine gets response from ther server, a callback is called. The response can be extracted from callback using IWEBRESP_GetInfo. The response usually contains the header along with
the data that contains HTML tags.  Data is transfered to the webkit rendering engine for rendering the webpages.

