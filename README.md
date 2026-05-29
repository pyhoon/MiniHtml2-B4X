# MiniHtml2-B4X

Version 2 of the MiniHtml library for B4X — a fluent HTML builder for B4J/B4A/B4i.

Previous version: https://github.com/pyhoon/MiniHtml-B4X

## Overview

MiniHtml2 lets you construct HTML documents programmatically in B4X using an object-oriented, chaining API. Instead of concatenating strings, you build a tree of `MiniHtml` tag objects, then call `build` to render the final HTML string.

## Features

- **Fluent API** — Method chaining for concise, readable code
- **Automatic tag mode** — Self-closing (`<br>`, `<img>`), uniline (`<span>text</span>`), multiline (`<div>\n  text\n</div>`), meta (`<!DOCTYPE>`, `<meta>`), or no-tag (raw text)
- **Class & style management** — `addClass`, `removeClass`, `addStyle`, `removeStyle` with auto-sync to `class`/`style` attributes
- **Attribute helpers** — `attr`, `attr2` (map), `attr3` (boolean), plus convenience methods: `lang`, `required`, `disabled`, `checked`, `selected`, `hidden`, `readonly`
- **HTML parsing** — Parse existing HTML strings into MiniHtml objects (`Parse`, `ConvertFromBytes`, `ConvertToMiniHtml`)
- **CDN helpers** — `cdn`, `cdn2`, `cdn3` for adding `<script>` and `<link>` tags with integrity/crossorigin support
- **Comments** — `comment` (indented), `comment2` (inline)
- **Text wrapping** — `text`, `text2` (overwrite), `textWrap` (indented)
- **Output options** — `build` (no indent), `build2` (custom indent), `buildImpl` (custom indent + attribute alignment)
- **Flat/minified output** — Set `Flat = True` to suppress line breaks
- **Indentation control** — Customize indent string (default: `"  "`), control indent per-node
- **Format attributes** — `FormatAttributes = True` aligns attributes across lines
- **Child traversal** — `ChildByName`, `ChildById`, `ChildByIndex` with deep search

## Project Structure

```
MiniHtml2-B4X/
├── source/
│   ├── MiniHtml.bas        # Core class — HTML tag builder
│   ├── MiniHtmlParser.bas  # HTML parser (credits: Erel)
│   ├── MH.bas              # Static helper — factory methods for common tags
│   ├── Index.bas           # Demo handler (B4J servlet)
│   ├── MiniHtml2.b4j       # B4J project file
│   ├── manifest.txt        # Library manifest (v2.30, depends on B4XCollections)
│   ├── libs.json           # External library dependencies (EndsMeet)
│   ├── Files/config.example
│   └── Snippets/           # Code templates: Handler, View, Model, Helper
├── release/
│   └── MiniHtml.b4xlib     # Compiled library
├── LICENSE                 # MIT License
└── README.md
```

## Installation

1. Copy `release/MiniHtml.b4xlib` to your B4X additional libraries folder
2. Add the library reference in your B4J/B4A/B4i project

## Quick Start

```b4x
' Build a simple HTML page
Dim doc As MiniHtml
doc.Initialize("doctype")
doc.Append(GeneratePage)
Return doc.ToString

Sub GeneratePage As String
    Dim html1 As MiniHtml = MH.Html
    Dim head1 As MiniHtml = MH.Head.up(html1)
    Dim title1 As MiniHtml = MH.Title.up(head1)
    title1.text("Hello")
    Dim body1 As MiniHtml = MH.Body.up(html1)
    Dim div1 As MiniHtml = MH.Div.up(body1)
    div1.cls("container").text("Hello World!")
    Return html1.build
End Sub
```

## API Reference

### MH.bas — Tag Factories

| Method | Tag |
|--------|-----|
| `Html` | `<html lang="en">` |
| `Head` | `<head>` |
| `Body` | `<body>` |
| `Title` | `<title>` |
| `Div` | `<div>` |
| `Span` | `<span>` |
| `P` | `<p>` |
| `Anchor` | `<a>` |
| `Button` | `<button>` |
| `Input` | `<input>` |
| `Label` | `<label>` |
| `Form` | `<form>` |
| `Table` | `<table>` |
| `Tr` | `<tr>` |
| `Td` | `<td>` |
| `Th` | `<th>` |
| `Thead` | `<thead>` |
| `Tbody` | `<tbody>` |
| `Ul` | `<ul>` |
| `Li` | `<li>` |
| `SelectTag` | `<select>` |
| `Option` | `<option>` |
| `Textarea` | `<textarea>` |
| `Img` | `<img>` |
| `Meta` | `<meta>` |
| `Link` | `<link>` |
| `Script` | `<script>` |
| `Style` | `<style>` |
| `Strong` | `<strong>` |
| `Br` | `<br>` |
| `Nav` | `<nav>` |
| `Icon` | `<i>` |
| `Svg` | `<svg>` |
| `Path` | `<path>` |
| `Footer` | `<footer>` |
| `Caption` | `<caption>` |
| `H1`, `H2`, `H3`, `H5`, `H6` | Heading tags |

