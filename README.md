# *j*ωiXML Processor

John Lumley

2024feb13

This is the README for the *j*ωiXML  *Invisible XML* processor, version 1.1.0, 
to run from an XSLT program executing in a browser, using SaxonJS and a custom-constructed
ixml 'engine' written in JavaScript. It contains the necessary JavaScript library, an XSLT 'stub' set of functions to invoke from XSLT,
and a sample application. An [interactive iXML 'workbench'](https://johnlumley.github.io/jwiXML.xhtml) is also available.

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
I consider it to have reasonable production status, with fully-functioning code that passes all (non-infinitely-ambiguous) grammar and parse tests
in the current test suite. The areas under development/change include:

- Checking much more accurately for infinitely ambiguous grammar cases (e.g. `S: S; 'a'.`). 
  Currently there is a limit of 1000 state steps in the per-character processing - if this is exceeded, a loop is assumed.
- Having an option to produce a single consistent XML parse tree / error report as the spec. states, rather than
  the current multi-valued map.
- Increasing the set of configuration options and supporting different output serialisations, e.g. JSON.

## Conformance
The processor passes all non-infinitely-ambiguous tests in the [test suite](https://github.com/invisibleXML/ixml/tree/master/tests). 

Currently the API parsing functions produce an XDM map as output, within which the result tree is the value of an entry, 
rather than a (serialised?) document, as required by the standard.

I have decided that this processor will *not* conform to support processing of infinitely ambiguous grammars, 
as apart from theoretical completeness they are of no practical value. 
As stated above the processor will stop parsing after processing 1000 consecutive states
for a character classification.


## Supplied files
There are four types of files in this distribution:

1. The compiled Javascript file that defines the iXML processor:   `dist/jwiXML.adv.cls.js`.  
1. An XSLT library file (`dist/jwiXML.processor.xsl`) which provides a small set of 
    XSLT functions for compiling iXML grammars to run in the processor.
    See below and comments within that file for descriptions of those functions. 
1. A sample web page (`sample/jwiXMLsample.xhtml`) which loads SaxonJS and the iXML processor
    and executes the exported form (`sample/jwiXML.sample.sef.json`) of a trivial 
    sample XSLT program `jwiXML.sample.xsl`. 
1. A runtime version of SaxonJS 2.6 (in directory `SaxonJS`) referenced from the web page, 
    and included subject to the LICENCE file therein.
 
## Interactive workbench
An [interactive iXML 'workbench'](https://johnlumley.github.io/jwiXML.xhtml) using *j*ωiXML is available,
from which grammars and input strings can be loaded, edited and processed from a variety of sources 
including the InvisibleXML [test suites and sample grammars](https://github.com/invisiblexml/ixml/), as well as local filestores. 

Version 1.4 of the workbench (released February 2024) uses version 1.1.0 of the processor

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
    
This file also contains XSLT-coded implementations of
  - [fn:invisible-xml()](https://qt4cg.org/specifications/xpath-functions-40/Overview.html#func-invisible-xml") (in the <code>jwL:</code> namespace), corresponding to the signature and semantics given in the current draft of
                     [XPath and XQuery Functions and
                     Operators 4.0](https://qt4cg.org/specifications/xslt-40/Overview.html) 
     
(See the comments in the XSLT file for more details.)
 
## JavaScript invocation
 To be completed
 
## Change History
### 2024feb13 - V 1.1.0
  - Corrected an error on syntax reformatting of multi-element repetition separators e.g.`body++(s1,s2)`
  - Corrected a state propagation fault that manifested itself in failures in parsing with the XPath
  grammars when some names were not followed by whitespace. For example parsing the expression
  `a/(b|c)/d` succeeded whereas `a/(b |c)/d ` (trailing whitespace) failed.
  - Added an XSLT implementation of [fn:invisible-xml()](https://qt4cg.org/specifications/xpath-functions-40/Overview.html#func-invisible-xml") (in the `jwL:` namespace)
  - Added support for iXML version 1.1 features, most notably the `naming` production of 
    [Invisible XML Specification - Working Draft](https://invisiblexml.org/current/) where
    `name`s can have optional `alias`es to be used in XML tree production.
  - Added experimental support for processing multi-character quoted strings *en-bloc* rather than
    character-by-character
  - Added very experimental support for limited use of regular expressions in character classes (inclusions and exclusions)
    and repetititons of both classes and quoted strings
  - Added more API use examples in the sample application, including invocation of `jwL:invisible-xml()`




**It's called *j*ωiXML for reasons induced by my electrical engineering background.**

