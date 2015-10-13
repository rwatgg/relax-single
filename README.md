




	      ___           ___           ___       ___           ___     
	     /\  \         /\  \         /\__\     /\  \         |\__\    
	    /::\  \       /::\  \       /:/  /    /::\  \        |:|  |   
	   /:/\:\  \     /:/\:\  \     /:/  /    /:/\:\  \       |:|  |   
	  /::\~\:\  \   /::\~\:\  \   /:/  /    /::\~\:\  \      |:|__|__ 
	 /:/\:\ \:\__\ /:/\:\ \:\__\ /:/__/    /:/\:\ \:\__\ ____/::::\__\
	 \/_|::\/:/  / \:\~\:\ \/__/ \:\  \    \/__\:\/:/  / \::::/~~/~   
	    |:|::/  /   \:\ \:\__\    \:\  \        \::/  /   ~~|:|~~|    
	    |:|\/__/     \:\ \/__/     \:\  \       /:/  /      |:|  |    
	    |:|  |        \:\__\        \:\__\     /:/  /       |:|  |    
	     \|__|         \/__/         \/__/     \/__/         \|__|    
	, we are the good guys


# Relax Single

A Single Page Application Javascript framework that has all the wistles to make your website as fast as possible on the modern web.

Takes minutes to setup.

Can be connected to any CMS that supports JSON or setup with a static JSON file.

Supports standard, nested, overlay pages styles.

Has a simple event system that allows for a free component structure.

Build on Backbone for maximum code freedom.

To be made public end of 2015.





The framework works out of the box with a static JSON file. Check out the examples/page-types branch.

But it is primairly used to ajax-call a server and receive a JSON response data, see example branches for details.

The repo includes — other than the framework it self.
- a gulp setup that is geared towards how we work at RWATGG. In theory the Gulp files should not be modified pr. project.
- stylus mixins 
- Debug view for the framework, to get up and running with routes and all pages real quick

Dependencies
Backbone
RelaxLib (insert link)




#### Get started:

* NPM:

		npm install

* Bower:

		bower install

* Gulp:

		gulp build
		gulp deploy-dev (css,js,images,ejs)
		gulp deploy-prod (deploy + cdn)
		gulp upload-file --file <path> 


Project requires a ftp.json file



#### TODO:

* Create a Umbraco specific branch.


package.json specific stuff (should be placed in Umbraco repo)

	"NET_WebsitePath" ex.: "/Katvig/Katvig.Website"
	"remoteViews" ex.: "katvig.dk/Views/",
	"remoteConfig" ex.: "katvig.dk/Config/"

add scripts to be ignored in build process by adding them to scripts_ignore in package.
Be aware that watch task ignores vendors folder.

Features ...

1. Allow pages to cache them selves upon interaction

		<a href="..." single-cache="true">


1.1 to bind elements that are not apart of a DefaultPage scope
		
		application.pages.bindCacheElements()

2. add overlay pages
An overlay page does not remove it's previous page

		application.pages = new PagesCollection
			overlayTypes: ["signup"]
			...

3. Testing get started quickly, use Project folder to test static json.


4. single-boilerplate.css will clear out any bad styles.

5. Allow for pre-fetching of pages.

		application.pages.fetchCache(href)

6. Animation control
Listen for page animation state changes

Defaultly trigged from DefaultPage, but can be further controlled by overwriting these two methods:

	protected animateIn () ...
	protected animateOut (newModel: PageModel) ...

Remember to trigger states when your animations are done, or call super.
	
	this.trigger(DefaultPage.ON_ANIMATED_IN)
	this.trigger(DefaultPage.ON_ANIMATED_OUT)

7. New pages or global UI components can listen for the current PageModel instance or the "prev-model" from a PageModel instance.

		application.pages.on("updated", this.onPageUpdate, this);
		private onPageUpdate(model: PageModel) ...






#### Check branches:
* **page-transition**: see how to make a transition between two pages
* **page-types**: see how to create individual pages


		@pages = new PagesCollection
			updateTypes: [array of page types that act as a updateable page]
			ignoredCacheTypes: [array of page types that should not be cached (hover etc.)]
			modelClasses:
				"*": PageModel
				"storelocator": StoreLocatorModel
				set specific page models (always extend fra PageModel)
			$context: $("#content") where to add pages to
			rootURL: "/" 


		@router = new Router
			pages: @pages // mandatory
			routes: window.routes // Mandatory [object Object]
			tracking: new GATracking() // optional, expects the interface of GATracking


		// define templates in window.templates
		// bind pages types with template types
		PageModel.addAliasRoutes
			"home": "grid"
			"about": "grid"


Routes get's set via routes option in "new Router"
routes accept two types of definitions
List or arrays

Array example

	window.routes =
		frontpage: "/en"
		listpage: Array[1]
		section: Array[3]
		textpage: Array[12]
			0: "/en/about/who-we-are"
			1: "/en/about/resist-the-usual"
			2: "/en/network"
			...

List example
	
	window.routes =
		about: "/about"
		artist: "/artists/:artist"
		artists: "/artists"
		contact: "/contact"

These can be combined...
