Peritext spec
===

Peritext spec is a technical overview of the state of the project.

# Document model

```
sections // defines all the sections contents
    [uuid]
        metadata
            general
                [key]
                    value: [value]
                    inheritedfrom: [lateral propagation]
            dublincore
            ...
        contents // the content represented as draft's contentRaw or in the peritext html-to-js system ?
        notes
            [index]

resources // dict of resources used along the document
    [uuid]

contextualizations // dict of contextualizations used along the document
    [uuid]

contextualizers // dict of contextualizers used along the document
    [uuid]

customizers // customizations to apply if rendering/exporting the document
    styles
        page.css
        web.css
        global.css

metadata
    general
        [key]
            value: [value]

summary // the linear order of elements to display
    [index]
        type : [type] // section, table of contents, table of figures
        sectionId: [uuid]
        level: [level] // level of hierarchy within the summary
        role: [role]
        class: [class] // css class to append to the section title

settings // other stuff that could not be put there
```

# `peritext-core`

Peritext core provides :

* models data attached to what is a peritext document
* validators functions to consume with models
* getters for a peritext document
* setters for a peritext document

# Peritext connectors

Specification : a peritext connector allows to connect a specific flatfile source of contents to the representation of a peritext document.

peritext flatfile representation <-> peritext document representation

It *must* expose the following :

* read the source
* write into the source

## `peritext-connector-fs`

Connects to the FS system via an API.

# `peritext-converter-flatfile`

Converts a `peritextFlatfile` representation to a `peritextDocument`, and vice versa

# `peritext-controller`

A peritext controller handles updateFromSource and updateToSource operations - it updates a peritext document from a connector, or a peritext document to a connector.

# Peritext contextualizers

Peritext contextualizers provide two types of subcomponents :

* *peritext documents transformers* that consume a document and a contextualization to produce an updated document - they expose to this extent four functions : `contextualizeInlineStatic`, `contextualizeBlockStatic`, `contextualizeInlineDynamic`, `contextualizeBlockDynamic`
* *react components* that correspond to the component needed for displaying them

They all must be provided with a *storybook* showing what they look like.

## `peritext-contextualizer-citation`

Handles csl-json conversions to generate inline and block citations in html/react.

`contextualizeInlineStatic` : short citation

`contextualizeBlockStatic` : long citation

`contextualizeInlineDynamic` : short citation with click callback

`contextualizeBlockDynamic` : long citation with click callback

Dependencies :

* `citationstyles` : https://www.npmjs.com/package/citationstyles (csl styles)
* `citation-js`

## `peritext-contextualizer-glossary`

`contextualizeInlineStatic` : marker for glossary

`contextualizeBlockStatic` : glossary summary

`contextualizeInlineDynamic` : marker for glossary with click callback

`contextualizeBlockDynamic` : glossary summary in an interactive way

## `peritext-contextualizer-image-gallery`

`contextualizeInlineStatic` : adds a gallery later on

`contextualizeBlockStatic` : adds a gallery

`contextualizeInlineDynamic` : adds a dynamic gallery inline ref

`contextualizeBlockDynamic` : adds a dynamic gallery block ref

## `peritext-contextualizer-table`

`contextualizeInlineStatic` : adds a table figure later on

`contextualizeBlockStatic` : adds a table figure

`contextualizeInlineDynamic` : adds a ref to a dynamic table

`contextualizeBlockDynamic` : adds a dynamic table

## `peritext-contextualizer-timeline`

`contextualizeInlineStatic` : adds a timeline figure later on

`contextualizeBlockStatic` : adds a timeline figure

`contextualizeInlineDynamic` : interactive timeline figure inline ref

`contextualizeBlockDynamic` : interactive timeline figure block ref

## `peritext-contextualizer-webpage`

Contextualization of the webpage of a resource.

`contextualizeInlineStatic` : displays the address

`contextualizeBlockStatic` : /

`contextualizeInlineDynamic` : hyperlink

`contextualizeBlockDynamic` : iframe of the webpage


# Peritext renderers

## `peritext-renderer-html`

Renders an html webpage.

# Peritext exporters

They all take a `peritextDocument` and some parameters as input, and output a file.

## `peritext-exporter-pdf`

Produces a pdf file built with prince xml.

Dependencies :

* `peritext-renderer-static-html`
* `prince`

## `peritext-exporter-epub`

Dependencies :

* `peritext-core`

## `peritext-exporter-static-website`

Produces an isomorphic website made with react/redux + static pages. Writes a series of files or a zip archive as output.

