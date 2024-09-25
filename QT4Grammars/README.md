# QT4 XPath/XQuery grammars

John Lumley

2024dsep25

This folder contains 'near-current' XPath and XQuery 4.0 grammars in iXML format. An [interactive iXML 'workbench'](https://johnlumley.github.io/jwiXML.xhtml) is also available.

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



