# Update Functions

CouchDB uses update functions to parse an arbitrary HTTP request into JSON that gets saved to the database.

A good example of update functions can be found in the [CouchDB tests](http://svn.apache.org/repos/asf/couchdb/trunk/share/www/script/test/update_documents.js).

Update functions are saved in `updates` on the design document, and take two parameters:
```js
// haul/updates/new
function(doc, req) {
  log(doc);
  log(req);
}
```

### parameters

The first parameter (`doc`) will be populated with the document, if the update function was requested with a document id.  The second parameter (`req`) will be populated with information about the request environment.  The `req` object for a form creating a new document looks something like this:

```js
{"info": 
    {
        "db_name":"loot",
        /* and many more */
        "committed_update_seq":27
    },
    "id":null,
    "uuid":"7f8a0e3833bcc7161cfab80275221dc1",
    "method":"POST",
    "path":["loot","_design","haul","_update","new"],
    "query":{},
    "headers":{"Accept":"application/xml,application/xhtml+xml,text/html;q=0.9,text/plain;q=0.8,image/png,*/*;q=0.5" /* and many more */},
    "body":"name=Jeff",
    "peer":"127.0.0.1",
    "form":{"name":"Jeff"},
    "cookie":{},
    "userCtx":{"db":"loot","name":null,"roles":[]}
}
```

Since no ID was passed, the request doesn't have an ID.  However, the CouchDB server helpfully throws in a UUID so you can create a new document with a unique ID and be sure it won't conflict with any document in the database already.

The server also parses the POST body into a JSON tree called `form` and does the same with the query string, in `query`.

### usage

You can see that update functions are called in the same manner as their show and list cousins.

```html
http://127.0.0.1:5984/database/_design/ddoc/_update/updateFunction/(docId)
```

### return

The update function is tasked with returning an array with two members:

```js
[doc,response]
```

The first member of the returning array is the document you are saving to the database.  If you are updating the document, it should already have an `_id` set, and if you are creating a new document, make sure to set its `_id` to something, either generated based on the input or the `req.uuid` provided.

The second member is the HTTP response. This can be a javascript object with headers and a body:

```js
var resp =  {
  "headers" : {
    "Content-Type" : "application/xml"
  },
  "body" : doc.xml
};
```

or just a plain string:

```html
<p>Update function complete!</p>
```

Though you can set the headers, right now the status code for an update function response is hardcoded to be 200/201 unless an error occurs.  This is a bug.

### full example

Just like in list and show functions, you can access CommonJS modules in your code, which makes for easy path parsing and templating.  An example of a full update function using Mustache templates:

```js
function(doc, req) {
    var ddoc = this;
    var Mustache = require('vendor/couchapp/lib/mustache'); 
    var path = require("vendor/couchapp/lib/path").init(req);

    var newDoc = {};
    newDoc._id = req.uuid
    newDoc.name = req.form.name;
    newDoc.haul = ["Christmas Cheer!"];

    var data = {"redirect": path.list("index-hauls","all-hauls")};
    var resp = Mustache.to_html(ddoc.templates.redirect, data, ddoc.templates.partials);

    return [newDoc,resp];
}
```

This creates a new document, and then renders a template that uses javascript to redirect the user to the list function.

### Things to think about

* we seem to ignore \_rev and the optimistic concurrency used by CouchDB when using update functions.  I may just be missing something
* update functions allow you to update and create documents by POST requests from forms, allowing you to degrade gracefully when client-side JS support is not available
