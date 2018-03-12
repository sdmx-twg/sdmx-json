# Introduction to SDMX-JSON Structure Message

See the SDMX-JSON Data Message docs for a brief introduction of the SDMX information model. For additional information on the SDMX information model, please refer to the [SDMX documentation](http://sdmx.org/?page_id=10).

Samples, tools and other SDMX-JSON resources are available in the public Github repository <https://github.com/sdmx-twg/sdmx-json>.

Before we start, let's clarify a few more things about this guide:

- New fields may be introduced in later versions. Therefore consuming applications should tolerate the addition of new fields with ease.
- The ordering of properties in objects is undefined. The properties may appear in any order and consuming applications should not rely on any specific ordering. It is safe to consider a nulled property and the absence of a property as the same thing.
- Not all properties appear in all contexts. For example response with error messages may not contain a data property.

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
* content-languages - *Array* *optional*. Array of strings containing the identifyer of all languages used anywhere in the message for localized elements, and thus the languages of the intended audience, representaing in an array format the same information than the http Content-Language response header, e.g. "en, fr-fr". See IETF Language Tags: https://tools.ietf.org/html/rfc5646#section-2.1. The array's first element indicates the main language used in the message for localized elements. **The usage of this property is recommended.** Consult the section on [localised strings](#localised-strings) on how the message deals with languages.
* name - *Object* *optional*. A list of human-readable localised *[names](#name)* for the transmission. See the section on [localised strings](#localised-strings) on how the message deals with languages.
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
      "content-languages": [ "en", "fr-fr" ],
      “name”: {
          # name object #
      },
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
* contacts - *Array* *optional*. A collection of *[contacts](#contact)*. Provides contact information for the party in regard to the transmission of the message.

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

* id - *String*. Identifier for the resource.
* name - *Object* *optional*. Human-readable localised *[names](#name)* of the contact.
* department - *Object* *optional*. Human-readable localised *[names](#name)* of the organisational structure for the contact.
* role - *Object* *optional*. The Hunam-readable localised name of the responsibility of the contact.
* telephones - *Array* *optional*. An array of telephone numbers for the contact.
* faxes - *Array* *optional*. An array of fax numbers for the contact person.
* uris - *Array* *optional*. An array of uris. Each uri holds an information URL for the contact.
* emails - *Array* *optional*. An array of email addresses for the contact person.
* x400s - *Array* *optional*. An array of X.400 addresses for the contact person.

See the section on [localised strings](#localised-strings) on how the message deals with languages.

Example:

    {
      "id": "HOTLINE",
      "name": { "en": "Statistics hotline" },
      "department": { "en": "Statistics hotline" },
      "role": { "en": "Statistics hotline" },
      "telephones": [ "+00 0 00 00 00 00" ],
      "faxes": [ "+00 0 00 00 00 01" ],
      "uris": [ "www.xyz.org" ],
      "emails": [ "statistics@xyz.org" ]
    }

### receiver

*Object* *optional*. Information about the party that is receiving the message. 
This can be useful if the WS requires authentication. Receiver contains the 
same fields as [sender](#sender).

### link

See the section on [linking mechanism](#linking-mechanism) for all information on links.

## data

*Object* *optional*. Header contains the message's “primary data”.

* *[Artefact type]* - *Array* *optional*. This field is an array of objects of one of the corresponding SDMX Information Model artefact types: *dataStructure*, *metadataStructure*, *categoryScheme*, *conceptScheme*, *codelist*, *hierarchicalCodelist*, *agencyScheme*, *dataProviderScheme*, *dataConsumerScheme*, *organisationUnitScheme*, *dataflow*, *metadataflow*, *reportingTaxonomy*, *provisionAgreement*, *structureSet*, *process*, *categorisation* and *constraint*. Each of the corresponding object properties is allowed at maximum one time. Contains the requested structural information according to the definition of this artefact. For more information, please see:

    * *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*
    * *[Common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*

    * *[dataStructures](#datastructure)*
    * *[metadataStructures](#metadatastructure)*
    * *[categorySchemes](#categoryscheme)*
    * *[conceptSchemes](#conceptscheme)*
    * *[codelists](#codelist)*
    * *[hierarchicalCodelists](#hierarchicalcodelist)*
    * *[agencySchemes](#agencyscheme)*
    * *[dataProviderSchemes](#dataproviderscheme)*
    * *[dataConsumerSchemes](#dataconsumerscheme)*
    * *[organisationUnitSchemes](#organisationunitscheme)*
    * *[dataflows](#dataflow)*
    * *[metadataflows](#metadataflow)*
    * *[reportingTaxonomies](#reportingtaxonomy)*
    * *[provisionAgreements](#provisionagreement)*
    * *[structureSets](#structureset)*
    * *[processes](#process)*
    * *[categorisations](#categorisation)*
    * *[constraints](#constraint)*

Example:

    "data": {
      "dataStructures": [
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
* agencyID - *String* *optional*. ID of the agency maintaining this resource.
* version - *String* *optional*. Version of this resource. It is "1.0" by default.
* name - *Object* *optional*. A list of human-readable localised *[names](#name)* of the resource.
* description - *Object* *optional*. A list of human-readable localised descriptions (see *[names](#name)*) of the resource.
* validFrom - *String* *optional*. A timestamp from which the version is valid. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* validTo - *String* *optional*.  A timestamp from which the version is superceded. Values must follow the ISO 8601 syntax for combined dates and times, including time zone.
* isFinal - *Boolean* *optional*. True if this is the final version of the resource, otherwise False (draft version).
* isExternalReference - *Boolean* *optional*. If set to “true” it indicates that the content of the resource is held externally.
* annotations - *Array* *optional*. Provides a list of annotation objects.
* links - *Array* *optional*. A collection of links to additional resources for the resource. See the section *[link](#link)*. **It is recommended to systematically include as the first link a self-referencing hyperlink (link with "rel"="self") to indicate the URL address of the resource, and its URN.**

Example:

	{
	  "id": "MOBILE_NAVI_PUB",
	  "agencyID": "ECB.DISS",
	  "version": "1.0",
	  "name": { "en": "Economic concepts" },
	  "description": { "en": "This is the description of Economic concepts" },
	  "validFrom": "2012-05-04",
	  "validTo": "2015-05-04",
	  "isFinal": true,
	  "isExternalReference": false,
	  "annotations":[
		{
		  # annotation object#
		}
	  ],
	  "links": [
		{
		  # link object#
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
* categories/concepts - *Array* *optional*. Provides a list of *[items](#item)* if the resource inherits from the ItemScheme. **Note that the order of items is significant. In the use case of a submission of a partial list is is necessary to include preceding and succeeding items to allow determining the correct positioniong of the submitted items.**

Example:

	{
	  "id": "MY_DOMAINS",
	  "isPartial": false,
	  "categories": [
		{
		  # item object #
		}
	  ]
	}

#### item

*Object* *optional*. Item within the ItemScheme (if the resource is a CategoryScheme, ConceptScheme, Codelist, AgencyScheme, DataProviderScheme, DataConsumerSchemes, OrganisationUnitSchemes or ReportingTaxonomy. 

* id - *String*. Identifier for the item.
* name - *Object* *optional*. A list of human-readable localised *[names](#name)* of the item.
* description - *Object* *optional*. A list of human-readable localised description (see *[names](#name)*)  of the item.
* start, end - *String* *optional*. Start and end are instances of time that define the actual Gregorian calendar period covered by the values for the time dimension. The algorithm for computing start and end fields for any supported reporting period is defined in the SDMX Technical Notes. These fields should be used only when the component value represents one of the values for the time dimension. Values are considered as inclusive both for the start field and the end field. Values must follow the ISO 8601 syntax for combined dates and times, including time zone. These fields are useful for visualisation tools, when selecting the appropriate point in time for the time axis. Statistical data, can be collected, for example, at the beginning, the middle or the end of the period, or can represent the average of observations through the period. Based on this information and using the start and end fields, it is easy to get or calculate the desired point in time to be used for the time axis.
* parent - *String* *optional*. Contains the ID or the URN for the parent of the item (which is itself an item) enabling the reconstruction of the ordered item hierarchy.
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources for the item. See the section [link](#link).
* categories/concepts/codes/agencies/dataProviders/dataConsumers/organisationUnits/reportingCategories - *Array* *optional*. Provides a list of child items of the item.

Examples:

	{
	  "id": "01",
	  "name": { "en": "Population and migration" },
	  "description": { "en": "Description for Population and migration" },
	  "parent": "T",
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
	  "categories": [	
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

### dataStructure

*Object*. Describes the structure of a data structure definition. A data structure definition is defined as a collection of metadata concepts, their structure and usage when used to collect or disseminate data.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

* dataStructureComponents - *Object* *optional*. The *[dataStructureComponents](#dataStructureComponents)* object defines the grouping of the sets of structural metadata concepts that have a defined structural role in the data structure definition, like dimensions, measure dimension, time dimension, primary measure and attributes. Note that for any component or group defined in a data structure definition, its id must be unique. This applies to the identifiers explicitly defined by the components as well as those inherited from the concept identity of a component. For example, if two dimensions take their identity from concepts with same identity (regardless of whether the concepts exist in different schemes) one of the dimensions must be provided a different explicit identifier. Although there are XML schema constraints to help enforce this, these only apply to explicitly assigned identifiers. Identifiers inherited from a concept from which a component takes its identity cannot be validated against this constraint. Therefore, systems processing data structure definitions will have to perform this check outside of the XML validation. There are also three reserved identifiers in a data structure definition; OBS_VALUE, TIME_PERIOD, and REPORTING_PERIOD_START_DAY. These identifiers may not be used outside of their respective defintions (PrimaryMeasure, TimeDimension, and ReportingYearStartDay). This applies to both the explicit identifiers that can be assigned to the components or groups as well as an identifier inherited by a component from its concept identity. For example, if an ordinary dimension (i.e. not the time dimension) takes its concept identity from a concept with the identifier TIME_PERIOD, that dimension must provide a different explicit identifier.

Example:

	{
		"id": "DSD1",
		"version": "1.0",
		"agencyID": "SDMX",
		"dataStructureComponents": {
			# dataStructureComponents object #
		}
	}

#### dataStructureComponents

*Object* *optional*. DataStructureComponents describes the structure of the grouping to the sets of metadata concepts that have a defined structural role in the data structure definition. At a minimum at least one dimension and a primary measure must be defined.

* attributeList - *Object* *optional*. The *[attributeList](#attributeList)* object is a collection of metadata concepts that define the attributes of the data structure definition. 
* dimensionList - *Object* *optional*. The *[dimensionList](#dimensionList)* object is an ordered set of structural metadata concepts that, combined, classify a statistical series, such as a time series, and whose values, when combined (the key) in an instance such as a data set, uniquely identify a specific series.
* groups - *Array* *optional*. Array of *[group](#group)* objects that are sets of structural metadata concepts (and possibly their values) that define a partial key derived from the key descriptor in a data structure definition. 
* measureList - *Object* *optional*. The *[measureList](#measureList)* object contains a single metadata concepts that define the primary measures of a data structure. 

Example:

	{
		"attributeList": {
			# attributeList object #
		},
		"dimensionList": {
			# dimensionList object #
		},
		"groups": [
			{
				# group object #
			}
		],
		"measureList": {
			# measureList object #
		}
	}

#### attributeList

*Object* *optional*. AttributeList describes the attributes in the data structure definition.

* id - *String*. Identifier for the attributeList.
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* attributes - *Array* *optional*. The *[attribute](#attribute)* object describes the definition of a data attribute, which is defined as a characteristic of an object or entity.
* reportingYearStartDays - *Array* *optional*. The *[reportingYearStartDay](#reportingYearStartDay)* object is a specialized data attribute which provides important context to the time dimension. If the value of the time dimension is one of the standard reporting periods (see common:ReportingTimePeriodType) then this attribute is used to state the month and day that the reporting year begins. This provides a reference point from which the actual calendar dates covered by these periods can be determined. If this attribute does not occur in a data set, then the reporting year start day will be assumed to be January 1.

Example: 

	{
		"id": "AttributeDescriptor",
		"attributes": [
			{
				# attribute object #
			}
		],
		"reportingYearStartDays": [
			{
				# reportingYearStartDay object #
			}
		]
	}
					
##### attribute

*Object* *optional*. Attribute describes the structure of a data attribute, which is defined as a characteristic of an object or entity. The attribute takes its semantic, and in some cases it representation, from its concept identity. An attribute can be coded by referencing a code list from its coded local representation. It can also specify its text format, which is used as the representation of the attribute if a coded representation is not defined. Neither the coded or uncoded representation are necessary, since the attribute may take these from the referenced concept. An attribute specifies its relationship with other data structure components and is given an assignment status. These two properties dictate where in a data message the attribute will be attached, and whether or not the attribute will be required to be given a value. A set of roles defined in concept scheme can be assigned to the attribute.

* id - *String* *optional*. Identifier for the attribute.
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* assignmentStatus - *String*. Indication whether reporting a given attribute is mandatory or conditional. The two possible values are "Mandatory" and "Conditional".
* attributeRelationship - *Object*. The *[attributeRelationship](#attributeRelationship)* object describes how the value of this attribute varies with the values of other components. These relationships will be used to determine the attachment level of the attribute in the various data formats. 				
* conceptIdentity - *String*. Urn reference to a concept where the identification of the concept scheme which defines it is contained in another context.
* conceptRoles - *Array* of *String*s *optional*. ConceptRole references (through URNs) the concepts which define roles which this attribute serves. If the concept from which the attribute takes its identity also defines a role the concept serves, then the isConceptRole indicator can be set to true on the concept identity rather than repeating the reference here.
* localRepresentation - *Object* *optional*. The *[localRepresentation](#localRepresentation)* object defines the representation for the attribute.				

Example:

	{
		"id": "OBS_STATUS",
		"assignmentStatus": "Mandatory",
		"attributeRelationship": {
			# attributeRelationship object #
		},
		"conceptIdentity": "urn:sdmx:org.sdmx.infomodel.conceptscheme.Concept=ECB:ECB_CONCEPTS(1.0).OBS_STATUS",
		"conceptRoles": [
			"urn:sdmx:org.sdmx.infomodel.conceptscheme.Concept=ECB:ECB_CONCEPTS(1.0).OBS_STATUS"
		],
		"localRepresentation": {
			# localRepresentation object #
		}
	}

###### attributeRelationship

*Object* *optional*. AttributeRelationship defines the structure for stating the relationship between an attribute and other data structure definition components.

* attachmentGroups - *Array* of *String*s *optional*. One or more URN references to (a) local GroupKey Descriptor(s). This is used to specify that the attribute should always be attached to the groups referenced here. Note that if one of the referenced dimensions is the time dimension, the groups referenced here will be ignored.
* dimensions - *Array* of *String*s *optional*. One or more URN references to (a) local dimension(s). This is used to reference dimensions in the data structure definition on which the value of this attribute depends. An attribute using this relationship can be either a group, series (or section), or observation level attribute. The attachment level of the attribute will be determined by the data format and which dimensions are referenced.
* group - *String* *optional*. Urn reference to a local GroupKey Descriptor. This is used as a convenience to referencing all of the dimension defined by the referenced group. The attribute will also be attached to this group.
* none - Empty *Object* *optional*. This means that value of the attribute will not vary with any of the other data structure components. This will always be treated as a data set level attribute.
* primaryMeasure - *String* *optional*. Urn reference to a primary measure locally, where the reference to the data structure definition which defines the primary measure is provided in another context (for example the data structure definition in which the reference occurs). This is used to specify that the value of the attribute is dependent upon the observed value. An attribute with this relationship will always be treated as an observation level attribute.

Examples:

	{
		"attachmentGroups": [
			"MY_GROUP"
		]
	}

	{
		"dimensions": [
			"FREQ", "CURRENCY"
		]
	}

	{
		"group": "MY_GROUP"
	}

	{
		"none": {}
	}

	{
		"primaryMeasure": "OBS_VALUE"
	}

###### localRepresentation

*Object* *optional*. LocalRepresentation defines the representation for the attribute.

* enumeration - *String* *optional*. Urn reference to a codelist.
* enumerationFormat - *Object* *optional*. Urn reference to a codelist. The *[enumerationFormat](#enumerationformat)* object defines a restricted version of a text format that only allows facets and text types applicable to codes. Although the time facets permit any value, an actual code identifier does not support the necessary characters for time. Therefore these facets should not contain time in their values.
* textFormat - *Object* *optional*. The *[textFormat](#textFormat)* object defines the information for describing a range of text formats restricted to the representations allowed for all components except for target objects, and that does not allow for multi-lingual values.

Examples:

	{
		"enumeration": "urn:sdmx:org.sdmx.infomodel.codelist.Codelist=ECB:CL_ORGANISATION(1.0)",
		"enumerationFormat": {
			# enumerationFormat object #
		}
	}

	{
		"textFormat": {
			# textFormat object #
		}
	}

####### textFormat

*Object* *optional*. TextFormat defines the information for describing a range of text formats restricted to the representations allowed for all components except for target objects, and that does not allow for multi-lingual values.

* decimals - *Integer* *optional*. Positive number (minimum: 1).
* endTime - *String* *optional*. A valid standard time period (gYear, gYearMonth, date, dateTime and SDMX time periods).
* endValue - *Number* *optional*.
* interval - *Number* *optional*.
* isSequence - *Boolean* *optional*.
* maxLength - *Integer* *optional*. Positive number (minimum: 1).
* maxValue - *Number* *optional*.
* minLength - *Integer* *optional*. Positive number (minimum: 1).
* minValue - *Number* *optional*.
* pattern - *String* *optional*.
* startTime - *String* *optional*. A valid standard time period (gYear, gYearMonth, date, dateTime and SDMX time periods).
* startValue - *Number* *optional*.
* textType - *String* *optional*. Allowed is only one of the following string values: String, Alpha, AlphaNumeric, Numeric, BigInteger, Integer, Long, Short, Boolean, URI, Count, InclusiveValueRange, ExclusiveValueRange, Incremental, ObservationalTimePeriod, StandardTimePeriod, BasicTimePeriod, GregorianTimePeriod, GregorianYear, GregorianYearMonth, GregorianDay, ReportingTimePeriod, ReportingYear, ReportingSemester, ReportingTrimester, ReportingQuarter, ReportingMonth, ReportingWeek, ReportingDay, Month, MonthDay, Day, Duration
* timeInterval - *String* *optional*. A valid time duration.

Example:

	{
		textType: "String",
		maxLength: 1050
	}

##### reportingYearStartDay

*Object* *optional*. ReportingYearStartDay defines the structure of the reporting year start day attribute. The reporting year start day attribute takes its semantic from its concept identity (usually the REPORTING_YEAR_START_DAY concept), yet is always has a fixed identifier (REPORTING_YEAR_START_DAY). The reporting year start day attribute always has a fixed text format, which specifies that the format of its value is always a day and month in the ISO 8601 format of '--MM-DD'. As with any other attribute, an attribute relationship must be specified. this relationship should be carefully selected as it will determin what type of data the data structure definition will allow. For example, if an attribute relationship of none is specified, this will mean the data sets conforming to this data structure definition can only contain data with standard reporting periods where the all reporting periods have the same start day. In this case, data reported as standard reporting periods from two entities with different fiscal year start days could not be contained in the same data set.

* id - *String* *optional*. Identifier for the reportingYearStartDay data attribute.
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* assignmentStatus - *String*. Indication whether reporting a given attribute is mandatory or conditional. The two possible values are "Mandatory" and "Conditional".
* attributeRelationship - *Object*. The *[attributeRelationship](#attributeRelationship)* object describes how the value of this attribute varies with the values of other components. These relationships will be used to determine the attachment level of the attribute in the various data formats. 				
* conceptIdentity - *String*. Urn reference to a concept where the identification of the concept scheme which defines it is contained in another context.
* localRepresentation - *Object* *optional*. The *[ReportingYearStartDayRepresentation](#ReportingYearStartDayRepresentation)* object defines the representation for the reporting year start day attribute. Enumerated values are not allowed and the text format is fixed to be a day and month in the ISO 8601 format of '--MM-DD'.  		

Example:

	{
		"id": "FISCALYEAR",
		"assignmentStatus": "Mandatory",
		"attributeRelationship": {
			# attributeRelationship object #
		},
		"conceptIdentity": "urn:sdmx:org.sdmx.infomodel.conceptscheme.Concept=ECB:ECB_CONCEPTS(1.0).FISCALYEAR",
		"localRepresentation": {
			# ReportingYearStartDayRepresentation object #
		}
	}

##### ReportingYearStartDayRepresentation

*Object* *optional*. ReportingYearStartDayRepresentation defines the representation for the reporting year start day attribute. Enumerated values are not allowed and the text format is fixed to be a day and month in the ISO 8601 format of '--MM-DD'.

* textFormat - *Object*. The *textFormat* object has only one single property *textType* fixed to "MonthDay". This type exists solely for the purpose of fixing the representation of the reporting year start day attribute.

Example:

	{
		"textFormat": {
			"textType": "MonthDay"
		}
	}

#### dimensionList

*Object* *optional*. DimensionList describes the key descriptor for a data structure definition. The order of the declaration of child dimensions is significant: it is used to describe the order in which they will appear in data formats for which key values are supplied in an ordered fashion (exclusive of the time dimension, which is not represented as a member of the ordered key). Any data structure definition which uses the time dimension should also declare a frequency dimension, conventionally the first dimension in the key (the set of ordered non-time dimensions). If is not necessary to assign a time dimension, as data can be organised in any fashion required.

* id - *String*. Identifier for the attributeList.
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* dimensions - *Array* *optional*. The *[dimension](#dimension)* object describes the structure of a dimension, which is defined as a statistical concept used (most probably together with other statistical concepts) to identify a statistical series, such as a time series, e.g. a statistical concept indicating certain economic activity or a geographical reference area.
* measureDimensions - *Array* *optional*. The *[measureDimension](#measureDimension)* object describes a special type of dimension which defines multiple measures in a data structure. This is represented as any other dimension unless it is the observation dimension. It takes its representation from a concept scheme, and this scheme defines the measures and their representations. When data is formatted with this as the observation dimension, these measures can be made explicit or the value of the dimension can be treated as any other dimension. If the measures are explicit, the representation of the observation will be specific to the core representation for each concept in the representation concept scheme. Note that it is necessary that these representations are compliant (the same or derived from) with that of the primary measure.
* timeDimensions - *Array* *optional*. The *[timeDimension](#timeDimension)* object describes a special dimension which designates the period in time in which the data identified by the full series key applies..

Example:

	{
		"id": "DimensionDescriptor",
		"dimensions": [
			{
				# dimension object #
			}
		],
		"measureDimensions": [
			{
				# measureDimension object #
			}
		],
		"timeDimensions": [
			{
				# timeDimension object #
			}
		],
	}

##### dimension

*Object* *optional*. Dimension describes the structure of an ordinary dimension, which is defined as a statistical concept used (most probably together with other statistical concepts) to identify a statistical series, such as a time series, e.g. a statistical concept indicating certain economic activity or a geographical reference area. The dimension takes its semantic, and in some cases it representation, from its concept identity. A dimension can be coded by referencing a code list from its coded local representation. It can also specify its text format, which is used as the representation of the dimension if a coded representation is not defined. Neither the coded or uncoded representation are necessary, since the dimension may take these from the referenced concept.

* id - *String* *optional*. Identifier for the dimension.
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* position - *Integer* *optional*. Positive integer (minimum: 0). The position attribute specifies the position of the dimension in the data structure definition, starting at 0. It is optional as the position of the dimension in the key descriptor (dimensionList object) always takes precedence over the value supplied here. This is strictly for informational purposes only.
* type - *String* *optional*. The type attribute identifies whether the dimension is a measure dimension, the time dimension, or a regular dimension. Although these are all apparent by the element names, this attribute allows for each dimension to be processed independent of its element as well as maintaining the restriction of only one measure and time dimension while still allowing dimension to occur in any order. The only possible values are: Dimension, MeasureDimension, TimeDimension.
* conceptIdentity - *String*. Urn reference to a concept where the identification of the concept scheme which defines it is contained in another context.
* conceptRoles - *Array* of *String*s *optional*. ConceptRole references concepts (through URNs) which define roles which this dimension serves. If the concept from which the dimension takes its identity also defines a role the concept serves, then the isConceptRole indicator can be set to true on the concept identity rather than repeating the reference here.
* localRepresentation - *Object* *optional*. The *[localRepresentation](#localRepresentation)* object defines the representation for the dimension.

Example:

	{
		"id": "FREQ",
		"position": 0,
		"type": "Dimension",
		"conceptIdentity": "urn:sdmx:org.sdmx.infomodel.conceptscheme.Concept=ECB:ECB_CONCEPTS(1.0).FREQ",
		"conceptRoles": [
			"FREQ"
		],
		"localRepresentation": {
			# localRepresentation object #
		}
	}

##### measureDimension

*Object* *optional*. MeasureDimension defines the structure of the measure dimension. It is derived from the base dimension structure, but requires that a coded representation taken from a concept scheme is given.

* id - *String* *optional*. Identifier for the measure dimension.
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* position - *Integer* *optional*. Positive integer (minimum: 0). The position attribute specifies the position of the dimension in the data structure definition, starting at 0. It is optional as the position of the dimension in the key descriptor (DimensionList element) always takes precedence over the value supplied here. This is strictly for informational purposes only.
* type - *String* *optional*. The type attribute identifies whether the dimension is a measure dimension, the time dimension, or a regular dimension. Although these are all apparent by the element names, this attribute allows for each dimension to be processed independent of its element as well as maintaining the restriction of only one measure and time dimension while still allowing dimension to occur in any order. The only possible values are: Dimension, MeasureDimension, TimeDimension.
* conceptIdentity - *String*. Urn reference to a concept where the identification of the concept scheme which defines it is contained in another context.
* conceptRoles - *Array* of *String*s *optional*. ConceptRole references concepts (through URNs) which define roles which this dimension serves. If the concept from which the dimension takes its identity also defines a role the concept serves, then the isConceptRole indicator can be set to true on the concept identity rather than repeating the reference here.
* localRepresentation - *Object*. The *measureDimension localRepresentation* object has only one required property *enumeration* which has to provide the URN reference to a concept scheme object.

Example:

	{
		"id": "MEASURE",
		"position": 5,
		"type": "MeasureDimension",
		"conceptIdentity": "urn:sdmx:org.sdmx.infomodel.conceptscheme.Concept=ECB:ECB_CONCEPTS(1.0).MEASURE",
		"conceptRoles": [
			"MEASURE"
		],
		"localRepresentation": {
			"enumeration": "urn:sdmx:org.sdmx.infomodel.conceptscheme.ConceptScheme=ECB:ECB_MEASURES(1.0)"
		}

##### timeDimension

*Object* *optional*. TimeDimension describes the structure of a time dimension. The time dimension takes its semantic from its concept identity (usually the TIME_PERIOD concept), yet is always has a fixed identifier (TIME_PERIOD). The time dimension always has a fixed text format, which specifies that its format is always the in the value set of the observational time period (see common:ObservationalTimePeriodType). It is possible that the format may be a sub-set of the observational time period value set. For example, it is possible to state that the representation might always be a calendar year. See the enumerations of the textType attribute in the LocalRepresentation/TextFormat for more details of the possible sub-sets. It is also possible to facet this representation with start and end dates. The purpose of such facts is to restrict the value of the time dimension to occur within the specified range. If the time dimension is expected to allow for the standard reporting periods (see common:ReportingTimePeriodType) to be used, then it is strongly recommended that the reporting year start day attribute also be included in the data structure definition. When the reporting year start day attribute is used, any standard reporting period values will be assumed to be based on the start day contained in this attribute. If the reporting year start day attribute is not included and standard reporting periods are used, these values will be assumed to be based on a reporting year which begins January 1.

* id - *String* *optional*. Identifier for the time dimension.
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* position - *Integer* *optional*. Positive integer (minimum: 0). The position attribute specifies the position of the dimension in the data structure definition, starting at 0. It is optional as the position of the dimension in the key descriptor (DimensionList element) always takes precedence over the value supplied here. This is strictly for informational purposes only.
* type - *String* *optional*. The type attribute identifies whether the dimension is a measure dimension, the time dimension, or a regular dimension. Although these are all apparent by the element names, this attribute allows for each dimension to be processed independent of its element as well as maintaining the restriction of only one measure and time dimension while still allowing dimension to occur in any order. The only possible values are: Dimension, MeasureDimension, TimeDimension.
* conceptIdentity - *String*. Urn reference to a concept where the identification of the concept scheme which defines it is contained in another context.
* localRepresentation - *Object*. The *localRepresentation* object has only one required property *[textFormat](#timeDimensionTextFormat)* which defines the representation for the time dimension.

Example:

	{
		"id": "TIME_PERIOD",
		"position": 6,
		"type": "TimeDimension",
		"conceptIdentity": "urn:sdmx:org.sdmx.infomodel.conceptscheme.Concept=ECB:ECB_CONCEPTS(1.0).TIME_PERIOD",
		"localRepresentation": {
			"textFormat": {
				# timeDimension textFormat object #
			}
		}
	}

##### timeDimensionTextFormat

*Object* *optional*. The timeDimension textFormat only allows time based format and specifies a default ObservationalTimePeriod representation and facets of a start and end time.

* endTime - *String* *optional*. End time for the time dimension.
* startTime - *String* *optional*. Start time for the time dimension.
* textType - *String* *optional*. Any of the following values: ObservationalTimePeriod, StandardTimePeriod, BasicTimePeriod, GregorianTimePeriod, GregorianYear, GregorianYearMonth, GregorianDay, ReportingTimePeriod, ReportingYear, ReportingSemester, ReportingTrimester, ReportingQuarter, ReportingMonth, ReportingWeek, ReportingDay, DateTime, TimeRange.

Example:

	{
		"endTime": "2050",
		"startTime": "1960",
		"textType": "ObservationalTimePeriod"
	}

#### group

*Object* *optional*. Group describes the structure of a group descriptor in a data structure definition. A group may consist of a of partial key, or collection of distinct cube regions or key sets to which attributes may be attached. The purpose of a group is to specify attributes values which have the same value based on some common dimensionality. All groups declared in the data structure must be unique - that is, you may not have duplicate partial keys. All groups must be given unique identifiers.

* id - *String*. Identifier for the group.
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* attachmentConstraint - *String* *optional*. URN reference to an attachment constraint that defines the key sets and/or cube regions that attributes may be attached to. This is an alternative to referencing the dimensions, and allows attributes to be attached to data for given values of dimensions.
* groupDimensions - *Array* *optional*. Each of the *groupDimension* objects contains only a reference to a dimension in the key descriptor (DimensionList) through its single property *dimensionReference*. Although it is conventional to declare dimensions in the same order as they are declared in the ordered key, there is no requirement to do so - the ordering of the values of the key are taken from the order in which the dimensions are declared. Note that the id of a dimension may be inherited from its underlying concept - therefore this reference value may actually be the id of the concept.

Examples:

	{
		"id": "GROUP1",
		"attachmentConstraint": "ATTACHMENTCONSTRAINT"
	}

	{
		"id": "GROUP1",
		"groupDimensions": [
			{
				"dimensionReference": "CURRENCY"
			},
			{
				"dimensionReference": "CURRENCY_DENOM"
			}
		]
	}

#### measureList

*Object* *optional*. MeasureList describes the structure of the measure descriptor for a data structure definition. Only a primary may be defined.

* id - *String* *optional*. Identifier for the measureList.
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* primaryMeasure - *Object*. The *[primaryMeasure](#primaryMeasure)* object defines the structure of the primary measure, which is the concept that is the value of the phenomenon to be measured in a data set. Although this may take its semantic from any concept, this is provided a fixed identifier (OBS_VALUE) so that it may be easily distinguished in data messages.

Example:

	{
		"id": "MEASURELIST",
		"primaryMeasure": {
			# primaryMeasure object #
		}
	}
		
#### primaryMeasure

*Object* *optional*. PrimaryMeasure describes the structure of the primary measure. It describes the observation values for all presentations of the data. The primary measure takes its semantic, and in some cases it representation, from its concept identity (conventionally the OBS_VALUE concept). The primary measure can be coded by referencing a code list from its coded local representation. It can also specify its text format, which is used as the representation of the primary measure if a coded representation is not defined. Neither the coded or uncoded representation are necessary, since the primary measure may take these from the referenced concept. Note that if the data structure declares a measure dimension, the representation of this must be a superset of all possible measure concept representations.

* id - *String* *optional*. Identifier for the measureList.
* annotations - *Array* *optional*. Provides a list of annotation objects. See the section [annotation](#annotation).
* links - *Array* *optional*. A collection of links to additional resources. See the section [link](#link).
* conceptIdentity - *String*. Urn reference to a concept where the identification of the concept scheme which defines it is contained in another context.
* localRepresentation - *Object* *optional*. The *[localRepresentation](#localRepresentation)* object defines the representation for the primaryMeasure.				

Example:

	{
		"id": "OBS_VALUE",
		"conceptIdentity": "urn:sdmx:org.sdmx.infomodel.conceptscheme.Concept=ECB:ECB_CONCEPTS(1.0).OBS_VALUE",
		"localRepresentation": {
			# localRepresentation object #
		}
	}

### metadataStructure

*Object*. Used to describe a metadata structure definition, which is defined as a collection of metadata concepts, their structure and usage when used to collect or disseminate reference metadata.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### categoryScheme

*Object*. Describes the structure of a category scheme. A category scheme is the descriptive information for an arrangement or division of categories into groups based on characteristics, which the objects have in common. This provides for a simple, leveled hierarchy or categories.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.
See *[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*.
There are no additional parameters.
The start, end and parent properties are not used.

Example:

	{
		"id": "TOPICS",
		"version": "1.0",
		"agencyID": "SDMX",
		"name": {
			"en-GB-oed": "Topics",
			"fr": "Thèmes"
		},
		"isPartial": true,
		"categories": [
			{
				"id": "TOPIC1",
				"name": {
					"en-GB-oed": "Topic 1",
					"fr": "Thème 1"
				},
				"categories": [
					{
						"id": "SUBTOPIC11",
						"name": {
							"en-GB-oed": "Topic 11",
							"fr": "Thème 11"
						}
					}
				]
			}
		]
	}


### conceptScheme

*Object*. Describes the structure of a concept scheme. A concept scheme is the descriptive information for an arrangement or division of concepts into groups based on characteristics, which the objects have in common. It contains a collection of concept definitions, that may be arranged in simple hierarchies.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.
See *[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*.

In addition, `conceptScheme`'s *[item](#item)* artefacts share the following common object properties:

* coreRepresentation - *Object* *optional*. The *[coreRepresentation](#coreRepresentation)* object defines the core representation that are allowed for a concept. The text format allowed for a concept is that which is allowed for any non-target object component.
* isoConceptReference - *Object* *optional*. The *[isoConceptReference](#isoConceptReference)* object provides a reference to an ISO 11179 concept.
* parent - *String* *optional*. Urn reference to a local concept. Parent captures the semantic relationships between concepts which occur within a single concept scheme. This identifies the concept of which the current concept is a qualification (in the ISO 11179 sense) or subclass.
The start and end properties are not used.

Example:

	{
	  "id": "CS_BOP",
	  "name": { "en": "Balance of Payments Concept Scheme" },
	  "agencyId": "IMF",
	  "version": "1.9",
	  "concepts": [
		{
		  "id": "FREQ",
		  "name": { "en": "Frequency" },
		  "coreRepresentation": {
			# coreRepresentation object #
		  },
		  "isoConceptReference": {
			# isoConceptReference object #
		  }
		}
	  ]
	}

#### coreRepresentation

*Object* *optional*. A core representation for a concept. It is either a reference to a codelist which enumerates the possible values that can be used as the representation of this concept, or a text format.

* enumeration - *String* *optional*. Urn reference to a codelist which enumerates the possible values that can be used as the representation of this concept. This must be a valid SDMX Registry URN (see SDMX Registry Specification for details) of a codelist.
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

* textType *String* *optional*. One of the types: String, Alpha, AlphaNumeric, Numeric, BigInteger, Integer, Long, Short, Boolean, URI, Count, InclusiveValueRange, ExclusiveValueRange, Incremental, ObservationalTimePeriod, StandardTimePeriod, BasicTimePeriod, GregorianTimePeriod, GregorianYear, GregorianYearMonth, GregorianDay, ReportingTimePeriod, ReportingYear, ReportingSemester, ReportingTrimester, ReportingQuarter, ReportingMonth, ReportingWeek, ReportingDay, Month, MonthDay, Day, Duration.
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

#### isoConceptReference

*Object*. Provides a reference to an ISO 11179 concept.

* conceptAgency - *String*.
* conceptID - *String*.
* conceptSchemeID - *String*.

### codelist

*Object*. Defines the structure of a codelist. A codelist is defined as a list from which some statistical concepts (coded concepts) take their values.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.
See *[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*.

Note that the SDMX Information Model does not foresee an `codes` property for codelist items (`code`), hierarchies being expressed through the `parent` property for codelist items, which contains the ID of the parent code. However, for retrieval use cases, implementers can choose to resolve the parent-child relationships also into recursive `code` properties of `code`. A `code` describes the structure of a code. A code is defined as a language independent set of letters, numbers or symbols that represent a concept whose meaning is described in a natural language. Presentational information not present may be added through the use of annotations.

In addition, `codelist`'s *[item](#item)* artefacts share the following common object properties:

* parent - *String* *optional*. The ID of the parent code. Parent provides the ability to describe simple hierarchies within a single codelist, by referencing the ID value of another code in the same codelist.
The start and end properties are not used.

Example:

	{
		"id": "CODELIST1",
		"version": "1.0",
		"agencyID": "SDMX",
		"name": {
			"en": "Code list 1"
		},
		"isPartial": true,
		"codes": [
			{
				"id": "CODE1",
				"name": {
					"en": "Code 1"
				}
			},
			{
				"id": "CODE11",
				"name": {
					"en": "Code 11"
				},
				"parent": "CODE1"
			}
		]
	}


### hierarchicalCodelist

*Object*. Describes the structure of a hierarchical codelist. A hierarchical code list is defined as an organised collection of codes that may participate in many parent/child relationships with other codes in the list, as defined by one or more hierarchy of the list.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### agencyScheme

*Object*. Defines a specific type of organisation scheme which contains only maintenance agencies. The agency scheme maintained by a particular maintenance agency is always provided a fixed identifier and version, and is never final. Therefore, agencies can be added or removed without have to version the scheme. Agencies schemes have no hierarchy, meaning that no agency may define a relationship with another agency in the scheme. In fact, the actual parent agency for an agency in a scheme is the agency which defines the scheme.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.
See *[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*.

In addition, `agencyScheme`'s *[item](#item)* artefacts share the following common object properties:

* contacts - *Array* *optional*. A collection of *[contacts](#contact)*. Provides contact information for the agency. The contacts defined for the organisation are specific to the agency role the organisation is serving.

See the schema file for more information.

Example:

	{
		"id": "AGENCIES",
		"version": "1.0",
		"agencyID": "SDMX",
		"isExternalReference": false,
		"isFinal": false,
		"name": {
			"en": "SDMX Agency Scheme"
		},
		"links": [
			{
				"href": "https://web-service-root/agencyScheme/SDMX/AGENCIES/1.0",
				"rel": "self",
				"urn": "urn:sdmx:org.sdmx.infomodel.base.AgencyScheme=SDMX:AGENCIES(1.0)"
			}
		],
		"isPartial": true,
		"agencies": [
			{
				"id": "SDMX",
				"name": {
					"en": "SDMX"
				},
				"contacts": [
					{
						"id": "SDMX",
						"name": {
							"en": "SDMX"
						},
						"uris": [
							"sdmx.org"
						]
					}
				]
			}
		]
	}

### dataProviderScheme

*Object*. Defines a type of organisation scheme which contains only data providers. The data provider scheme maintained by a particular maintenance agency is always provided a fixed identifier and version, and is never final. Therefore, providers can be added or removed without have to version the scheme. This scheme has no hierarchy, meaning that no organisation may define a relationship with another organisation in the scheme.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.
See *[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*.

See the schema file for more information.

### dataConsumerScheme

*Object*. Defines a type of organisation scheme which contains only data consumers. The data consumer scheme maintained by a particular maintenance agency is always provided a fixed identifier and version, and is never final. Therefore, consumers can be added or removed without have to version the scheme. This scheme has no hierarchy, meaning that no organisation may define a relationship with another organisation in the scheme.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.
See *[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*.

See the schema file for more information.

### organisationUnitScheme

*Object*. Defines a type of organisation scheme which simply defines organisations and there parent child relationships. Organisations in this scheme are assigned no particular role, and may in fact exist within the other type of organisation schemes as well.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.
See *[common properties of SDMX artefacts of base type "ItemScheme"](#common-properties-of-sdmx-artefacts-of-base-type-itemscheme)*.

See the schema file for more information.

### dataflow

*Object*. Describes the structure of a data flow. A data flow is defined as the structure of data that will provided for different reference periods. If this type is not referenced externally, then a reference to a data structure definition must be provided.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

In addition, `dataflow` has the following property:

* structure - *String* *optional*. Urn reference to the data structure definition which defines the structure of all data for this flow.

Example:

	{
		"id": "EXR",
		"version": "1.0",
		"agencyID": "ECB",
		"isExternalReference": false,
		"isFinal": false,
		"name": {
			"en": "Exchange Rates"
		},
		"links": [
			{
				"href": "/dataflow/ECB/EXR/1.0",
				"rel": "self",
				"urn": "urn:sdmx:org.sdmx.infomodel.datastructure.Dataflow=ECB:EXR(1.0)",
			}
		],
		"structure": "urn:sdmx:org.sdmx.infomodel.datastructure.DataStructure=ECB:ECB_EXR1(1.0)"
	}

### metadataflow

*Object*. Describes the structure of a metadata flow. A dataflow is defined as the structure of reference metadata that will be provided for different reference periods. If this type is not referenced externally, then a reference to a metadata structure definition must be provided.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### reportingTaxonomy

*Object*.  Describes the structure of a reporting taxonomy, which is a scheme which defines the composition structure of a data report where each component can be described by an independent structure or structure usage description.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### provisionAgreement

*Object*. Describes the structure of a provision agreement. A provision agreement defines an agreement for a data provider to report data or reference metadata against a flow. Attributes which describe how the registry must behave when data or metadata is registered against this provision agreement are supplied.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### structureSet

*Object*. Describes the structure of a structure set. It allows components in one structure, structure usage, or item scheme to be mapped to components in another structural component of the same type.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### process

*Object*. Describes the structure of a process, which is a scheme which defines or documents the operations performed on data in order to validate data or to derive new information according to a given set of rules. Processes occur in order, and will continue in order unless a transition dictates another step should occur.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

### categorisation

*Object*. Defines the structure for a categorisation. A source object is referenced via an object reference and the target category is referenced via the target category.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

In addition, `categorisation` has the following properties:

* source - *String* *optional*. Source is a urn reference to an object to be categorized.
* target - *String* *optional*. Target is a urn reference to the category that the referenced object is to be mapped to.

Example:

	{
		"id": "53A341E8-D48B-767E-D5FF-E2E3E0E2BB19",
		"version": "1.0",
		"agencyID": "ECB",
		"isExternalReference": false,
		"isFinal": false,
		"name": {
			"en": "Categorise: DATAFLOWECB:EXR(1.0)"
		},
		"links": [
			{
				"href": "/categorisation/ECB/53A341E8-D48B-767E-D5FF-E2E3E0E2BB19/1.0",
				"rel": "self",
				"urn": "urn:sdmx:org.sdmx.infomodel.categoryscheme.Categorisation=ECB:53A341E8-D48B-767E-D5FF-E2E3E0E2BB19(1.0)"
			}
		],
		"source": "urn:sdmx:org.sdmx.infomodel.datastructure.Dataflow=ECB:EXR(1.0)",
		"target": "urn:sdmx:org.sdmx.infomodel.categoryscheme.CategoryScheme=ECB:MOBILE_NAVI(1.0).07"
	}

### constraint

*Object*. There exists 2 specific types of constraints: content and attachment contraints which have specific restrictions and extension. The inclusion of a key or region in a constraint is determined by first processing the included key sets, and then removing those keys defined in the excluded key sets. If no included key sets are defined, then it is assumed the all possible keys or regions are included, and any excluded key or regions are removed from this complete set.

See *[Common SDMX artefact properties](#common-sdmx-artefact-properties)*.

See the schema file for more information.

In addition, `constraint` has the following properties:

* constraintAttachment - *Object* *optional*. The *[constraintAttachment](#constraintAttachment)* object describes the collection of constrainable artefacts that the constraint is attached to.
* cubeRegions - *Array* *optional*. A list of of *[cubeRegion](#cubeRegion)* objects. CubeRegion describes a set of dimension values which define a region and attributes which relate to the region for the purpose of describing a constraint.
* dataKeySets - *Array* *optional*. A list of of *[dataKeySet](#dataKeySet)* objects. DataKeySet defines a collection of full or partial data keys.
* metadataKeySets - *Array* *optional*. A list of *[metadataKeySet](#metadataKeySet)* objects. MetadataKeySet defines a collection of metadata keys.
* metadataTargetRegions - *Array* *optional*. A list of of *[metadataTargetRegion](#metadataTargetRegion)* objects. Describes a set of target object values for a given report structure which define a region, and the metadata attribute which relate to the target for the purpose of describing a constraint.

Example: 

	{
		"id": "EXR_CONSTRAINTS",
		"version": "1.0",
		"agencyID": "ECB",
		"isExternalReference": false,
		"isFinal": false,
		"name": {
			"en": "Constraints for the EXR dataflow"
		},
		"links": [
			{
				"href": "/constraint/ECB/EXR_CONSTRAINTS/1.0",
				"rel": "self",
				"urn": "urn:sdmx:org.sdmx.infomodel.registry.ContentConstraint=ECB:EXR_CONSTRAINTS(1.0)"
			}
		],
		"constraintAttachment": {
			# constraintAttachment object #
		},
		"cubeRegions": [
			{
				# cubeRegion object #
			}
		],
		"dataKeySets": [
			{
				# dataKeySet object #
			}
		],
		"metadataKeySets": [
			{
				# metadataKeySet object #
			}
		],
		"metadataTargetRegions": [
			{
				# metadataTargetRegion object #
			}
		]
	}

#### constraintAttachment

*Object*. ConstraintAttachment describes a collection of references to constrainable artefacts.

* dataProvider - *String* *optional*. DataProvider is a URN reference to a data provider to which the constraint is attached. If this is used, then only the release calendar is relevant. 
* dataSets - *Array* *optional*. A list of *[dataSet reference](#dataSetReference)* objects. DataSet reference is a urn reference to a data set to which the constraint is attached.
* dataStructures - *Array* *optional* of *string*s. URN references to data structure definitions to which the constraint is attached. A constraint which is attached to more than one data structure must only express key sets and/or cube regions where the identifiers of the dimensions are common across all structures to which the constraint is attached. 
* dataflows - *Array* *optional* of *string*s. Urn references to data flows to which the constraint is attached. A constraint can be attached to more than one dataflow, and the dataflows do not necessarily have to be usages of the same data structure. However, a constraint which is attached to more than one data structure must only express key sets and/or cube regions where the identifiers of the dimensions are common across all structures to which the constraint is attached. 
* metadataSets - *Array* *optional*. A list of of *[metadataSet reference](#dataSetReference)* objects. MetadataSets reference is a urn reference to a metadata set to which the constraint is attached.
* metadataStructures - *Array* *optional* of *string*s. URN references to metadata structure definitions to which the constraint is attached. A constraint which is attached to more than one metadata structure must only express key sets and/or target regions where the identifiers of the target objects are common across all structures to which the constraint is attached. 
* metadataflows - *Array* *optional* of *string*s. Urn references to metadata flows to which the constraint is attached. A constraint can be attached to more than one metadataflow, and the metadataflows do not necessarily have to be usages of the same metadata structure. However, a constraint which is attached to more than one metadata structure must only express key sets and/or target regions where the identifiers of the target objects are common across all structures to which the constraint is attached. 
* provisionAgreements - *Array* *optional* of *string*s. Urn references to provision agreements to which the constraint is attached. A constraint can be attached to more than one provision agreement, and the provision agreements do not necessarily have to be references structure usages based on the same structure. However, a constraint which is attached to more than one provision agreement must only express key sets and/or cube/target regions where the identifier of the components are common across all structures to which the constraint is attached. 
* queryableDataSources - *Object* *optional*. The *[queryableDataSources](#queryableDataSources)* object describes a queryable data source to which the constraint is attached.
* simpleDataSources - *Array* *optional* of *string*s. URLs of SDMX-ML data or metadata messages.

Examples:

	{
		"dataProvider": "urn:sdmx:org.sdmx.infomodel...."
	}
	
	{
		"dataSets": [
			{
				# dataSetReference object #
			}
		]
	}
	
	{
		"dataStructures": [
			"urn:sdmx:org.sdmx.infomodel.datastructure.DataStructure=ECB:ECB_EXR1(1.0)"
		]
	}
	
	{
		"dataflows": [
			"urn:sdmx:org.sdmx.infomodel.datastructure.Dataflow=ECB:EXR(1.0)"
		]
	}
	
	{
		"metadataSets": [
			{
				# dataSetReference object #
			}
		]
	}
	
	{
		"metadataStructures": [
			"urn:sdmx:org.sdmx.infomodel.metadatastructure.MetadataStructure=ECB:ECB_EXR1_M(1.0)"
		]
	}
	
	{
		"metadataflows": [
			"urn:sdmx:org.sdmx.infomodel.metadatastructure.Metadataflow=ECB:EXR_M(1.0)"
		]
	}
	
	{
		"provisionAgreements": [
			"urn:sdmx:org.sdmx.infomodel...."
		]
	}

	{
		"queryableDataSources": [
			{
				# queryableDataSource object #
			}
		]
	}
	
	{
		"simpleDataSources": [
			"/data/EXR/M..EUR.SP00.A"
		]
	}

##### dataSetReference

*Object*. DataSetReference defines the structure of a reference to a data/metadata set. A full reference to a data provider and the identifier for the data set must be provided. Note that this is not derived from the base reference structure since data/metadata sets are not technically identifiable.

* dataProvider - *String*. DataProvider is a URN reference to a the provider of the data/metadata set. 
* id - *String*. ID contains the identifier of the data/metadata set being referenced. 

Example:

	{
		"dataProvider": "EXB",
		"id": "DATASET1"
	}

##### queryableDataSources

*Object*. QueryableDataSources describes a data source which accepts an standard SDMX Query message and responds appropriately.

* isRESTDatasource - *Boolean*.  
* isWebServiceDatasource - *Boolean*. 
* dataURL - *String*. DataURL contains the URL of the data source. 
* wadlURL - *String* *optional*. WADLURL provides the location of a WADL instance on the internet which describes the REST protocol of the queryable data source.
* wsdlURL - *String* *optional*. WSDLURL provides the location of a WSDL instance on the internet which describes the queryable data source.

Example:

	{
		"isRESTDatasource": true,
		"isWebServiceDatasource": true,
		"dataURL": "/data/EXR/M..EUR.SP00.A"
	}

#### cubeRegion

*Object*. CubeRegion defines the structure of a data cube region. This is based on the abstract RegionType and simply refines the key and attribute values to conform with what is applicable for dimensions and attributes, respectively. See the documentation of the base type for more details on how a region is defined.

* isIncluded - *Boolean* *optional*.
* attributes - *Array* *optional*. A list of of *AttributeValueSet* objects described in *[ValueSet](#ValueSet)* defines the structure for providing values for a data attribute. If no values are provided, the attribute is implied to include/excluded from the region in which it is defined, with no regard to the value of the data attribute. Note that for metadata attributes which occur within other metadata attributes, a nested identifier can be provided. For example, a value of CONTACT.ADDRESS.STREET refers to the metadata attribute with the identifier STREET which exists in the ADDRESS metadata attribute in the CONTACT metadata attribute, which is defined at the root of the report structure.
* keyValues - *Array* *optional*. A list of of *CubeRegionKey* objects described in *[ValueSet](#ValueSet)* for providing a set of values for a dimension for the purpose of defining a data cube region. A set of distinct value can be provided, or if this dimension is represented as time, and time range can be specified. 

Example:

	{
		"isIncluded": true,
		"attributes": [
			{
				# ValueSet object #
			}
		],
		"keyValues": [
			{
				# ValueSet object #
			}
		]
	}
		
##### ValueSet

*Object*. ValueSet 

* id - *String*. 
* timeRange - *Object* *optional*. A *[TimeRangeValue](#TimeRangeValue)* object.
* values - *Array* *optional* of *string*s. 
* cascadeValues - *Array* *optional* of *string*s. CascadeValues identifies which values in the previous array should be cascaded.

Example:

	{
		"id": "EXR_TYPE",
		"timeRange": {
			# TimeRangeValue object #
		},
		"values": [
			"NRP0", "NN00", "NRD0", "NRU1"
		],
		"cascadeValues": [
			"NRU1"
		]
	}

###### TimeRangeValue

*Object*. TimeRangeValue allows a time period value to be expressed as a range. It can be expressed as the period before a period, after a period, or between two periods. Each of these properties can specify their inclusion in regards to the range.

* afterPeriod - *Object* *optional*. A *[TimePeriodRange](#TimePeriodRange)* object. AfterPeriod is the period after which the period is meant to cover. This date may be inclusive or exclusive in the range.
* beforePeriod - *Object* *optional*. A *[TimePeriodRange](#TimePeriodRange)* object. BeforePeriod is the period before which the period is meant to cover. This date may be inclusive or exclusive in the range.
* endPeriod - *Object* *optional*. A *[TimePeriodRange](#TimePeriodRange)* object. EndPeriod is the end period of the range. This date may be inclusive or exclusive in the range.
* startPeriod - *Object* *optional*. A *[TimePeriodRange](#TimePeriodRange)* object. StartPeriod is the start date or the range that the queried date must occur within. This date may be inclusive or exclusive in the range.

Example:

	{
		"afterPeriod": {
			# TimePeriodRange object #
		},
		"beforePeriod": {
			# TimePeriodRange object #
		},
		"endPeriod": {
			# TimePeriodRange object #
		},
		"startPeriod": {
			# TimePeriodRange object #
		}
	}

####### TimePeriodRange

*Object*. TimePeriodRange defines a time period, and indicates whether it is inclusive in a range.

* period - *String* *optional*. Specifies a distinct time period or point in time in SDMX. The time period can either be a Gregorian calendar period, a standard reporting period, a distinct point in time, or a time range with a specific date and duration.
* isInclusive - *Boolean* *optional*.

Example:

	{
		"period": "2018",
		"isInclusive": true
	}

#### dataKeySet

*Object*. dataKeySet defines a collection of full or partial data keys (dimension values).

* isIncluded - *Boolean*.
* keys - Non-empty *array* of *[dataKey](#dataKey)* objects. Data Key contains a set of dimension values which identify a full set of data.

Example:

	{
		"isIncluded": true,
		"keys": [
			{
				# DataKey object #
			}
		]
	}

##### dataKey

*Object*. DataKey is a region which defines a distinct full or partial data key. The key consists of a set of values, each referencing a dimension and providing a single value for that dimension. The purpose of the key is to define a subset of a data set (i.e. the observed value and data attribute) which have the dimension values provided in this definition. Any dimension not stated explicitly in this key is assumed to be wild carded, thus allowing for the definition of partial data keys.

* keyValues - Non-empty *array* of *[dataKeyValue](#dataKeyValue)* objects.

Example:

	{
		"keyValues": [
			{
				# dataKeyValue object #
			}
		]
	}

###### dataKeyValue

*Object*. DataKeyValue provides a dimension value for the purpose of defining a distinct data key. Only a single value can be provided for the dimension.

* id - *String*. 
* value - *String*. 

Example:

	{
		"id": "EXR_TYPE",
		"value": "NRP0"
	}

#### metadataKeySet

*Object*. metadataKeySet defines a collection of metadata keys (identifier component values).

* isIncluded - *Boolean*.
* keys - Non-empty *array* of *[MetadataKey](#MetadataKey)* objects. Key contains a set of target object values for a specified report structure which serve to identify which object reference metadata conforming to the specified report structure is available for.

Example:

	{
		"isIncluded": true,
		"keys": [
			{
				# MetadataKey object #
			}
		]
	}

##### MetadataKey

*Object*. MetadataKey is a region which defines a distinct full or partial metadata key. The key consists of a set of values, each referencing a target object for the metadata target referenced in the metadataTarget attribute, which must be defined in the report structure referenced in the report attribute. Each target object can be assigned a single value. If an target object from the reference metadata target is not included in this key, the value of that is assumed to be all known objects for a reference target object, all possible keys for a key descriptor values target object, or all dates for report period target object. The purpose of this key reference a metadata conforming to a particular report structure for given object or set of objects.

* metadataTarget - *String*. The ID of the metadataTarget.
* report - *String*. The ID of the report.
* keyValues - Non-empty *array* of *[MetadataKeyValue](#MetadataKeyValue)* objects. Metadata Key Value provides a target object value for the purpose of defining a distinct metadata key. Only a single value can be provided for the target object.

Example:

	{
		"metadataTarget": "TARGET",
		"report": "REPORT",
		"keyValues": [
			{
				# MetadataKeyValue object #
			}
		]
	}

##### MetadataKeyValue

*Object*. MetadataKeyValue provides a target object value for the purpose of defining a distinct metadata key. Only a single value can be provided for the target object.

* id - *String*. The ID of the metadata key.
* dataKey - *Object* *optional*. *[DataKey](#DataKey)* object. DataKey is a region which defines a distinct full or partial data key. The key consists of a set of values, each referencing a dimension and providing a single value for that dimension. The purpose of the key is to define a subset of a data set (i.e. the observed value and data attribute) which have the dimension values provided in this definition. Any dimension not stated explicitly in this key is assumed to be wild carded, thus allowing for the definition of partial data keys.
* dataSet - *Object* *optional*. *[dataSet reference](#dataSetReference)* object. DataSet reference is a urn reference to a data set to which the constraint is attached.
* object - *String* *optional*. Urn reference to any object. The type of object actually referenced can be determined from the URN.
* value - *String* *optional*. Key values are meant to describe a distinct full or partial key.

Example:

	{
		"id": "A",
		"dataKey": {
			# DataKey object #
		},
		"dataSet": {
			# dataSetReference object #
		},
		"object": "a",
		"value": "a"
	}

#### metadataTargetRegion

*Object*. metadataTargetRegion defines the structure of a metadata target region. A metadata target region must define the report structure and the metadata target from that structure on which the region is based. This type is based on the abstract RegionType and simply refines the key and attribute values to conform with what is applicable for target objects and metadata attributes, respectively. See the documentation of the base type for more details on how a region is defined.

* isIncluded - *Boolean* *optional*.
* metadataTarget - *String*. The ID of the metadata target.
* report - *String*. The ID of the metadata report.
* attributes - *Array* *optional*. A list of of *MetadataAttributeValueSet* objects described in *[ValueSet](#ValueSet)* defines the structure for providing values for a metadata attribute. If no values are provided, the attribute is implied to include/excluded from the region in which it is defined, with no regard to the value of the metadata attribute.
* keyValues - *Array* *optional*. A list of *[MetadataTargetRegionKey](#MetadataTargetRegionKey)* objects that provide a set of values for a target object in a metadata target of a refence metadata report. A set of values or a time range can be provided for a report period target object. A collection of the respective types of references can be provided for data set reference and identifiable object reference target objects. For a key descriptor values target object, a collection of data keys can be provided.

Example:

	{
		"include": true,
		"metadataTarget": "TARGET",
		"report": "REPORT",
		"attributes": [
			# ValueSet object #
		],
		"keyValues": [
			# MetadataTargetRegionKey object #
		]
	}

##### MetadataTargetRegionKey

*Object*. MetadataTargetRegionKey provides a set of values for a target object in a metadata target of a refence metadata report. A set of values or a time range can be provided for a report period target object. A collection of the respective types of references can be provided for data set reference and identifiable object reference target objects. For a key descriptor values target object, a collection of data keys can be provided.

* id - *String*. ID of the metadata target region key.
* dataKeys - *Array* *optional* of *[DataKey](#DataKey)* objects. DataKey is a region which defines a distinct full or partial data key. The key consists of a set of values, each referencing a dimension and providing a single value for that dimension. The purpose of the key is to define a subset of a data set (i.e. the observed value and data attribute) which have the dimension values provided in this definition. Any dimension not stated explicitly in this key is assumed to be wild carded, thus allowing for the definition of partial data keys.
* dataSets - *Array* *optional* of *[dataSet reference](#dataSetReference)* objects. DataSet reference is a urn reference to a data set to which the constraint is attached.
* objects - *Array* *optional* of *string*s representing a URN reference to any object. The type of object actually referenced can be determined from the URN.
* timeRange - *Object* *optional*. A *[TimeRangeValue](#TimeRangeValue)* object.
* values - *Array* *optional* of *string*s. Key values are meant to describe a distinct full or partial key.

Example:

	{
		"id": "A",
		"dataKeys": [
			{
				# DataKey object #
			}
		],
		"dataSets": [
			{
				# dataSetReference object #
			}
		],
		"objects": [
			"urn:sdmx:org.sdmx.infomodel...."
		],
		"timeRange": {
			# TimeRangeValue object #
		},
		"values": [
			"A"
		]
	}

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
		"title": { "en": "Invalid number of items in the item parameter" }
		"wsCustomErrorCode": 39272
	  }
	]
	```
