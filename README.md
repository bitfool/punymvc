This is Puny MVC, a little javascript MVC example.  It's too small to be a library of any use (hence, my altname Useless MVC).  But it may be instructive to some.  

It follows (way too closely) the examples given by Peter Michaux in his blog post from last year, 

   http://peter.michaux.ca/articles/mvc-architecture-for-javascript-applications 

which is really damn good reading.

Basics: 
This (like Peter's MVC Clock example) is a hybrid... there is a Model, and a Controller and a View, neatly separated, but there is also a Widget, which Peter describes as a semi-heretical construct whereby a view incorporates aspects of a controller unto itself and communicates directly with the Model.  There is also a Mediator, of sorts, which is of the Observer type.  In short your various UI components become Observers that can communicate with the Model (directly through their Widgets or indirectly through their Views to Controllers), and the Model can message out to the Observers.  Finally, there is a Bootstrap section to set everything up and get started.

Unlike Peter, I tried to abstract the meat of the Model and Observer out and into its own function, called a Puny.  The Puny handles all the Observer stuff, to unclutter the Model code.

Let's start at the bottom: 

Bootstrap:   To run this program, we do just a few things.

<code>
            //this inits the Puny, which keeps track of observers
            var puny = makePuny();   
            
            //this inits the custom model, and passes in the Puny
            var curseModel = makeQuoteModel(puny);   
            
            //this inits the Controller, and passes in the Model
            // (the Controller manages the UI start/stop/reset buttons)
            var curseKnobsController = makeCurseKnobsController(curseModel);
            
            //this inits the View, and passes in the Controller
            // (the View creates the UI start/stop/reset buttons)
            var curseKnobsView = makeCurseKnobsView(curseKnobsController);

            //this inits the Widget, and passes in the Model
            // (the Widget creates the UI blockquote for the Shakespeare)
            var cursingShakespeareWidget = makeCursingShakespeareWidget(curseModel);

            //the View and Widget have constructors for all the DOM elements, 
            //  so here's where we use them to build the DOM
            document.body.appendChild(curseKnobsView.getRootEl());
            document.body.appendChild(cursingShakespeareWidget.getRootEl());
</code>

That's pretty easy, nice and organized.  The Widget displays a random bowdlerized Shakespearean quote that is generated at 1 second intervals by the Model.  The View has Start/Stop/Reset buttons to control the actions of the Widget (via the Controller, and the Model).

But you can read code.  The actual meat of the sample is silliness, but I like the organization and maybe will build on it a bit in the future.