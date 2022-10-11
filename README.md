# *j*ωiXML Processor

John Lumley

2022oct11

This is the README for the *j*ωiXML  *Invisible XML* processor 
to run from an XSLT program executing in a browser, using SaxonJS and a custom-constructed
ixml 'engine' written in JavaScript. It contains the necessary JavaScript library, an XSLT 'stub' set of functions to invoke from XSLT,
and a simple sample application. An [interactive iXML 'workbench'](https://johnlumley.github.io/jwiXML.xhtml) is also available.

For fuller details of Invisible XML see [https://invisiblexml.org/](https://invisiblexml.org/). 
For details of SaxonJS see [Saxonica](https://www.saxonica.com/saxon-js/documentation2/index.html)

## One-stop setup and go
Firstly unzip the file `jwiXML.zip` into a folder in your localhost web server. 
Then load the web page `sample/jwiXML.sample.xhtml` into your browser from your local host.
You should see a display of a grammar, a very simple input and the resulting parse tree.

To generate your own application within a SaxonJS environment:

- Ensure you link to the iXML processor via a script reference in your top-level web page
- Include `jwiXML.processor.xsl` in your application and export the complete XSLT package/stylesheet,
    targetted for SaxonJS 2+ using SaxonEE in the normal way. 


## Status of the implementation
At present it appears to be running well and is capable 
of processing the ixml specification using the grammar of the ixml specification in O(300ms).
This is approaching production status, with fully-functioning code that passes all (non-infinitely-ambiguous) grammar and parse tests
in the current test suite. The areas still under development/change include:

- Checking much more accurately for infinitely ambiguous grammar cases (e.g. `S: S; 'a'.`). 
  Currently there is a limit of 1000 state steps in the per-character processing - if this is exceeded, a loop is assumed.
- Having an option to produce a single consistent XML parse tree / error report as the spec. states, rather than
  the current multi-valued map.
- Increasing the set of configuration options and supporting different output serialisations, e.g. JSON.


## Supplied files
There are four types of files in this distribution:

1. The compiled Javascript file that defines the iXML processor:   `dist/jwiXML.adv.cls.js`.  
1. An XSLT library file (`dist/jwiXML.processor.xsl`) which provides a small set of 
    XSLT functions for compiling iXML grammars to run in the processor.
    See below and comments within that file for descriptions of those functions. 
1. A sample web page (`sample/jwiXMLsample.xhtml`) which loads SaxonJS and the iXML processor
    and executes the exported form (`sample/jwiXML.sample.sef.json`) of a trivial 
    sample XSLT program `jwiXML.sample.xsl`. 
1. A runtime version of SaxonJS 2.5 (in directory `SaxonJS`) referenced from the web page, 
    and included subject to the LICENCE file therein.
 
## Interactive workbench
An [interactive iXML 'workbench'](https://johnlumley.github.io/jwiXML.xhtml) using *j*ωiXML is available,
from which grammars and input strings can be loaded, edited and processed from a variety of sources 
including the InvisibleXML [test suites and sample grammars](https://github.com/invisiblexml/ixml/), as well as local filestores. 

## SaxonJS/XSLT invocation
  The XSLT file `dist/jwiXML.processor.xsl` provides all necessary interfaces to compile and use iXML grammars to parse input strings.
  The three most frequently used are the functions:
  - `jwL:compileGrammar($grammar-source, $options?)` which compiles the iXML grammar 
     (supplied either as text string or an already-parsed XML version) and produces a 'Grammar'
     JavaScript object. `$options` is a map (defaulting empty when no argument is present)
     with the following possible entries: 
      - `twRewrites` as `xs:boolean` - use the 'Tovey-Walsh' rewrites for repetition
        ( `f+ => f-plus. f-plus = f | (f, f-plus).` ).
  - `jwL:parse($grammar,$input as xs:string) as map(*)` which parses the input string against the
     already-compiled grammar, returning a map with several entries, of which the `tree` member 
     contains the (main) parse tree or an error report.
  - `jwL:parseRecords($grammar,$input as xs:string, $separator as xs:string) as element()*` which parses the set of records 
     contained within the input string against the
     already-compiled grammar, returning a sequence of elements for each parse. `$separator` is treated as
     a regular expression matching the record separator character sequence.
     
(See the comments in the XSLT file for more details.)
 
## JavaScript invocation
 To be completed



**It's called *j*ωiXML for reasons induced by my electrical engineering background.**

