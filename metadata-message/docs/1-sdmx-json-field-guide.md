# Introduction to SDMX-JSON Metadata Message

See the SDMX-JSON Data Message docs for a brief introduction of the SDMX information model. For additional information on the SDMX information model, please refer to the [SDMX documentation](http://sdmx.org/?page_id=10).

Samples, tools and other SDMX-JSON resources are available in the public Github repository <https://github.com/sdmx-twg/sdmx-json>.

Before we start, let's clarify a few more things about this guide:

- New fields may be introduced in later versions. Therefore consuming applications should tolerate the addition of new fields with ease.
- The ordering of properties in objects is undefined. The properties may appear in any order and consuming applications should not rely on any specific ordering. It is safe to consider a nulled property and the absence of a property as the same thing.
- Not all properties appear in all contexts. For example response with error status messages may not contain a data property.

# Field Guide to SDMX-JSON 2.0 Metadata Message Objects (aligned with SDMX 3.0.0)

## message

Message is the top level object and it contains the requested information (referential metadata) as well as the meta-information decribing the (technical) context of the message and, possibly, status information.

* meta - *Object* *optional*. A *[meta](#meta)* object that contains non-standard meta-information and basic technical information about the message, such as when it was prepared and who has sent it.
* data - *Object* *optional*. *[Data](#data)* contains the message's “primary data”.
* errors - *Array* *optional*. *Errors* field is an array of *[statusMessage](#statusMessage)* objects. When appropriate provides a list of status messages in addition to RESTful web services HTTP error status codes.

The members data and status CAN coexist in the same message.

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
				# statusMessage object #
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
* contentLanguages - *Array* *optional*. Array of strings containing the identifyer of all languages used anywhere in the message for localized elements, and thus the languages of the intended audience, representaing in an array format the same information than the http Content-Language response header, e.g. "en, fr-fr". See IETF Language Tags: https://tools.ietf.org/html/rfc5646#section-2.1. The array's first element indicates the main language used in the message for localized elements. **The usage of this property is recommended.**
* name - *String* *optional*. Human-readable (best-language-match) name for the transmission.
* names - *Object* *optional*. Human-readable localised *[names](#names)* for the transmission.
* sender - *Object*. *[Sender](#sender)* contains information about the party that is transmitting the message.
* receivers - *Array* *optional* of *[Receiver](#receiver)* objects thats contain information about the party that is receiving the message. This can be useful if the WS requires authentication.
* links - *Array* *optional*. *Links* field is an array of *[link](#link)* objects. If appropriate, a collection of links to additional external resources for the header.

See the section on [localised text elements](#localised-text-elements) on how the message deals with languages.

Example:

	"meta": {
		"schema": "https://raw.githubusercontent.com/sdmx-twg/sdmx-json/master/metadata-message/tools/schemas/sdmx-json-metadata-schema.json",
		"copyright": "Copyright 2017 Statistics hotline",
		"id": "b1804c51-1ee3-45a9-bb75-795cd4e06489",
		"prepared": "2018-01-03T12:54:12",
		"test": false,
		"contentLanguages": [ "en", "fr-fr" ],
		"name": "Transmission name",
		"names": {
			# name object #
		},
		"sender": {
			# sender object #
		},
		"receivers": [
			{
				# receiver object #
			}
		],
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
* name - *String* *nullable*. A human-readable (best-language-match) name of the sender.
* names - *Object* *optional*. A list of human-readable localised *[names](#names)* of the sender.
* contacts - *Array* *optional*. A collection of *[contacts](#contact)*. Provides contact information for the party in regard to the transmission of the message.

See the section on [localised text elements](#localised-text-elements) on how the message deals with languages.

Example:

	"sender": {
		"id": "ECB",
		"name": "European Central Bank",
		"names": {
			"en": "European Central Bank",
			"fr": "Banque Centrale Européenne"
		},
		"contacts": [
			{
				# contact objects #
			}
		]
	}

#### names

*Object* containing all appropriate localised names, one per object property:

* One or more of: IETF Language Tag according to [RFC 5646 documentation](https://tools.ietf.org/html/rfc5646#section-2.1) for specifying locals in HTTP - *String*. The localised name.

See the section on [localised text elements](#localised-text-elements) on how the message deals with languages.

Example:

	{ 
		"en": "This is an English name",
		"en-GB": "This is a British name",
		"fr": "C'est un nom français"
	}

#### contact

*Object*. A collection of contact details. Each object in the collection 
may contain the following field:

* id - *String*. Identifier for the resource.
* name - *String* *optional*. Human-readable (best-language-match) name of the contact.
* names - *Object* *optional*. Human-readable localised *[names](#names)* of the contact.
* department - *String* *optional*. Human-readable (best-language-match) name of the organisational structure for the contact.
* departments - *Object* *optional*. Human-readable localised *[names](#names)* of the organisational structure for the contact.
* role - *String* *optional*. Human-readable (best-language-match) name of the responsibility of the contact.
* roles - *Object* *optional*. Human-readable localised *[names](#names)* of the responsibility of the contact.
* telephones - *Array* *optional*. An array of telephone numbers for the contact.
* faxes - *Array* *optional*. An array of fax numbers for the contact person.
* uris - *Array* *optional*. An array of uris. Each uri holds an information URL for the contact.
* emails - *Array* *optional*. An array of email addresses for the contact person.
* x400s - *Array* *optional*. An array of X.400 addresses for the contact person.

See the section on [localised strings](#localised-strings) on how the message deals with languages.

Example:

	{
		"id": "HOTLINE",
		"name": "Statistics hotline",
		"names": {
			"en": "Statistics hotline",
			"fr": "Service d'assistance téléphonique des Statistiques"
		},
		"department": "Statistics hotline",
		"departments": {
			"en": "Statistics hotline",
			"fr": "Service d'assistance téléphonique des Statistiques"
		},
		"role": "Statistics hotline",
		"roles": {
			"en": "Statistics hotline",
			"fr": "Service d'assistance téléphonique des Statistiques"
		},
		"telephones": [ "+00 0 00 00 00 00" ],
		"faxes": [ "+00 0 00 00 00 01" ],
		"uris": [ "www.xyz.org" ],
		"emails": [ "statistics@xyz.org" ]
	}

### receiver

*Object* *optional*. Information about the party that is receiving the message. 
This can be useful if the WS requires authentication. Receiver contains the 
same fields as *[sender](#sender)*.

### link

See the section on *[linking mechanism](#linking-mechanism)* for all information on links.

## data

*Object* *optional*. Header contains the message's “primary data”.

* metadataSets - *Array* *optional*. This field is an array of *[metadataSet](#metadataSet)* objects. A metadata set contains a collection of reported metadata against a set of values for a given full or partial target identifier, as described in a metadata structure definition. The metadata set may contain reported metadata for multiple report structures defined in a metadata structure definition.

Example:

	"data": {
		"metadataSets": [
			{
				# metadataSet object #
			}
		]
	}

### metadataSet

*Object*. Contains a collection of reported metadata against a set of values for a given full or partial target identifier, as described in a metadata structure definition. The metadata set may contain reported metadata for multiple report structures defined in a metadata structure definition.

* publicationPeriod - *String* *optional*. The publicationPeriod specifies the period of publication of the data in terms of whatever provisioning agreements might be in force (i.e., "2005-Q1" if that is the time of publication for a `metadataSet` published on a quarterly basis).
* publicationYear - *String* *optional*. The publicationYear holds the ISO 8601 four-digit year.
* reportingBegin - *String* *optional*. The start of the time period covered by the message.
* reportingEnd - *String* *optional*. The end of the time period covered by the message.
* id - *String* *optional*. Identifier for the metadata set.
* metadataflowRef - *String*. URN reference to the metadataflow definition (either *metadataflowRef* or *metadataProvisionRef* is required).
* metadataProvisionRef - *String*. URN reference to the metadata provision definition (either *metadataflowRef* or *metadataProvisionRef* is required).
* validFrom - *String* *optional*. The validFrom indicates the inclusive start time indicating the validity of the information in the data.
* validTo - *String* *optional*. The validTo indicates the inclusive end time indicating the validity of the information in the data.
* annotations - *Array* *optional*. *[Annotations](#annotation)* is a collection of indices of the corresponding *annotations* for the metadata set.
* links - *Array* *optional*. *Links* field is an array of *[link](#link)* objects. If appropriate, a collection of links to additional information regarding the metadata set.
* name - *String* *optional*. Human-readable (best-language-match) name of the metadata set.
* names - *Object* *optional*. Human-readable localised *[names](#names)* of the metadata set.
* metadataProvider - *String* *optional*. MetadataProvider provides a references to an organisation with the role of metadata provider that is providing this metadata set. DataProvider is a URN reference to a data provider to which the constraint is attached. If this is used, then only the release calendar is relevant. 
* target - *String*. *[Target](#target)* holds a valid SDMX Registry URN (see SDMX Registry Specification for details) of the object to which the reported metadata apply.
* attributes - Non-empty *array* of recursive *[attribute](#attribute)* objects. Contains the reported metadata attribute values for the reported metadata and recursively their child metadata attributes.

See the section on [localised text elements](#localised-text-elements) on how the message deals with languages.

Example:

	{
		"publicationPeriod": "2018-Q1",
		"publicationYear": "2018",
		"reportingBegin": "1960",
		"reportingEnd": "2020",
		"id": "METADATASET",
		"metadataflowRef": "urn:sdmx:org.sdmx.infomodel.metadatastructure.Metadataflow=IMF:IMFSTAT(1.0)",
		"metadataProvisionRef": "urn:sdmx:org.sdmx.infomodel.metadataprovision=OECD:ALB_DQAF(1.0.0)",
		"validFrom": "2018-04-01",
		"validTo": "2018-07-01",
		"annotations": [
			{
				# annotation object #
			}
		],
		"links": [
			{
				# link object #
			}
		],
		"name": "Metadata set",
		"names": {
			"en": "Metadata set",
			"fr": "Set de métadonnées"
		},
		"metadataProvider": "urn:sdmx:org.sdmx.infomodel.dataproviderscheme.DataproviderScheme=IMF:IMF(1.0).IMF_DISS",
		"target": "urn:sdmx:org.sdmx.infomodel.datastructure.DataStructure=ECB:ECB_EXR1(1.0)",
		"attributes": [
			{
				# attribute object #
			}
		]
	}

#### attribute

*Object*. Defines the structure for a reported metadata attribute. A value for the attribute can be supplied as either a single value, or multi-lingual text values (either structured or unstructured). An optional set of child metadata attributes is also available if the metadata attribute definition defines nested metadata attributes.

* id - *String*. ID for the reported metadata attribute.
* value - *String* *optional*. Value for the reported metadata attribute.
* annotations - *Array* *optional*. *[Annotations](#annotation)* is a collection of indices of the corresponding *annotations* for the reported metadata attribute.
* structuredText - *String* *optional*. A human-readable (best-language-match) structured text allowing for mixed content of text and XHTML tags. When using this type, one will have to provide a reference to the XHTML schema, since the processing of the tags within this type is strict, meaning that they are validated against the XHTML schema provided.
* structuredTexts - *Object* *optional*. A list of human-readable localised structured text (see *[names](#names)*), see above.
* text - *String* *optional*. A human-readable (best-language-match) unstructured text (without XHTML tags).
* texts - *Object* *optional*. A list of human-readable localised unstructured text (see *[names](#names)*), see above.
* attributes - Non-empty *array* of recursive *[attribute](#attribute)* objects. Contains the reported metadata attribute values for the reported metadata and recursively their child metadata attributes.

See the section on [localised text elements](#localised-text-elements) on how the message deals with languages.

Example:

	{
		"id": "REPORT_ATTRIBUTE1",
		"value": "CODED_TEXT",
		"annotations": [
			{
				# annotation object #
			}
		],
		"structuredText": "<p>An XHTML text</p>",
		"structuredTexts": {
			"en": "<p>An XHTML text</p>"
		},
		"text": "A text",
		"texts": {
			"en": "A text"
		},
		"attributes": [
			{
				# attribute object #
			}
		]
	}

#### annotation

*Object* *optional*. Provides all information about an annotation. 

* id - *String* *optional*. ID provides a non-standard identification of an annotation. It can be used to disambiguate annotations.
* title - *String* *optional*. Provides a non-localised title for the annotation.
* type - *String* *optional*. Type is used to distinguish between annotations designed to support various uses. The types are not enumerated, and these can be freely specified by the creator of the annotations. The definitions and use of annotation types should be documented by their creator.
* value - *String* *optional*. Provides a non-localised value for the annotation.
* text - *String* *optional*. A human-readable (best-language-match) text of the annotation.
* texts - *Object* *optional*. A list of human-readable localised texts (see *[names](#names)*) of the annotation.
* links - *Array* *optional*. *Links* field is an array of *[link](#link)* objects. If appropriate, a link to an additional external resource which may contain or supplement the annotation.

See the section on [localised text elements](#localised-text-elements) on how the message deals with languages.

Example:

	{
		"id": "74747",
		"title": "Sample annotation",
		"type": "reference",
		"value": "Sample annotation value",
		"text": "Sample annotation text",
		"texts": {
			"en": "Sample annotation text",
			"fr": "Exemple de texte d'annotation"
		},
		"links": [
			{
				# link object #
			}
		]
	}

#### link

See the section on [linking mechanism](#linking-mechanism) for all information on links.
Providing links allowing accessing the underlying SDMX Data Structure Definition, Dataflow
and/or Provision Agreements is recommended.

## statusMessage

*Object* *optional*. Used to provide status messages in addition to RESTful web services HTTP error status codes. The following pieces of information should be provided:

* code - *Number*. Provides a code number for the status message if appropriate. Standard code numbers are defined in the SDMX 2.1 Web Services Guidelines.
* title - *String* *optional*. A short, human-readable (best-language-match) summary of the situation that SHOULD NOT change from occurrence to occurrence of the status, except for purposes of localization.
* titles - *Object* *optional*. A list of short, human-readable localised summaries (see *[names](#names)*) of the status that SHOULD NOT change from occurrence to occurrence of the status, except for purposes of localization.
* detail - *String* *optional*. A human-readable (best-language-match) explanation specific to this occurrence of the status. Like title, this field’s value can be localized. It is fully customizable by the service providers and should provide enough detail to ease understanding the reasons of the status.
* details - *Object* *optional*. A list of human-readable localised explanations (see *[names](#names)*) specific to this occurrence of the status. Like titles, this field’s value can be localized. It is fully customizable by the service providers and should provide enough detail to ease understanding the reasons of the status.
* links - *Array* *optional*. *Links* field is an array of *[link](#link)* objects. If appropriate, a collection of links to additional external resources for the status message.

See the section on [localised strings](#localised-strings) on how the message deals with languages.

Example:

	{
		"code": 150,
		"title": "Invalid parameter",
		"titles": {
			"en": "Invalid parameter",
			"fr": "Paramètre invalide"
		}
	}

# Linking mechanism

## link

*Object* *optional*. A link to an external resource.

* href - *String*. Absolute or relative URL of the external resource.
* rel - *String*. Relationship of the object to the external resource. See semantics below.
* urn - *String* *optional*. The urn holds a valid SDMX Registry URN (see SDMX Registry Specification for details).
* uri - *String* *optional*. The uri attribute holds a URI that contains a link to additional information about the resource, such as a web page. This uri is not an SDMX resource.
* title - *String* *optional*. A human-readable (best-language-match) description of the target link.
* titles - *Object* *optional*. A list of human-readable localised descriptions (see *[names](#names)*) of the target link.
* type - *String* *optional*. A hint about the type of representation returned by the link.
* hreflang - *String* *optional*. The natural language of the external link, the same as used in the HTTP Accept-Language request header.

See the section on [localised text elements](#localised-text-elements) on how the message deals with languages.

Examples:

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

Collections of links can be attached to various elements in SDMX-JSON.

Similarily with standards such as HTML5 and Atom, link elements in SDMX-JSON *must* define a *URL* (the `href` attribute) and a *semantic* (the `rel` attribute). This allows clients to follow the links they care about and ignore the ones whose semantic they are not interested in. In addition, links in SDMX-JSON *may* define a `title` (a human-friendly description of the target link) and a `type` (a hint about the type of representation returned by the link). Please refer to the [list of Media Types and Subtypes](http://www.iana.org/assignments/media-types/media-types.xhtml) assigned and listed by the IANA for additional information about expected values for the `type` attribute.

SDMX-JSON offers a list of predefined semantics, but implementers are free to extend it. The list of predefined semantics comes from the list of SDMX artefacts that can be returned by SDMX RESTful web services, semantics defined in [RFC5988](https://tools.ietf.org/rfc/rfc5988.txt) and additional items deemed to be useful in the context of statistical data dissemination. These semantics are:

  - SDMX artefacts: dataStructure, metadataStructure, categoryScheme, conceptScheme, codelist, hierarchicalCodelist, organisationScheme, agencyScheme, dataProviderScheme, dataConsumerScheme, organisationUnitScheme, dataflow, metadataflow, reportingTaxonomy, provisionAgreement, structureSet, process, categorisation, contentconstraint, attachmentconstraint, category, concept, code, organisation, agency, dataProvider, dataConsumer, organisationUnit, reportingCategory, Data
  - RFC5988: alternate, copyright, glossary, help, index.
  - Miscellaneous: calendar (link to a release calendar), source (information about the source of data), request (the SDMX RESTful query that triggered the SDMX-JSON response).

The *URL* captured in the `href` attribute can be *absolute* or *relative*. **It is recommended to use absolute URLs in case the SDMX-JSON message is archived.**

# Localised text elements

**Localised best-language-match text strings (static properties matched through "Lookup"):**

The first best language match according to the user’s preferred language choices expressed through the HTTP content negotiation (Accept-Language header parameter) is to be provided for each localised text element. The message does however not indicate the returned language per localised text element.

This language matching type is called "Lookup", see <https://tools.ietf.org/html/rfc4647#section-3.4>.

Example:

	"name": "Frequency",

**Localised text objects (variable properties matched through "Filtering"):**

All available language matches according to the user’s preferred language choices expressed through the HTTP content negotiation (Accept-Language header parameter) is to be provided for each localised text element. 

This language matching type is called "Filtering", see <https://tools.ietf.org/html/rfc4647#section-3.3>.

Example:

	"names": {
		"en": "Frequency", 
		"fr": "Fréquence"
	},


The localised text object needs to be present whenever the related localised best-language-match text strings is present, and especially whenever a localised text is mandatory in the SDMX Information model. Note that localised text (and the knowledge about the locale) is mandatory in structure messages when artefacts are being submitted for storage to a registry or to other databases. The localised text object is important for use cases where multiple languages are required or where the information on the language used is required.


In case that there is no language match for a particular localisable element, it is optional to:

- return the element in a system-default language or alternatively to not return the element
- indicate available alternative languages for the element's maintainable artefact through links to underlying localised resources

**It is recommended to indicate all languages used anywhere in the message for localised elements through http Content-Language response header (languages of the intended audience) and/or through a “contentLanguages” property in the meta tag.** The main language used can be indicated through the “lang” property in the meta tag.


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
			"title": "Invalid parameter",
			"titles": {
				"en": "Invalid parameter",
				"fr": "Paramètre invalide"
			},
			"wsCustomErrorCode": 39272
		}
	]
	```