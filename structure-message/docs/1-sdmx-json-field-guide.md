# Introduction to SDMX-JSON Structure Message

See the SDMX-JSON Data Message docs for a brief introduction of the SDMX information model. For additional information on the SDMX information model, please refer to the [SDMX documentation](http://sdmx.org/?page_id=10).

Samples, tools and other SDMX-JSON resources are available in the public Github repository <https://github.com/sdmx-twg/sdmx-json>.

Before we start, let's clarify a few more things about this guide:

- New fields may be introduced in later versions. Therefore consuming applications should tolerate the addition of new fields with ease.
- The ordering of fields in objects is undefined. The fields may appear in any order and consuming applications should not rely on any specific ordering. It is safe to consider a nulled field and the absence of a field as the same thing.
- Not all fields appear in all contexts. For example response with error messages may not contain fields for data, dimensions and attributes.

# Field Guide to SDMX-JSON Structure Message Objects

## Message

Message is the top level object and it contains the requested resources and as well as the metadata needed to interpret those data.

* header - *Object* *nullable*. Header contains basic technical information about the message, such as when it was prepared and who has sent it.
* resources - *Array* *nullable*. Provides a list of resource objects. That's where the requested resources will be.
* references - *Object* *nullable*. Collection of referenced resource objects. It contains the full information of referenced resources. The properties of the "references" object are the "urn" properties of the referenced resources.
* errors - *Array* *nullable*. When appropriate provides a list of error messages in addition to RESTful web services HTTP error status codes.

Example:

	{
	  "header": {
		# header fields #
	  },
	  "resources": [
		# resource objects #
	  ],
	  "references": {
		# referenced resource objects #
	  },
	  "errors": [
		# error messages #
	  ]
	}

## header

*Object* *nullable*. Header contains basic technical information about the message, such as when it was prepared and who has sent it.

* id - *String*. *String*. Unique string that identifies the message for further references.
* test - *Boolean* *nullable*. Indicates whether the message is for test purposes or not. False for normal messages.
* prepared - *String*. A timestamp indicating when the message was prepared. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* sender - *Object*. Information about the party that is transmitting the message.
* receiver - *Object* *nullable*. Information about the party that is receiving the message. This can be
useful if the WS requires authentication.
* links - *Array* *nullable*. A collection of links to additional resources for the header.

Example:

	"header": {
	  "id": "b1804c51-1ee3-45a9-bb75-795cd4e06489",
	  "prepared": "2016-01-03T12:54:12",
	  "test": false,
	  "sender": {
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

*Object*. Information about the party that is transmitting the message. Sender contains the following fields:

* id - *String*. A unique identifier of the party.
* name - *String* *nullable*. A human-readable name of the sender.
* contact - *Array* *nullable*. Information on how the party can be contacted.

Example:

	"sender": {
	  "id": "ECB",
	  "name": "European Central Bank",
	  "contact": [
		# contact details #
	  ]
	}

#### contact

*Array* *nullable*. A collection of contact details. Each object in the collection may contain the following field:

* name - *String*. The contact's name.
* department - *String* *nullable*. The organisational unit for the contact.
* role - *String* *nullable*. The responsibility of the contact.
* telephone - *Array* *nullable*. An array of telephone numbers for the contact.
* fax - *Array* *nullable*. An array of fax numbers for the contact person.
* uri - *Array* *nullable*. An array of uris. Each uri holds an information URL for the contact.
* X400 - *Array* *nullable*. An array of X400. Each X400 holds an X400 address of the contact.
* email - *Array* *nullable*. An array of email addresses for the contact person.

Example:

	"contact": [
	  {
		"name": "Statistics hotline",
		"department": "Statistics hotline",
		"role": "Statistics hotline",
		"telephone": [ "+00 0 00 00 00 00" ],
		"fax": [ "+00 0 00 00 00 01" ],
		"uri": [ "www.xyz.org" ],
		"X400": [ "X400" ],
		"email": [ "statistics@xyz.org" ]
	  }
	]

### receiver

*Object* *nullable*. Information about the party that is receiving the message. This can be useful if the WS requires authentication. Receiver contains the same fields as [sender](#sender).

### link

*Object* *nullable*. A link to an external resource.

* href - *String*. Absolute or relative URL of the external resource.
* rel - *String*. Relationship of the object to the external resource.
* title - *String* *nullable*. A human-friendly description of the target link.
* type - *String* *nullable*. A hint about the type of representation returned by the link.

See the section about the [linking mechanism](#linking-mechanism) for additional information.

Example:

	{
	  "href": "https://registry.sdmx.org/help.html",
	  "rel": "help",
	  "title": "Documentation about the SDMX Global Registry",
	  "type": "text/html"
	}

## resource

*Object* *nullable*. Provides the information about a requested resource. A resource can be any SDMX artifact that can be requested through a structure request: DataStructureDefinition, MetadataStructureDefinition, CategoryScheme, ConceptScheme, Codelist, HierarchicalCodelist, OrganisationsScheme, AgencyScheme, DataProviderScheme, DataConsumerScheme, OrganisationUnitScheme, Dataflow, Metadataflow, ReportingTaxonomy, ProvisionAgreement, StructureSet, Process, Categorisation, Constraint

* id - *String*. Identifier for the resource.
* uri - *String* *nullable*. The URL address of the resource.
* urn - *String* *nullable*. URN - typically a URL - which points to an external resource which may contain or supplement the annotation. If a specific behavior is desired, an annotation type should be defined which specifies the use of this field more exactly.
* name - *String* *nullable*. Resource name.
* description - *String* *nullable*. Description of the resource.
* agencyID - *String* *nullable*. ID of the agency maintaining this resource.
* version - *String* *nullable*. Version of this resource. It is "1.0" by default.
* validFrom - *String* *nullable*. A timestamp from which the version is valid. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* validTo - *String* *nullable*.  A timestamp from which the version is superceded. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* isFinal - *Boolean* *nullable*. True if this is the final version of the resource, otherwise False (draft version).
* isExternalReference - *Boolean* *nullable*. If set to “true” it indicates that the content of the resource is held externally.
* isPartial - *Boolean* *nullable*. If set to true, it indicates that the resource contains only a sub-set of items. Only for resources that inherit from the ItemScheme (CategoryScheme, Codelist, ConceptScheme, ReportingTaxonomy, and OrganisationScheme).
* links - *Array* *nullable*. A collection of links to additional resources for the resource. See the section [link](#link).
* annotations - *Array* *nullable*. Provides a list of annotation objects.
* items - *Array* *nullable*. Provides a list of items if the resource inherits from the ItemScheme (CategoryScheme, Codelist, ConceptScheme, ReportingTaxonomy, and OrganisationScheme). 

Example:

	{
	  "id": "MOBILE_NAVI_PUB",
	  "uri": "HTTP://www.xyz.org/resource/0123456789",
	  "urn": "urn:sdmx:org.sdmx.infomodel.datastructure.Dataflow=ECB.DISS:BSI_PUB(1.0)",
	  "name": "Economic concepts",
	  "description": "This is the description of Economic concepts",
	  "agencyID": "ECB.DISS",
	  "version": "1.0",
	  "validFrom": "2012-05-04",
	  "validTo": "2015-05-04",
	  "isFinal": true,
	  "isExternalReference": false,
	  "isPartial": false,
	  "links": [
		{
		  # link object#
		}
	  ],
	  "annotations":[
		{
		  # annotation object#
		}
	  ],
	  "items": [
		{
		  # item object #
		}
	  ],
	  "contact": [
		# contact details #
	  ]
	}

### annotation

*Object* *nullable*. Provides all information about an annotation. 

* id - *String* *nullable*. ID provides a non-standard identification of an annotation. It can be used to disambiguate annotations.
* title - *String* *nullable*. Provides a title for the annotation.
* type - *String* *nullable*. Type is used to distinguish between annotations designed to support various uses. The types are not enumerated, and these can be freely specified by the creator of the annotations. The definitions and use of annotation types should be documented by their creator.
* url - *String* *nullable*. URI - typically a URL - which points to an external resource which may contain or supplement the annotation. If a specific behavior is desired, an annotation type should be defined which specifies the use of this field more exactly.
* text - *String* *nullable*. Contains the text of the annotation.

Example:

	{
	  "id": "74747",
	  "title": "Sample annotation",
	  "type": "reference",
	  "url": "http://sample.org/annotations/74747",
	  "text": "Sample annotation text"
	}

### item

*Object* *nullable*. Item within the ItemScheme (if the resource is a CategoryScheme, Codelist, ConceptScheme, ReportingTaxonomy, or OrganisationScheme. 

* id - *String*. Identifier for the item.
* uri - *String* *nullable*. The URL address of the item.
* urn - *String* *nullable*. URN - typically a URL - which points to an external resource which may contain or supplement the annotation. If a specific behavior is desired, an annotation type should be defined which specifies the use of this field more exactly.
* name - *String* *nullable*. Item name.
* description - *String* *nullable*. Description of the item. 
* links - *Array* *nullable*. A collection of links to additional resources for the item. See the section [link](#link).
* annotations - *Array* *nullable*. Provides a list of annotation objects. See the section [annotation](#annotation).
* items - *Array* *nullable*. Provides a list of child items of the item.

Example:

	{
	  "id": "01",
	  "uri": "http://www.xyz.org/resource/0123456789",
	  "urn": "urn:sdmx:org.sdmx.infomodel.categoryscheme.Category=SDMX:SDMXStatSubMatDomainsWD1(1.0).1.1",
	  "name": "Population and migration",
	  "description": "Description for Population and migration",
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

## reference

*Object* *nullable*. Provides full information about a referenced resource object. See [resource](#resource) for the structure of a resource object. 

Example:

	{
	  "id": "FM",
	  "uri": "http://www.xyz.org/resource/0123456789",
	  "urn": "urn:sdmx:org.sdmx.infomodel.datastructure.Dataflow=ECB:FM(1.0)",
	  "name": "Financial market data",
	  "description": "This is the description of Financial market data",
	  "agencyID": "ECB",
	  "version": "1.0",
	  "validFrom": "2012-05-04",
	  "validTo": "2015-05-04",
	  "isFinal": true,
	  "isExternalReference": false,
	  "isPartial": false,
	  "items": [	
		{
		  # item object #
		}
	  ],
	  "links": [
		{
		  # link object #
		}
	  ],
	  "annotations": [
		{
		  # annotation object #
		}
	  ]
	}

References within the "references" object are accessible through the "urn" properties.

Example:

	"references": {
	  "urn:sdmx:org.sdmx.infomodel.datastructure.Dataflow=ECB:FM(1.0)":
	  {
		# referenced resource object #
	  }
	}


## error

*Object* *nullable*. Provides information about an error message.

* code - *number*. Provides a code number for the error message. Code numbers are defined in the SDMX 2.1 Web Services Guidelines.
* message - *string*. Provides the error message. Error messages are fully customizable by the service providers and should provide enough detail to ease understanding the reasons of the error.

Example:

	"errors": [
	  {
		"code": 150,
		"message": "Invalid number of dimensions in the key parameter"
	  }
	]

# Linking mechanism

Collections of links can be attached to various elements in SDMX-JSON.

Similarily with standards such as HTML5 and Atom, link elements in SDMX-JSON *must* define a *URL* (the `href` attribute) and a *semantic* (the `rel` attribute). This allows clients to follow the links they care about and ignore the ones whose semantic they are not interested in. In addition, links in SDMX-JSON *may* define a `title` (a human-friendly description of the target link) and a `type` (a hint about the type of representation returned by the link). Please refer to the [list of Media Types and Subtypes](http://www.iana.org/assignments/media-types/media-types.xhtml) assigned and listed by the IANA for additional information about expected values for the `type` attribute.

SDMX-JSON offers a list of predefined semantics, but implementers are free to extend it. The list of predefined semantics comes from the list of SDMX artefacts that can be returned by SDMX RESTful web services, semantics defined in [RFC5988](https://tools.ietf.org/rfc/rfc5988.txt) and additional items deemed to be useful in the context of statistical data dissemination. These semantics are:

  - SDMX artefacts: datastructure, metadatastructure, categoryscheme, conceptscheme, codelist, hierarchicalcodelist, organisationscheme, agencyscheme, dataproviderscheme, dataconsumerscheme, organisationunitscheme, dataflow, metadataflow, reportingtaxonomy, provisionagreement, structureset, process, categorisation, contentconstraint, attachmentconstraint, category, concept, code, organisation, agency, dataprovider, dataconsumer, organisationunit, reportingcategory, data
  - RFC5988: alternate, copyright, glossary, help, index.
  - Miscellaneous: calendar (link to a release calendar), source (information about the source of data), request (the SDMX RESTful query that triggered the SDMX-JSON response).

The *URL* captured in the `href` attribute can be *absolute* or *relative*. If you intend to archive the SDMX-JSON message, it is recommended to use absolute URLs.


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
		"message": "Invalid number of dimensions in the key parameter",
		"wsCustomErrorCode": 39272
	  }
	]
	```
