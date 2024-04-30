SimWrapper version of Papa Parse
================================

SimWrapper uses a modified version of the CSV-parsing "Papaparse" library to address what we consider a bug in Papaparse's handling of quoted string fields and to enhance comment support.

### 1. Quoted fields

Papaparse has a nice "dynamic typing" feature which converts numeric and boolean fields into actual numbers and booleans in the javascript objects that it returns. Unfortunately, it will convert fields even if they are surrounded by double-quotes. This is absolutely counterintuitive, because our users (most people generally?) use double-quotes to declare that a field is a string, full-stop. 

This behavior causes big problems when a field contains something like a US ZIP Code, for example. Boston's ZIP code is 02134.  Papaparse returns that as the number 2,134, which is WRonG! So then the user surrounds the data in that field in double-quotes "02134" which is **obviously and unambiguously a string**, and Papaparse still returns the number 2,134. 🤬

We've also encountered network files where link ID's have leading zeroes, and these also end up getting parsed incorrectly. 

Users know that putting double-quotes around a field in Excel will force Excel to interpret that cell as a string even if its content is numeric.

So, this patched version of PapaParse does the same thing: Even when the library's dynamic typing option is enabled, fields surrounded with double-quotes will always be returned as strings, not numbers/booleans. ✅

### 2. Comments in CSV files

Papaparse includes an option to skip lines beginning with a comment tag, and SimWrapper uses the `#` hash/number sign to mark lines that are to be skipped. 

But Papaparse throws these comments away, whereas SimWrapper would like to get some meta information
about the CSV file using comments. For example, some SimWrapper plugins look for the EPSG coordinate reference system code in comments.

So this modified version of Papaparse builds an array called `comments` containing the list of comment lines that is
returned as part of the `{data, meta, errors, comments}` object that Papaparse returns.


Parse CSV with JavaScript
========================================

Papa Parse is the [fastest](http://jsperf.com/javascript-csv-parsers/4) in-browser CSV (or delimited text) parser for JavaScript. It is reliable and correct according to [RFC 4180](https://tools.ietf.org/html/rfc4180), and it comes with these features:

- Easy to use
- Parse CSV files directly (local or over the network)
- Fast mode ([is really fast](http://jsperf.com/javascript-csv-parsers/3))
- Stream large files (even via HTTP)
- Reverse parsing (converts JSON to CSV)
- Auto-detect delimiter
- Worker threads to keep your web page reactive
- Header row support
- Pause, resume, abort
- Can convert numbers and booleans to their types
- Optional jQuery integration to get files from `<input type="file">` elements
- One of the only parsers that correctly handles line-breaks and quotations

Papa Parse has **no dependencies** - not even jQuery.

Install
-------

papaparse is available on [npm](https://www.npmjs.com/package/papaparse). It
can be installed with the following command:

    npm install papaparse

If you don't want to use npm, [papaparse.min.js](https://unpkg.com/papaparse@latest/papaparse.min.js) can be downloaded to your project source.


Homepage & Demo
----------------

- [Homepage](http://papaparse.com)
- [Demo](http://papaparse.com/demo)

To learn how to use Papa Parse:

- [Documentation](http://papaparse.com/docs)

The website is hosted on [Github Pages](https://pages.github.com/). Its content is also included in the docs folder of this repository. If you want to contribute on it just clone the master of this repository and open a pull request.


Papa Parse for Node
--------------------

Papa Parse can parse a [Readable Stream](https://nodejs.org/api/stream.html#stream_readable_streams) instead of a [File](https://www.w3.org/TR/FileAPI/) when used in Node.js environments (in addition to plain strings). In this mode, `encoding` must, if specified, be a Node-supported character encoding. The `Papa.LocalChunkSize`, `Papa.RemoteChunkSize` , `download`, `withCredentials` and `worker` config options are unavailable.

Papa Parse can also parse in a node streaming style which makes `.pipe` available.  Simply pipe the [Readable Stream](https://nodejs.org/api/stream.html#stream_readable_streams) to the stream returned from `Papa.parse(Papa.NODE_STREAM_INPUT, options)`.  The `Papa.LocalChunkSize`, `Papa.RemoteChunkSize` , `download`, `withCredentials`, `worker`, `step`, and `complete` config options are unavailable.  To register a callback with the stream to process data, use the `data` event like so: `stream.on('data', callback)` and to signal the end of stream, use the 'end' event like so: `stream.on('end', callback)`.

Get Started
-----------

For usage instructions, see the [homepage](http://papaparse.com) and, for more detail, the [documentation](http://papaparse.com/docs).

Tests
-----

Papa Parse is under test. Download this repository, run `npm install`, then `npm test` to run the tests.

Contributing
------------

To discuss a new feature or ask a question, open an issue. To fix a bug, submit a pull request to be credited with the [contributors](https://github.com/mholt/PapaParse/graphs/contributors)! Remember, a pull request, *with test*, is best. You may also discuss on Twitter with [#PapaParse](https://twitter.com/search?q=%23PapaParse&src=typd&f=realtime) or directly to me, [@mholt6](https://twitter.com/mholt6).

If you contribute a patch, ensure the tests suite is running correctly. We run continuous integration on each pull request and will not accept a patch that breaks the tests.
