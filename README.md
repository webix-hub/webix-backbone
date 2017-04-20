Backbone adapter for Webix UI
==========================

[![npm version](https://badge.fury.io/js/webix-backbone.svg)](https://badge.fury.io/js/webix-backbone)

Before Webix 4.3 this module was part of webix.js

See the detailed documentation on [integration of Webix with Backbone](http://docs.webix.com/desktop__backbone.html).

How it can be used
-------------

Webix and Backbone can be cross-integrated on multiple levels:

- Webix components can load data from backbone collection, check [Backbone Collections](http://docs.webix.com/desktop__backbone_collections.html) for more details;
- Webix components can be wrapped in a Backbone View, check [Backbone Views](http://docs.webix.com/desktop__backbone_views.html) for more details;
- visibility of Webix Components can be controlled by Backbone router, check [Backbone Routers](http://docs.webix.com/desktop__backbone_routers.html) for more details.

Making Preparations
------------------

To integrate Backbone framework into your webpage, your should include links to it into the document head. Be attentive to specify the right relative paths to the places where the files
are stored on your machine. The important point - library need to be included BEFORE webix.js: 

~~~html
<script type="text/javascript" 
	src="http://code.jquery.com/jquery-1.7.1.min.js"></script>
<script type="text/javascript" src="./common/underscore.js"></script>
<script type="text/javascript" src="./common/backbone.js"></script>

<link rel="stylesheet" href="../../codebase/webix.css" type="text/css" 
	media="screen" charset="utf-8">
<script src="../../codebase/webix.js" type="text/javascript" charset="utf-8"></script>
~~~


Creating Views
-----------------

In an integrated app, pure Backbone views and Backbone-Webix views co-exist. 

Backbone views are created under Backbone standard pattern:

~~~js
cView = Backbone.View.extend({
	tagName: "h2",
	render: function(){
		$(this.el).html("Child View");
	},
});
var v2 = new cView();
v2.render();
~~~

Backbone-Webix integrated views resemble this pattern yet feature a **config** property where Webix configuration is stored. To instantiate such a view, a special **WebixView()** constructor is used:

~~~js
var ui_config ={
	type:"wide", rows:[
		{ template:"top", height:35 },
		{ type:"wide", cols:[ ..cols ..]},
		{ template:"bottom", height:35 }
	]
};

var view  = new WebixView({
	el: ".app1_here", // will be rendered to div with "app1_here" class
	config:ui_config
}).render();
~~~

**Related sample:** [View](https://webix-hub.github.io/webix-backbone/samples/01_view.html)

In the code above Webix layout with 3 rows and columns in the second row is created. It is rendered to a div block with *"app1_here"* class. As a rule, views should be **manually rendered** 
with a dedicated function. 


Creating Models and Collections
------------------------

~~~js
FilmRecord = Backbone.Model.extend({});
FilmList = Backbone.Collection.extend({
	model: FilmRecord,
	url:"./common/data.json" //adding data to collection
});
~~~

Loading the Collection into a View
-------------------------

First, lets create a Webix data component, for instance a List view:

~~~js
$(".app1_here").webix_list({
	id:"mylist", width:200,
	template:"#title#", select:true
});
~~~

Now, when we have a view and a model we can render the model into the list view with the syncing method:

~~~js
//get data
var films = new FilmList();
films.fetch();

//sync collection data with list 
$$("mylist").sync(films);
~~~

**Related sample:** [Loading data from Collection](https://webix-hub.github.io/webix-backbone/samples/03_load_collection.html)

The code above renders List view and fills it with data from FilmList Collection that is comprised of FirmRecord Models.

The sync between data collection and the component can be as well removed by the opposite method: 

~~~js
$$("mylist").unsync(films);
~~~
