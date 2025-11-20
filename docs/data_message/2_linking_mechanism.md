# Linking mechanism

## link

_Object_ _optional_. A link to an external resource.

- href - _String_ or . Absolute or relative URL of the external resource.
- rel - _String_. Relationship of the object to the external resource. See
  semantics below.
- urn - _String_ _optional_. The urn holds a valid SDMX Registry URN (see SDMX
  Registry Specification for details).
- uri - _String_ _optional_. The uri attribute holds a URI that contains a link
  to additional information about the resource, such as a web page. This uri is
  not an SDMX resource.
- title - _String_ _optional_. A human-readable (best-language-match)
  description of the target link.
- titles - _Object_ _optional_. A list of human-readable localised descriptions
  (see _[names](./1_field_guide.md#names)_) of the target link.
- type - _String_ _optional_. A hint about the type of representation returned
  by the link.
- hreflang - _String_ _optional_. The natural language of the external link, the
  same as used in the HTTP Accept-Language request header.

See the section on [localised text elements](./4_localised_text_elements.md) on
how the message deals with languages.

??? example

    ```json
    {
        "href": "https://registry.sdmx.org/ws/rest/datastructure/ECB/ECB_EXR1",
        "rel": "self",
        "uri": "http://www.xyz.org/pdf/0123456789"
    }

    {
        "href": "https://registry.sdmx.org/ws/rest/dataflow/ECB.DISS/BSI_PUB/1.0",
        "rel": "dataflow",
        "urn": "urn:sdmx:org.sdmx.infomodel.datastructure.dataflow=ECB.DISS:BSI_PUB(1.0)"
    }

    {
        "href": "https://registry.sdmx.org/FusionRegistry/ws/rest/provisionagreement/ESTAT/PA_NAMAIN_IDC_N",
        "rel": "provisionagreement"
    }

    {
        "href": "https://registry.sdmx.org/help.html",
        "rel": "help",
        "title": "Documentation about the SDMX Global Registry",
        "titles": { "en": "Documentation about the SDMX Global Registry" },
        "type": "text/html",
        "hreflang": "en"
    }
    ```

Collections of links can be attached to various elements in SDMX-JSON.

Similarly with standards such as HTML5 and Atom, link elements in SDMX-JSON
_must_ define a _URL_ (the `href` attribute) and a _semantic_ (the `rel`
attribute). This allows clients to understand the semantics of links, in order
to follow those that are of interest to them. In addition, links in SDMX-JSON
_may_ define a `title` (a human-friendly description of the target link) and a
`type` (a hint about the type of representation returned by the link). Please
refer to the
[list of Media Types and Subtypes](http://www.iana.org/assignments/media-types/media-types.xhtml)
assigned and listed by the IANA for additional information about expected values
for the `type` attribute.

SDMX-JSON offers a list of predefined semantics, but implementers are free to
extend it. The list of predefined semantics comes from the list of SDMX
artefacts that can be returned by SDMX RESTful web services, semantics defined
in [RFC5988](https://tools.ietf.org/rfc/rfc5988.txt) and additional items deemed
to be useful in the context of statistical data dissemination. These semantics
are:

- SDMX artefacts: dataStructure, metadataStructure, categoryScheme,
  conceptScheme, codelist, hierarchicalCodelist, organisationScheme,
  agencyScheme, dataProviderScheme, dataConsumerScheme, organisationUnitScheme,
  dataflow, metadataflow, reportingTaxonomy, provisionAgreement, structureSet,
  process, categorisation, contentconstraint, attachmentconstraint, category,
  concept, code, organisation, agency, dataProvider, dataConsumer,
  organisationUnit, reportingCategory, Data
- RFC5988: alternate, copyright, glossary, help, index.
- Miscellaneous: calendar (link to a release calendar), source (information
  about the source of data), request (the SDMX RESTful query that triggered the
  SDMX-JSON response).

The _URL_ captured in the `href` attribute can be _absolute_ or _relative_. **It
is recommended to use absolute URLs in case the SDMX-JSON message is archived.**
