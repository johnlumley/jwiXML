# *j*ωiXML Processor

John Lumley

2022jun09

This is the README for my attempt at an *Invisible XML* processor 
that runs in the browser using SaxonJS and a custom-constructed
ixml 'engine' written in Javascript. For fuller details of Invisible XML see [https://invisiblexml.org/](https://invisiblexml.org/). 
For details of SaxonJS see [Saxonica](https://www.saxonica.com/saxon-js/documentation2/index.html)

## One-stop setup and go
Firstly unzip the file `'jwiXML.zip` into a folder in your localhost web server. 
Then load the web page `jwiXML.xhtml` into your browser from your local host.. 

There are two sections, which can be viewed by clicking the appropriate expander:
-  **Predefined Test examples**, where you will see some example iXML grammars, 
their parses and some input strings and results of parsing those strings against the supplied grammar.
These examples are defined in the file `jwiXML.xml` or other files indirected therefrom - see the notes in that file.
- **Interactive Processor**, where you can load or edit grammars and try seeing whether 
differing input strings parse against that grammar and what the result is. Clicking on a green sample grammar will load it. 
Clicking on **Go!** will first parse/compile the grammar and if successful, and there is a non-empy input string,
parse that string against the grammar. 
Check boxes control whether you will only get one answer in the case of ambiguous solutions, 
and a display of the parser states.  


(Note that the difference between a parsed and compiled grammar is that in the latter 
the grammar has been *canonicalised* for execution into a BNF form:
repetition constructs, bracketed alternates and multi-character quoted strings have been expanded. 
The difference can be examined using the *parsed*/*compiled* radio buttons. 
The grammar can be viewed either in the ixml (texual) format or the XML form, dependent on the *XML"/*ixml* radio buttons .)




## Status of the implementation
At present it appears to be running pretty well and is capable 
of processing the ixml specification using the grammar of the ixml specification. in O(300ms).
This is still experimental, but fully-functioning code that passes all grammar and parse tests
in the current test suite. The areas still under develoment/change include:

- Checking for an infinitely ambiguous grammar (e.g. `S: S; 'a'.`). 
  If a parse is attempted against such a grammar, currently you'l have to kill your browser window to get out!
- Producing a consistent XML error report as the spec. states.
  If there are errors, in some cases you may get a `SaxonJS.XError` thrown.

## Supplied files
There are five types of files in this distribution:

1. The compiled Javascript file that defines the iXML processor:   `jwiXML.adv.cls.js`.   
1. A web page and associated CSS (`jwiXML.xhtml` and `jwiXML.css` and the directories `styles` and `logos`) that references the Javascript files above 
and SaxonJS2 and then invokes `SaxonJS.transform()`, with suitable invocation arguments.
1. An exported XSLT program for SaxonJS (`jwiXML.sef.json`) which processes a control file (`jwiXML.xml`). 
    This collects and executes simple tests designed for developing and examining the processor, 
    as well as providing an interactive ability to edit and select grammars and inputs, showing the results
    as and giving other diagnostic information such as tracing Earley parser execution and so forth.
1. Some simple test or sample declarations (in `jwiXML.xml` or `myTests/*.xml`) defined in XML as `test[grammar][input*]`
    or `sample[@href]` constructs, where the grammars and the inputs
    can be supplied as text or collected from a file.
    Grammars can be presented in text, or (with a `@syntax = 'xml'`decoration) as the XML form of an ixml grammar.
1. A runtime version of SaxonJS (in directory `SaxonJS`) referenced from the web page, 
    and included subject to the LICENCE file therein.
 

## Invocation from other programs 
I fully intend this processor to be invocable from either SaxonJS/XSLT programs 
or directly from Javascript (albeit within a browser for now - the browser DOM is employed). More details will appear here. 
### SaxonJS/XSLT invocation
 To be completed
 
### JavaScript invocation
 To be completed



**It's called `jωiXML` for reasons induced by my electrical engineering background.**


