# Introduction to SDMX-JSON Structure Message

See the SDMX-JSON Data Message docs for a brief introduction of the SDMX information model. For additional information on the SDMX information model, please refer to the [SDMX documentation](http://sdmx.org/?page_id=10).

Samples, tools and other SDMX-JSON resources are available in the public Github repository <https://github.com/sdmx-twg/sdmx-json>.

Before we start, let's clarify a few more things about this guide:

- New fields may be introduced in later versions. Therefore consuming applications should tolerate the addition of new fields with ease.
- The ordering of fields in objects is undefined. The fields may appear in any order and consuming applications should not rely on any specific ordering. It is safe to consider a nulled field and the absence of a field as the same thing.
- Not all fields appear in all contexts. For example response with error messages may not contain fields for data, dimensions and attributes.

# Field Guide to SDMX-JSON Structure Message Objects

## message

Message is the top level object and it contains the requested information ("data") as well as the meta-information decribing the (technical) context of the message and, possibly, error information.

* meta - *Object* *optional*. A *[meta](#meta)* object that contains non-standard meta-information and basic technical information about the message, such as when it was prepared and who has sent it.
* data - *Object* *optional*. *[Data](#data)* contains the message's “primary data”.
* errors - *Array* *optional*. *Errors* field is an array of *[error](#error)* objects. When appropriate provides a list of error messages in addition to RESTful web services HTTP error status codes.

The members data and errors MUST NOT coexist in the same message.

Example:

    {
      "meta": {
          # meta object #
      },
      "data": {
          # data object #
      },
      "errors": [
        {
          # error object #
        }
      ]
    }

## meta

*Object* *optional*. Used to include non-standard meta-information and basic technical information 
about the message, such as when it was prepared and who has sent it.
Any members MAY be specified within `meta` objects.
* schema - *String* *optional*. Contains the URL to the schema allowing to validate the message. This also allows identifying the version of SDMX-JSON format used in this message. **Providing the link to the SDMX-JSON schema is recommended.**
* id - *String*. Unique string that identifies the message for further references.
* test - *Boolean* *optional*. Indicates whether the message is for test purposes or not. False for normal messages.
* prepared - *String*. A timestamp indicating when the message was prepared. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* content-language - *Array* *optional*. Array of strings containing the identifyer of all languages used anywhere in the message for localized elements, and thus the languages of the intended audience, representaing in an array format the same information than the http Content-Language response header, e.g. "en, fr-fr". See IETF Language Tags: https://tools.ietf.org/html/rfc5646#section-2.1. The array's first element indicates the main language used in the message for localized elements. **The usage of this property is recommended.** Consult the section on [localised strings](#localised-strings) on how the message deals with languages.
* sender - *Object*. *[Sender](#sender)* contains information about the party that is transmitting the message.
* receiver - *Object* *optional*. *[Receiver](#receiver)* contains information about the party that is receiving the message. This can be useful if the WS requires authentication.
* links - *Array* *optional*. *Links* field is an array of *[link](#link)* objects. If appropriate, a collection of links to additional external resources for the header.

Example:

    "meta": {
      "schema": "https://raw.githubusercontent.com/sdmx-twg/sdmx-json/master/data-message/tools/schemas/sdmx-json-data-schema.json",
      "copyright": "Copyright 2017 Statistics hotline",
      "id": "b1804c51-1ee3-45a9-bb75-795cd4e06489",
      "prepared": "2018-01-03T12:54:12",
      "test": false,
      "content-language": [ "en", "fr-fr" ],
      "sender: {
        # sender object #
      },
      "receiver": {
        # receiver object #
      },
      "links": [
        {
          # link object #
        }
      ]
    }

### sender

*Object*. Information about the party that is transmitting the message. 
Sender contains the following fields:

* id - *String*. A unique identifier of the party.
* name - *Object* *optional*. A list of human-readable localised *[names](#name)* of the sender. See the section on [localised strings](#localised-strings) on how the message deals with languages.
* contacts - *Array* *optional*. A collection of *[contacts](#contact)*.

Example:

    "sender": {
      "id": "ECB",
      “name”: {
          # name object #
      },
      "contacts": [
        {
          # contact objects #
        }
      ]
    }

#### name

*Object* containing all returned localised names, one per object property:

* One or more of: IETF Language Tag according to [RFC 5646 documentation](https://tools.ietf.org/html/rfc5646#section-2.1) for specifying locals in HTTP - *String*. The localised name.

See the section on [localised strings](#localised-strings) on how the message deals with languages.

Example:

    { 
      "en": "This is an English name",
      "en-GB": "This is a British name",
      "fr": "C'est un nom français"
    }

#### contact

*Object*. A collection of contact details. Each object in the collection 
may contain the following field:

* name - *Object* *optional*. A list of human-readable localised *[names](#name)* of the contact.
* department - *Object* *optional*. A list of human-readable localised *[names](#name)* of the organisational structure for the contact.
* role - *String* *optional*. The localised name of the responsibility of the contact.
* telephone - *Array* *optional*. An array of telephone numbers for the contact.
* fax - *Array* *optional*. An array of fax numbers for the contact person.
* uri - *Array* *optional*. An array of uris. Each uri holds an information URL for the contact.
* email - *Array* *optional*. An array of email addresses for the contact person.

See the section on [localised strings](#localised-strings) on how the message deals with languages.

Example:

    {
      "name": { "en": "Statistics hotline" },
      "department": { "en": "Statistics hotline" },
      "role": "Statistics hotline",
      "telephone": [ "+00 0 00 00 00 00" ],
      "fax": [ "+00 0 00 00 00 01" ],
      "uri": [ "www.xyz.org" ],
      "email": [ "statistics@xyz.org" ]
    }

### receiver

*Object* *optional*. Information about the party that is receiving the message. 
This can be useful if the WS requires authentication. Receiver contains the 
same fields as [sender](#sender).

### link

See the section on [linking mechanism](#linking-mechanism) for all information on links.

## data

*Object* *optional*. Header contains the message's “primary data”.

* *[Artefact type]* - *Array* *optional*. This field is an array of objects of one of the corresponding SDMX Information Model artefact types: *dataStructure*, *metadataStructure*, *categoryScheme*, *conceptScheme*, *codelist*, *hierarchicalCodelist*, *organisationsScheme*, *agencyScheme*, *dataProviderScheme*, *dataConsumerScheme*, *organisationUnitScheme*, *dataflow*, *metadataflow*, *reportingTaxonomy*, *provisionAgreement*, *structureSet*, *process*, *categorisation* and *constraint*. Each of the corresponding object properties is allowed at maximum one time. Contains the requested structural information according to the definition of this artefact. For more information, please see:

    * *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*
    * *[Common property of SDMX artefacts of base type "ItemScheme"](#common-property-of-sdmx-artefacts-of-base-type-itemscheme)*

    * *[dataStructure](#datastructure)*
    * *[metadataStructure](#metadatastructure)*
    * *[categoryScheme](#categoryscheme)*
    * *[conceptScheme](#conceptscheme)*
    * *[codelist](#codelist)*
    * *[hierarchicalCodelist](#hierarchicalcodelist)*
    * *[organisationsScheme](#organisationsscheme)*
    * *[agencyScheme](#agencyscheme)*
    * *[dataProviderScheme](#dataproviderscheme)*
    * *[dataConsumerScheme](#dataconsumerscheme)*
    * *[organisationUnitScheme](#organisationunitscheme)*
    * *[dataflow](#dataflow)*
    * *[metadataflow](#metadataflow)*
    * *[reportingTaxonomy](#reportingtaxonomy)*
    * *[provisionAgreement](#provisionagreement)*
    * *[structureSet](#structureset)*
    * *[process](#process)*
    * *[categorisation](#categorisation)*
    * *[constraint](#constraint)*

Example:

    "data": {
      "dataStructureDefinitions": [
        {
          # dataStructureDefinition object #
        }
      ],
      "dataflows": [
        {
          # dataflow object #
        }
      ]
    }

### Common SDMX artefact properties

All SDMX artefact types share the following common object properties:

* id - *String*. Identifier for the resource.
* uri - *String* *optional*. The URL address of the resource.
* urn - *String* *optional*. URN - typically a URL - which points to an external resource which may contain or supplement the annotation. If a specific behavior is desired, an annotation type should be defined which specifies the use of this field more exactly.
* name - *Object* *optional*. A list of human-readable localised *[names](#name)* of the resource.
* description - *Object* *optional*. A list of human-readable localised descriptions (see *[names](#name)*) of the resource.
* agencyID - *String* *optional*. ID of the agency maintaining this resource.
* version - *String* *optional*. Version of this resource. It is "1.0" by default.
* validFrom - *String* *optional*. A timestamp from which the version is valid. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* validTo - *String* *optional*.  A timestamp from which the version is superceded. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* isFinal - *Boolean* *optional*. True if this is the final version of the resource, otherwise False (draft version).
* isExternalReference - *Boolean* *nullable*. If set to “true” it indicates that the content of the resource is held externally.
* links - *Array* *optional*. A collection of links to additional resources for the resource. See the section *[link](#link)*. **It is recommended to systematically include a self-referencing hyperlink (link with "rel"="self").**
* annotations - *Array* *optional*. Provides a list of annotation objects.

Example:

	{
	  "id": "MOBILE_NAVI_PUB",
	  "uri": "HTTP://www.xyz.org/resource/0123456789",
	  "urn": "urn:sdmx:org.sdmx.infomodel.datastructure.Dataflow=ECB.DISS:BSI_PUB(1.0)",
	  "name": { "en": "Economic concepts" },
	  "description": { "en": "This is the description of Economic concepts" },
	  "agencyID": "ECB.DISS",
	  "version": "1.0",
	  "validFrom": "2012-05-04",
	  "validTo": "2015-05-04",
	  "isFinal": true,
	  "isExternalReference": false,
	  "links": [
		{
		  # link object#
		}
	  ],
	  "annotations":[
		{
		  # annotation object#
		}
	  ]
	}

#### annotation

*Object* *optional*. Provides all information about an annotation. 

* id - *String* *optional*. ID provides a non-standard identification of an annotation. It can be used to disambiguate annotations.
* title - *String* *optional*. A title for the annotation.
* type - *String* *optional*. Type is used to distinguish between annotations designed to support various uses. The types are not enumerated, and these can be freely specified by the creator of the annotations. The definitions and use of annotation types should be documented by their creator.
* url - *String* *optional*. URI - typically a URL - which points to an external resource which may contain or supplement the annotation. If a specific behavior is desired, an annotation type should be defined which specifies the use of this field more exactly.
* text - *Object* *optional*. A list of human-readable localised texts (see *[names](#name)*) of the annotation.

Example:

	{
	  "id": "74747",
	  "title": "Sample annotation",
	  "type": "reference",
	  "url": "http://sample.org/annotations/74747",
	  "text": { "en": "Sample annotation text" }
	}

### Common property of SDMX artefacts of base type "ItemScheme"

All SDMX artefacts of base type "ItemScheme" (CategoryScheme, Codelist, ConceptScheme, ReportingTaxonomy, and OrganisationScheme) share the following common object property:

* items - *Array* *optional*. Provides a list of items if the resource inherits from the ItemScheme . 
* isPartial - *Boolean* *optional*. If set to true, it indicates that the resource contains only a sub-set of items.

Example:

	{
	  "id": "ITEMSCHEME_EXAMPLE",
	  "items": [
		{
		  # item object #
		}
	  ],
	  "isPartial": false
	}

#### item

*Object* *optional*. Item within the ItemScheme (if the resource is a CategoryScheme, Codelist, ConceptScheme, ReportingTaxonomy, or OrganisationScheme. 

* id - *String*. Identifier for the item.
* uri - *String* *optional*. The URL address of the item.
* urn - *String* *optional*. URN - typically a URL - which points to an external resource which may contain or supplement the annotation. If a specific behavior is desired, an annotation type should be defined which specifies the use of this field more exactly.
* name - *Object* *optional*. A list of human-readable localised *[names](#name)* of the item.
* description - *Object* *optional*. A list of human-readable localised description (see *[names](#name)*)  of the item.
* links - *Array* *optional*. A collection of links to additional resources for the item. See the section [link](#link).
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* items - *Array* *optional*. Provides a list of child items of the item.

Example:

	{
	  "id": "01",
	  "uri": "http://www.xyz.org/resource/0123456789",
	  "urn": "urn:sdmx:org.sdmx.infomodel.categoryscheme.Category=SDMX:SDMXStatSubMatDomainsWD1(1.0).1.1",
	  "name": { "en": "Population and migration" },
	  "description": { "en": "Description for Population and migration" },
	  "links":[
		{
		  # link object#
		}
	  ],
	  "annotations": [
		{
		  # annotation object #
		}
	  ],
	  "items": [	
		{
		  # item object #
		}
	  ]
	}

### dataStructureDefinition

...

### metadataStructureDefinition

...

### categoryScheme

...

### conceptScheme

...

### codelist

...

### hierarchicalCodelist

...

### organisationsScheme

...

### agencyScheme

...

### dataProviderScheme

...

### dataConsumerScheme

...

### organisationUnitScheme

...

### dataflow

...

### metadataflow

...

### reportingTaxonomy

...

### provisionAgreement

...

### structureSet

...

### process

...

### categorisation

...

### constraint

...

## error

*Object* *optional*. Used to provide a error message in addition to RESTful web services HTTP error status codes. The following pieces of information are to be provided:

* code - *Number*. Provides a code number for the error message. Code numbers are defined in the SDMX 2.1 Web Services Guidelines.
* title - *Object* *optional*. A list of short, human-readable localised summary (see *[names](#name)*) of the problem that SHOULD NOT change from occurrence to occurrence of the problem, except for purposes of localization.
* detail - *Object* *optional*. A list of human-readable localised explanations (see *[names](#name)*) specific to this occurrence of the problem. Like title, this field’s value can be localized. It is fully customizable by the service providers and should provide enough detail to ease understanding the reasons of the error.
* links - *Array* *optional*. *Links* field is an array of *[link](#link)* objects. If appropriate, a collection of links to additional external resources for the error.

See the section on [localised strings](#localised-strings) on how the message deals with languages.

Example:

    {
      "code": 150,
      "title": { "en": "Invalid number of items in the item parameter" }
    }

# Linking mechanism

## link

*Object* *optional*. A link to an external resource.

* href - *String*. Absolute or relative URL of the external resource.
* rel - *String*. Relationship of the object to the external resource. See semantics below.
* title - *Object* *optional*. A list of human-readable localised descriptions (see *[names](#name)*) of the target link. See the section on [localised strings](#localised-strings) on how the message deals with languages.
* type - *String* *optional*. A hint about the type of representation returned by the link.
* hreflang - *String* *optional*. The natural language of the external link, the same as used in the HTTP Accept-Language request header.

Examples:

    {
      "href": "https://registry.sdmx.org/ws/rest/dataflow/ECB/EXR",
      "rel": "dataflow"
    }
    
    {
      "href": "https://registry.sdmx.org/ws/rest/datastructure/ECB/ECB_EXR1",
      "rel": "datastructure"
    }
    
    {
       "href": "https://registry.sdmx.org/FusionRegistry/ws/rest/provisionagreement/ESTAT/PA_NAMAIN_IDC_N",
       "rel": "provisionagreement"
    }

    {
      "href": "https://registry.sdmx.org/help.html",
      "rel": "help",
      "title": { "en": "Documentation about the SDMX Global Registry" },
      "type": "text/html",
      "hreflang": "en"
    }

Collections of links can be attached to various elements in SDMX-JSON.

Similarily with standards such as HTML5 and Atom, link elements in SDMX-JSON *must* define a *URL* (the `href` attribute) and a *semantic* (the `rel` attribute). This allows clients to follow the links they care about and ignore the ones whose semantic they are not interested in. In addition, links in SDMX-JSON *may* define a `title` (a human-friendly description of the target link) and a `type` (a hint about the type of representation returned by the link). Please refer to the [list of Media Types and Subtypes](http://www.iana.org/assignments/media-types/media-types.xhtml) assigned and listed by the IANA for additional information about expected values for the `type` attribute.

SDMX-JSON offers a list of predefined semantics, but implementers are free to extend it. The list of predefined semantics comes from the list of SDMX artefacts that can be returned by SDMX RESTful web services, semantics defined in [RFC5988](https://tools.ietf.org/rfc/rfc5988.txt) and additional items deemed to be useful in the context of statistical data dissemination. These semantics are:

  - SDMX artefacts: dataStructure, metadataStructure, categoryScheme, conceptScheme, codelist, hierarchicalCodelist, organisationScheme, agencyScheme, dataProviderScheme, dataConsumerScheme, organisationUnitScheme, dataflow, metadataflow, reportingTaxonomy, provisionAgreement, structureSet, process, categorisation, contentconstraint, attachmentconstraint, category, concept, code, organisation, agency, dataProvider, dataConsumer, organisationUnit, reportingCategory, Data
  - RFC5988: alternate, copyright, glossary, help, index.
  - Miscellaneous: calendar (link to a release calendar), source (information about the source of data), request (the SDMX RESTful query that triggered the SDMX-JSON response).

The *URL* captured in the `href` attribute can be *absolute* or *relative*. **It is recommended to use absolute URLs in case the the SDMX-JSON message is archived.**

# Localised strings

At least all language matches according to the user’s preferred language choices in the http Accept-Language header (or if that is not available than according to the system's default languages) are to be provided for each localisable message element.
In case that there is no such language match for a particular localisable element, it is optional to:

- return the element in the system-default languages or alternatively to not return the element
- indicate available alternative languages for the element's maintainable artefact through links to underlying localised resources

**It is recommended to indicate all languages used anywhere in the message for localised elements through http Content-Language response header (languages of the intended audience) and/or through a “content-language” property in the meta tag.** The main language used can be indicated through the “lang” property in the meta tag.


# Security Considerations

This document defines a response format for SDMX RESTful Web Services in JSON
and it raises no new security considerations. SDMX Web Services Guidelines
includes the security considerations associated with its usage.


# Extending SDMX-JSON

The objects defined in SDMX-JSON are "open", i.e. they can be extended with properties not defined in this specification. Providers of SDMX-JSON messages are therefore welcome to add support for features not covered in this specification. Whenever appropriate, providers who opt to do so are invited to inform us, so that future versions of SDMX-JSON may integrate these extensions, thereby improving interoperability.

The snippet below shows an example of an `error` object, extended with a `wsCustomErrorCode`:

	```
	"errors": [
	  {
		"code": 150,
		"title": "Invalid number of items in the item parameter"
		"wsCustomErrorCode": 39272
	  }
	]
	```
