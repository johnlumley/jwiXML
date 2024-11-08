# QT4 XPath/XQuery grammars

John Lumley

2024nov05

This folder contains 'near-current' XPath and XQuery 4.0 grammars in iXML format.


I've been involved in the [QT4 Community Group](https://qt4cg.org/) since its
            start, and have been using the grammars for XPath and XQuery as large test examples for
            this iXML processor. These grammars are also available to support experimentation for
            those involved in that community to perhaps handle 'what-if' experiments on the
            grammar.

This folder contains iXML grammars for the 'near-current'
            draft 4.0 versions of the XPath and XQuery grammars, as well as the variants for XSLT patterns, and XQuery Update and Fulltext.. These are derived programmatically
            (with a modicum of automated patching) directly from the grammar definitions used to
            generate the specification grammars [XPath
               EBNF](https://qt4cg.org/specifications/xquery-40/xpath-40.html#id-grammar) and [XQuery
               EBNF](https://qt4cg.org/specifications/xquery-40/xquery-40.html#id-grammar). (See my [Balisage paper](https://www.balisage.net/Proceedings/vol29/html/Lumley01/BalisageVol29-Lumley01.html) for details of how these are constructed.)

The grammars are presented in three different versions of iXML:

- `1.0`, the published standard,
- `1.1`, the current draft which noteably adds the ability to rename
  nonterminals (`A>b`). This is used in areas such as <code>QName</code>s to
  accomodate both prefix and local parts as attributes, and
 
- `1.1+`, being the `1.1` version with the addition of a
  set-subtraction operator (see below), which is used to exclude certain reserved
  keywords from some name concepts, such as `function` being reserved from
  use as the name of a function call. As far as I'm aware, **this iXML feature is not
  currently supported by any other implementation** and whilst having been
  proposed as an additional part of iXML, decisions on it have not yet been made.

For both these grammars, there is also a 'reduced tree' version which truncates the very
deep trees that normally result from parsing.

All these grammars are available as samples (with example expressions to test)
in my in-browser [interactive iXML 'workbench'](https://johnlumley.github.io/jwiXML.xhtml).

Each of the (iXML) grammars contains a date-stamp in a comment near the top which can
be used to verify whether it is 'up to date', by comparison with the history of the
grammar definition file: [https://github.com/qt4cg/qtspecs/blob/master/specifications/grammar-40/xpath-grammar.xml](https://github.com/qt4cg/qtspecs/blob/master/specifications/grammar-40/xpath-grammar.xml)

Whilst the grammars have been tested across the 35k expressions of the QT4 test sets,
 with only some 50 failures, there are known areas where the grammar fails or produces
ambiguity. These notes are intended to explain.
         
            
### Failures
            
- The iXML grammars (which have to effectively act as their own tokenizers) are
  very 'whitespace sensitive' and several failures occur when certain operator
  symbols are directly adjacent to operands. The most common is a minus operator
  (`-`) unsurrounded by whitespace, e.g. `1-1`, but others can
  include examples such as `if(foo)else`, etc. Attempting to cure these
  will give rise to explosive ambiguity, especially in supporting a binary minus
  without surrounding whitespace. In the 35k tests, these failures occur about 20 times.
- Some very long sections of string/content, e.g. fully embedded transforms within
  a string initializer, can trigger the parser's 'looping' limit (no more than 1000
  additional states can be propagated for parsing a single character of the input.) 
            
         
### Ambiguities
The grammars, lacking a tokenizer and the ability to look-ahead without consumption,
retain a few ambiguities that are inherent in the basic EBNF grammars 
(i.e. without the explanatory notes and tokenization strategy)
           
- Function signatures with trailing occurrence indicators `fn(){..} as a?`
- Ambiguity from `/*/`, where the `*` can be a wildcard or a multiplication
- Colons in QNames and maps, e.g `map{a:*:c}`
         
         
            
### The Set-subtraction operator
The grammars use an experimental 'set-subtraction' operator `¬` to
indicate certain reserved keywords such as limitations on function names:

```
         FunctionCall: EQName ¬ reservedFunctionNames, ArgumentList.
reservedFunctionNames: types |
                       commands.
                types: "attribute"; "comment"; ... .
             commands: "fn"; "function"; "if"; "switch"; "typeswitch".
```

Removal of the operator and its RH argument will permit the grammar to run on a 1.0
conformant iXML processor, though now `comment()` can be ambiguous. The
operator is currently used on the productions for `FunctionCall`,
               `NamedFunctionRef`, `DirCommentContents`,
               `CompPIConstructor`, `ElementContentChar`,
               `QuotAttrContentChar` and `AposAttrContentChar` and indirectly
               from `CompElemConstructor` and `CompAttrConstructor`. 
               The `1.0` and `1.1` versions do *not* use this operator.

### Change Log
**2024nov05** - Updated grammars to current, added grammars for XSLT Patterns [https://qt4cg.org/specifications/xslt-40/Overview.html#pattern-syntax], 
XQuery Update and XQuery FullText variants. Changed grammar names to drop `.extracted`.
