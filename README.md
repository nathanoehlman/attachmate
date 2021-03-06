# attachmate - Streamable CouchDB attachments :)

An experiment in using @isaacs useful [fstream](https://github.com/isaacs/fstream) project to stream files in and out of [CouchDB](http://couchdb.apache.org/) as attachments to documents.

## Downloading From Couch to FileSystem

### Example: Download Attachments to the Filesystem

```js
var attachmate = require('attachmate'),
    fstream = require('fstream'),
    path = require("path"),
    
    r = new attachmate.Reader({ path: 'http://localhost:5984/testdb/doc_with_attachments' }),
    w = fstream.Writer({ path: path.resolve(__dirname, 'output'), type: 'Directory'});
    
// pipe the attachments to the directory
r.pipe(w);
```

### Example: Download Attachments (using download helper)

```js
var attachmate = require('attachmate'),
    path = require('path');
    
attachmate.download(
    'http://localhost:5984/testdb/doc_with_attachments', 
    path.resolve(__dirname, 'output'), 
    function(err) {
        console.log('done, error = ', err);
    }
);
```

## Uploading from Filesystem to Couch

### Options

The fstream classes take a set of options at initialization.  In keeping with the fstream standard, we use the `path` option to denote the target document.  The following additional options are also supported:

- `preserveExisting` - whether or not existing attachments on the document should be preserved. (default = true)

- `includeHidden` - whether or not hidden files should be included when uploading attachments to couch. (default = false)

### Example: Upload Attachments to the Filesystem

```js
var attachmate = require('attachment'),
    fstream = require('fstream'),
    path = require("path"),
    
    r = fstream.Reader({ path: 'input', type: 'Directory' }),
    w = new attachmate.Writer({ path: 'http://localhost:5984/testdb/test' });
    
// upload the contents of the input directory as attachments
r.pipe(w);
```

### Example: Upload Attachments (using upload helper)

```js
var attachmate = require('attachmate'),
    path = require('path');
    
attachmate.upload(
    'http://localhost:5984/testdb/test', 
    path.resolve(__dirname, 'input'), 
    function(err) {
        console.log('done, error = ', err);
    }
);
```
