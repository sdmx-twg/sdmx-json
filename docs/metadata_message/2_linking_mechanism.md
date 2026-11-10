# Linking mechanism

## link

*Object* *optional*. A link to an external resource.

- href - *String* *optional* only if `urn` is present. Absolute or relative URL 
  of the external resource.
- rel - *String*. Relationship of the object to the external resource. See
  semantics below.
- urn - *String* *optional* only if `href` is present. The `urn` holds any
  valid SDMX Registry URN (see SDMX Registry Specification for details) or any
  other URN.
- uri - *String* *optional*. Deprecated since SDMX 3.2. The `uri` attribute holds a URI that contains a
  link to additional information about the resource, such as a web page. This
  `uri` is not an SDMX resource.
- title - *String* *optional*. A human-readable (best-language-match)
  description of the target link.
- titles - *Object* *optional*. A list of human-readable localised descriptions
  (see *[names](./1_field_guide.md#names)*) of the target link. A title must be unique for a given locale.
- type - *String* *optional*. A hint about the media type of the representation
  returned by the link.
- locale - *String* *optional*. The natural language of the linked resource,
  based on the IETF BCP 47 language tag.
- hreflang - *String* *optional*. Deprecated since SDMX 3.2, use `locale`
  instead. The natural language of the external link, the same as used in the
  HTTP Accept-Language request header.

See the section on [localised text elements](./4_localised_text_elements.md) on how
the message deals with languages.

??? example

    ```json
    {
      "href": "https://registry.sdmx.org/ws/rest/datastructure/ECB/ECB_EXR1",
      "rel": "sdmx:dataStructure"
    }

    {
      "href": "https://registry.sdmx.org/ws/rest/dataflow/ECB.DISS/BSI_PUB/1.0",
      "rel": "sdmx:dataflow",
      "urn": "urn:sdmx:org.sdmx.infomodel.datastructure.dataflow=ECB.DISS:BSI_PUB(1.0)"
    }

    {
      "href": "https://registry.sdmx.org/FusionRegistry/ws/rest/provisionagreement/ESTAT/PA_NAMAIN_IDC_N",
      "rel": "sdmx:provisionAgreement"
    }

    {
      "href": "https://registry.sdmx.org/help.html",
      "rel": "rfc:help",
      "title": "Documentation about the SDMX Global Registry",
      "titles": { "en": "Documentation about the SDMX Global Registry" },
      "type": "text/html",
      "locale": "en"
    }
    ```

Collections of links can be attached to various elements in SDMX-JSON.

Similarly with standards such as HTML5 and Atom, link elements in SDMX-JSON
*must* define a *URL* (the `href` attribute) or a *URN* (the `urn` attribute),
and a *semantic* (the `rel` attribute). This allows clients to follow the links they care about and ignore
the ones whose semantic they are not interested in. In addition, links in
SDMX-JSON *may* define a `title` (a human-friendly description of the target link, unique
for a given locale), a `type` (a hint about the media type of the representation
returned by the link) and a `locale` (the natural language of the linked
resource). Please refer to the
[list of Media Types and Subtypes](http://www.iana.org/assignments/media-types/media-types.xhtml)
assigned and listed by the IANA for additional information about expected values
for the `type` attribute.

SDMX-JSON offers a list of predefined semantics, but implementers are free to
extend it. Each semantic is prefixed with the source of its definition (`sdmx`,
`rfc`, `misc`, `skos`, `xkos`, or `x` for custom extensions). The predefined
semantics are:

- SDMX artefacts (`sdmx`): the SDMX artefact types, e.g. sdmx:dataStructure,
  sdmx:metadataStructure, sdmx:codelist, sdmx:dataflow, sdmx:provisionAgreement.
- [RFC 8288](https://www.rfc-editor.org/rfc/rfc8288) (`rfc`): e.g.
  rfc:alternate, rfc:copyright, rfc:glossary, rfc:help, rfc:index.
- Miscellaneous (`misc`): misc:calendar (link to a release calendar),
  misc:source (information about the source of data), misc:request (the SDMX
  RESTful query that triggered the SDMX-JSON response).
- [SKOS relationships](https://www.w3.org/TR/skos-reference/#vocabrfc)
  (`skos`): e.g. skos:definition, skos:exactMatch.
- [XKOS relationships](https://rdf-vocabulary.ddialliance.org/xkos.html#nsvoc)
  (`xkos`): e.g. xkos:generalizes, xkos:precedes.
- Custom extensions (`x`): e.g. x:catalog.

The relationship `rfc:self` should not be used for SDMX artefacts, as this is
already expressed in their `urn`. Unprefixed semantics (e.g. `dataflow`,
`help`, `self`) are deprecated since SDMX 3.2.

The *URL* captured in the `href` attribute can be *absolute* or *relative*. **It
is recommended to use absolute URLs in case the SDMX-JSON message is archived.**
