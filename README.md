# XML for Silex

`XML` builds, parses, traverses and serializes strict XML documents.

```sx
use XML

var document = XML.parse(
    "<feed><entry id=\"1\"><title>Silex &amp; XML</title></entry></feed>"
)
var entry = document.elements("entry")[0]
if let identifier = entry.attribute("id") {
    print("$(identifier): $(entry.text_content())")
}
```

The parser preserves case, qualified names such as `atom:link`, processing
instructions, comments and CDATA. It requires quoted attributes, unique names,
matching closing elements and exactly one root. The five predefined XML
entities and numeric character references are decoded.

DTD declarations and custom entity expansion are rejected explicitly. This
keeps untrusted documents free from external-entity access and exponential
entity expansion. Version 0.2 preserves namespace declarations and qualified
names but does not resolve them into namespace URI/local-name pairs. XML name
support is currently the portable ASCII subset.

`try_parse` reports source-located structural and configured limit errors;
`parse` is the direct form for trusted application inputs.