### MiniHtml — Key Methods

**Construction:**
- `Initialize(Name)` — Create a tag with auto-detected mode
- `build` — Render to string (no indent, no CRLF on first line)
- `build2(indent)` — Render with custom indent
- `buildImpl(indent, AlignAttributes)` — Full control

**Adding children:**
- `add(ChildTag)` — Add child, returns child
- `up(ParentTag)` — Add to parent, returns self (alias for `addTo`)
- `addTo(ParentTag)` — Add to parent, returns self
- `text(value)` — Add inner text
- `text2(value)` — Clear children and set inner text
- `textWrap(value)` — Add indented inner text
- `script(value)` — Add `<script>value</script>` child
- `linebreak` — Add a non-indented line break
- `comment(value)` — Add `<!-- value -->` with indent
- `comment2(value, newline)` — Add inline comment

**Attributes:**
- `attr(key, value)` — Set attribute
- `attr2(map)` — Set multiple attributes from map
- `attr3(key)` — Set boolean attribute (no value)

**Classes & Styles:**
- `cls(value)` or `addClass(value)` — Add class(es)
- `removeClass(value)` — Remove class
- `sty(value)` or `addStyle(value)` — Add style(s) (semicolon-separated)
- `removeStyle(key)` — Remove style

**Boolean attributes:**
- `required`, `disabled`, `checked`, `selected`, `hidden`, `readonly`

**CDN helpers:**
- `cdn(format, url)` — Add `<script src="...">` or `<link href="...">`
- `cdn2(format, url, integrity, crossorigin)` — With SRI
- `cdn3(format, url, keyvals)` — With extra attributes

**Traversal:**
- `ChildByName(value)` — Deep search by tag name
- `ChildById(value)` — Deep search by `id` attribute
- `ChildByIndex(index)` — Get child by position

**Parsing:**
- `Parse(HtmlText)` — Parse HTML string into MiniHtml
- `ConvertFromBytes(Buffer())` — Parse from byte array
- `ConvertToBytes` — Serialize to byte array
- `ConvertToMiniHtml(HtmlNode)` — Convert parser node to MiniHtml

**Configuration:**
- `Flat` — Suppress line breaks (minified output)
- `Indentation` — Enable/disable per-node indent
- `LineFeed` — Enable/disable CRLF
- `IndentString` — Indentation string (default: `"  "`)
- `FormatAttributes` — Align multi-line attributes
- `Mode` — `uniline`, `multiline`, `meta`, `self`, `notext`
- `SpecialTags` — Tags excluded from default indentation

### MiniHtmlParser — HTML Parser

The parser (credits to Erel) converts HTML strings into a tree of `HtmlNode` objects.

**Types:**
- `HtmlNode(Name, Children, Attributes, Closed, Parent)`
- `HtmlAttribute(Key, Value)`

**Key methods:**
- `Parse(HtmlText)` — Returns root HtmlNode
- `FindNode(Root, TagName, Attribute)` — Recursive node search
- `FindDirectNodes(Root, TagName, Attribute)` — Direct children search
- `IsNodeMatches(Node, TagName, Attribute)` — Match test
- `GetTextFromNode(Node, ChildIndex)` — Extract text
- `GetAttributeValue(Node, Key, Default)` — Get attribute value
- `UnescapeEntities(XmlInput)` — Unescape HTML entities
- `PrintNode(node)` — Debug output
- `CreateHtmlAttribute(Key, Value)` — Create attribute

## Dependencies

- [B4XCollections](https://www.b4x.com/android/forum/threads/b4x-b4xcollections.101115/)

## License

MIT License. See [LICENSE](LICENSE).

## Links

- [B4X Forum](https://www.b4x.com/android/forum/threads/b4x-web-minihtml2.170180/)
- [GitHub](https://github.com/pyhoon/MiniHtml2-B4X)
