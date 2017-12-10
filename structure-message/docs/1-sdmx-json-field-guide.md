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
    * *[Common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*

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
* name - *Object* *optional*. A list of human-readable localised *[names](#name)* of the resource.
* description - *Object* *optional*. A list of human-readable localised descriptions (see *[names](#name)*) of the resource.
* agencyID - *String* *optional*. ID of the agency maintaining this resource.
* version - *String* *optional*. Version of this resource. It is "1.0" by default.
* validFrom - *String* *optional*. A timestamp from which the version is valid. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* validTo - *String* *optional*.  A timestamp from which the version is superceded. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* isFinal - *Boolean* *optional*. True if this is the final version of the resource, otherwise False (draft version).
* isExternalReference - *Boolean* *optional*. If set to “true” it indicates that the content of the resource is held externally.
* links - *Array* *optional*. A collection of links to additional resources for the resource. See the section *[link](#link)*. **It is recommended to systematically include as the first link a self-referencing hyperlink (link with "rel"="self") to indicate the URL address of the resource, and its URN.**
* annotations - *Array* *optional*. Provides a list of annotation objects.

Example:

	{
	  "id": "MOBILE_NAVI_PUB",
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
* title - *String* *optional*. Provides a non-localised title for the annotation.
* type - *String* *optional*. Type is used to distinguish between annotations designed to support various uses. The types are not enumerated, and these can be freely specified by the creator of the annotations. The definitions and use of annotation types should be documented by their creator.
* text - *Object* *optional*. A list of human-readable localised texts (see *[names](#name)*) of the annotation.
* links - *Array* *optional*. *Links* field is an array of *[link](#link)* objects. If appropriate, a link to an additional external resource which may contain or supplement the annotation.

Example:

	{
	  "id": "74747",
	  "title": "Sample annotation",
	  "type": "reference",
	  "text": { "en": "Sample annotation text" },
	  "links": [
	    {
	      # link object #
	    }
	  ]
	}

### Common properties of SDMX artefacts of base type "ItemScheme"

All SDMX artefacts of base type "ItemScheme" (CategoryScheme, Codelist, ConceptScheme, ReportingTaxonomy, and OrganisationScheme) share the *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

In addition, they share the following common object properties:

* isPartial - *Boolean* *optional*. If set to true, it indicates that the resource contains only a sub-set of items.
* items - *Array* *optional*. Provides a list of *[items](#item)* if the resource inherits from the ItemScheme. **Note that the order of items is significant. In the use case of a submission of a partial list is is necessary to include preceding and succeeding items to allow determining the correct positioniong of the submitted items.**

Example:

	{
	  "id": "ITEMSCHEME_EXAMPLE",
	  "isPartial": false,
	  "items": [
		{
		  # item object #
		}
	  ]
	}

#### item

*Object* *optional*. Item within the ItemScheme (if the resource is a CategoryScheme, Codelist, ConceptScheme, ReportingTaxonomy, or OrganisationScheme. 

* id - *String*. Identifier for the item.
* name - *Object* *optional*. A list of human-readable localised *[names](#name)* of the item.
* description - *Object* *optional*. A list of human-readable localised description (see *[names](#name)*)  of the item.
* start, end - *String* *optional*. Start and end are instances of time that define the actual Gregorian calendar period covered by the values for the time dimension. The algorithm for computing start and end fields for any supported reporting period is defined in the SDMX Technical Notes. These fields should be used only when the component value represents one of the values for the time dimension. Values are considered as inclusive both for the start field and the end field. Values must follow the ISO 8601 syntax for combined dates and times, including time zone. These fields are useful for visualisation tools, when selecting the appropriate point in time for the time axis. Statistical data, can be collected, for example, at the beginning, the middle or the end of the period, or can represent the average of observations through the period. Based on this information and using the start and end fields, it is easy to get or calculate the desired point in time to be used for the time axis.
* parent - *Object* *optional*. Contains a *[parent](#parent)* object to indicate the ID for the parent of the item (which is itself an item) enabling the reconstruction of the ordered item hierarchy.
* links - *Array* *optional*. A collection of links to additional resources for the item. See the section [link](#link).
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* items - *Array* *optional*. Provides a list of child items of the item.

Examples:

	{
	  "id": "01",
	  "name": { "en": "Population and migration" },
	  "description": { "en": "Description for Population and migration" },
	  "parent: {
	    # parent object #
	  },
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
		  # item object (recursive) #
		}
	  ]
	}

	{
	  "id": "2010",
	  "name": "2010",
	  "start": "2010-01-01T00:00Z",
	  "end": "2010-12-31T23:59:59Z",
	}

###### parent

*Object* *optional*. A reference to a parent item.

* id - *String*. Unique identifier of the parent item.

Example:

	{
	  "id": "T"
	}

### dataStructure

...

### metadataStructure

...

### categoryScheme

See *[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*.

### conceptScheme

See *[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*.

In addition, `conceptScheme`'s *[item](#item)* artefacts share the following common object properties:

* representation - *Object* *optional*. The *[representation](#representation)* object defines the core representation that are allowed for a concept. The text format allowed for a concept is that which is allowed for any non-target object component.

Example:

	{
	  "id": "CS_BOP",
	  "name": { "en": "Balance of Payments Concept Scheme" },
	  "agencyId": "IMF",
	  "version": "1.9",
	  "items": [
		{
		  "id": "FREQ",
		  "name": { "en": "Frequency" },
		  "representation": {
			# representation object #
		  }
		}
	  ]
	}

#### representation

*Object* *optional*. A core representation for a concept. It is either a reference to a codelist which enumerates the possible values that can be used as the representation of this concept, or a text format.

* urn - *String* *optional*. The urn holds a valid SDMX Registry URN (see SDMX Registry Specification for details) of a codelist.
* id - *String* *optional*. Identifier for a codelist.
* agencyID - *String* *optional*. ID of the agency maintaining the codelist.
* version - *String* *optional*. Version of the codelist. It is "1.0" by default.
* enumerationFormat - *Object* *optional*. To be used only with a codelist reference. The *[enumerationFormat](#enumerationformat)* object defines a restricted version of a text format that only allows facets and text types applicable to codes. Although the time facets permit any value, an actual code identifier does not support the necessary characters for time. Therefore these facets should not contain time in their values.
* textFormat - *String* *optional*. As an exclusive alternative to a codelist reference. It defines the information for describing a full range of text formats and may place restrictions on the values of the other attributes, referred to as "facets". Allowed is only one of the following string values: String, Alpha, AlphaNumeric, Numeric, BigInteger, Integer, Long, Short, Decimal, Float, Double, Boolean, URI, Count, InclusiveValueRange, ExclusiveValueRange, Incremental, ObservationalTimePeriod, StandardTimePeriod, BasicTimePeriod, GregorianTimePeriod, GregorianYear, GregorianYearMonth, GregorianDay, ReportingTimePeriod, ReportingYear, ReportingSemester, ReportingTrimester, ReportingQuarter, ReportingMonth, ReportingWeek, ReportingDay, DateTime, TimeRange, Month, MonthDay, Day, Time, Duration, XHTML.

Examples:

	{
	  "urn": "urn:sdmx:org.sdmx.infomodel.codelist.Codelist=SDMX:CL_FREQ(2.0)",
	  "enumerationFormat": {
		  # enumerationFormat object #
	  }
	}

	{
	  "id": "CL_FREQ",
	  "agencyID": "SDMX",
	  "version": "2.0",
	  "enumerationFormat": {
		  # enumerationFormat object #
	  }
	}

	{
	  "textFormat": "String"
	}

##### enumerationFormat

*Object* *optional*. A restricted version of a text format that only allows facets and text types applicable to codes. Although the time facets permit any value, an actual code identifier does not support the necessary characters for time. Therefore these facets should not contain time in their values.

* textType *String* *optional*. One of the types: Alpha, AlphaNumeric, Numeric, BigInteger, Integer, Long, Short, Boolean, URI, Count, InclusiveValueRange, ExclusiveValueRange, Incremental, ObservationalTimePeriod, StandardTimePeriod, BasicTimePeriod, GregorianTimePeriod, GregorianYear, GregorianYearMonth, GregorianDay, ReportingTimePeriod, ReportingYear, ReportingSemester, ReportingTrimester, ReportingQuarter, ReportingMonth, ReportingWeek, ReportingDay, Month, MonthDay, Day, Duration.
* isSequence *Boolean* *optional*.
* interval *Integer* *optional*
* startValue *Integer* *optional*
* endValue *Integer* *optional*
* timeInterval *String* *optional*. A valid time duration string.
* startTime *String* *optional*. A valid standard time period (gYear, gYearMonth, date, dateTime and SDMX time periods).
* endTime *String* *optional*. A valid standard time period (gYear, gYearMonth, date, dateTime and SDMX time periods).
* minLength *Integer* *optional*. A positive integer.
* maxLength *Integer* *optional*. A positive integer.
* minValue *Integer* *optional*.
* maxValue *Integer* *optional*.
* pattern *String* *optional*.

Example:

	{
	  "pattern": "^[0-9][0-9]$"
	}

### codelist

See *[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*.

Note that the SDMX Information Model does not foresee an `items` property for codelist items, hierarchies being expressed through the `parent` property for codelist items. However, for retrieval use cases, implementers can choose to resolve the parent-child relationships also into recursive `items` properties of `items`.

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
* urn - *String* *optional*. The urn holds a valid SDMX Registry URN (see SDMX Registry Specification for details).
* uri - *String* *optional*. The uri attribute holds a URI that contains a link to additional information about the resource, such as a web page. This uri is not an SDMX resource.
* title - *Object* *optional*. A list of human-readable localised descriptions (see *[names](#name)*) of the target link. See the section on [localised strings](#localised-strings) on how the message deals with languages.
* type - *String* *optional*. A hint about the type of representation returned by the link.
* hreflang - *String* *optional*. The natural language of the external link, the same as used in the HTTP Accept-Language request header.

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

The *URL* captured in the `href` attribute can be *absolute* or *relative*. **It is recommended to use absolute URLs in case the SDMX-JSON message is archived.**

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
