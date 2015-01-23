Ask your questions here and someone who understands the topic will try to answer for you. The answers might be short or long, but the goal in the long run is to roll them into real documentation.

### Q: whats the deal with $.evently.connect(blah)

A: it just makes it so when blah gets triggered on A, it also gets triggered on B. Here's an example: [http://github.com/couchone/Focus/blob/master/\_attachments/index.html#L39](http://github.com/couchone/Focus/blob/master/_attachments/index.html#L39)

### Q: How can I use another jQuery library in evently functions? Even with hardcoded data: [http://people.iola.dk/olau/flot/examples/basic.html](http://people.iola.dk/olau/flot/examples/basic.html)

A: I would just put the code in `_init.js`, or `_init/after.js` if you wanted to run a mustache template first and then add flot.

A: Indeed. I also had to make sure to include the flot js script after loader.js.

### Q: How do I save a doc with a custom id number?

### Q: How do I pass objects from one event to the next? From the `loggedIn` event I want to call the `edit` event. I have `loggedIn.data : function(e,r)`, how do I get `r` into `edit.data : function(e,r)`?

In other words, I want to use `{{name}}` inside edit.mustache, but do not want to do `$.couch.session` again to retrieve the name. 

### Q: When i use `render:'prepend/append'` i can add more listeners to the elements of the newly prepended/appended element using selectors but not to the prepended/appended element itself. How can i do this?

### Q: How can I use desktopcouch-links from my browser? Just calling desktopcouch://mydb/\_design/myApp/index.html wouldn't work neither on Firefox nor on Chrome.

### Q: Is it possible to create CouchDB Views on the fly dynamically? Say i have an App where you can enter objects and define their type dynamically, i would like to be able to query a view returning all objects of that new type, in a metaprogramming (method_missing) way? Any ideas?

A: Yes, as you can read in [http://wiki.apache.org/couchdb/Introduction\_to\_CouchDB\_views#Concept](http://wiki.apache.org/couchdb/Introduction_to_CouchDB_views#Concept) there is a way to do an HTTP POST request to `/{dbname}/_temp_view` containing the code of the view function. It's called _temporary view_. But keep in mind it does not cache the results, and as such are slower than usual views.

### Q: I am using _list and _show functions that use the mustache syntax to load a header and sidebar ({{&gt;header}}  that is stored in /design\_doc\_name/tempates/partials/header.html and /design\_doc\_name/tempates/partials/sidebar.html.  I want to be able to load these same files for my static html pages (with evently widgets) that are in the attachments directory (ex. /design\_doc\_name/\_attatchments/index.html).  Is there an easy way to have the evently widget access my header and sidebar?

A: I found a work around.  Since all the .html files in /design\_doc\_name/tempates/ get stored as json when the design\_doc is "pushed," you can't use the jquery[  $("#sidebar1").load("templates/partials/sidebar.html"); ] so what I do now is make a copy of my templates/partials directory in \_attachments every time I "push" the design\_doc into the database.  It works, but I am sure there is a better way to do it!

### Q: i like the idea with the selectors and putting the code in separate files. but how do you differentiate between multiple <a> elements, with different attributes. i do not see how to make filenames starting with "." or "#" for id or class values. if the </a><a> element has an attribute of op="delete", should one have a filename like op="delete" ?? what is the best practice here?</a>
<a>

### Q: Add your Questions here.
</a>
