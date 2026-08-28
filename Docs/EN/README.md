# XML for Silex

The `XML` package builds, parses, traverses, and serializes strict XML
documents. It preserves document structure while explicitly rejecting
features that could read or expand external entities.

[Lire cette documentation en français.](../FR/README.md)

## Install the package

```text
silex install XML
```

Then import its public module:

```sx
use XML
```

XML requires Silex 0.39.0 or newer.

## Parse and traverse a document

`XML.parse` turns known text into a node tree:

```sx
use XML

func main() {
    var document = XML.parse(
        "<feed><entry id=\"1\"><title>Silex &amp; XML</title></entry></feed>"
    )
    var entry = document.elements("entry")[0]
    if let identifier = entry.attribute("id") {
        print("$(identifier): $(entry.text_content())")
    }
}
```

The program prints:

```text
1: Silex & XML
```

The parser preserves case, qualified names such as `atom:link`, processing
instructions, comments, and CDATA sections. It requires quoted attributes,
unique attribute names, matching closing elements, and exactly one root. The
five predefined XML entities and numeric character references are decoded.

## Build and serialize

`XML.document()` and `XML.element(name)` create documents without source text.
Nodes can then add elements, attributes, text, comments, and CDATA sections.
`XML.serialize` produces compact text by default; use `XML.Format.pretty` for
readable indentation.

## Understand the security limits

DTD declarations and custom entity expansion are rejected. An untrusted
document therefore cannot trigger external-entity access or exponential entity
expansion.

Version 0.2 preserves namespace declarations and qualified names but does not
yet resolve them into namespace URI and local-name pairs. Accepted XML names
currently match the portable ASCII subset.

`try_parse` returns structural and configured limit errors with their source
location. `parse` is the direct form for trusted application input.
