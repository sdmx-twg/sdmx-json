# Appendix A. Full Example with Comments {.unnumbered}

```json
	{
	    "meta": {
	        "schema": "https://raw.githubusercontent.com/sdmx-twg/sdmx-json/develop/data-message/tools/schemas/1.0/sdmx-json-data-schema.json",
	        "comment for id": "# dynamically generated GUI",
	        "id": "62b5f19d-f1c9-495d-8446-a3661ed24753",
	        "comment for prepared": "# extraction time from db (=now in SQL query), include timezone!",
	        "prepared": "2012-11-29T08:40:26Z",
	        "comment for test": "# optional with default false",
	        "test": false,
	        "comment for content-languages": "# recommended indication of all languages (potentially) used in message",
	        "content-languages": ["en"],
	        "sender": {
	            "id": "ECB",
	            "name": {"en": "European Central Bank"},
	            "contact": [{
	                "name": {"en": "Statistics hotline"},
	                "department": {"en": "Statistics Department"},
	                "role": {"en": "helpdesk"},
	                "telephone": ["+00-00-99999"],
	                "fax": ["+00-00-88888"],
	                "uri": ["http://www.xyz.org"],
	                "email": ["statistics@xyz.org"]
	            }]
	        },
	        "comment for receiver": "# receiver is optional, info from user record if authenticated",
	        "receiver": {
	            "id": "SDMX",
	            "name": {"en": "SDMX"},
	            "contact": [{
	                "name": {"en": "name"},
	                "department": {"en": "department"},
	                "role": {"en": "role"},
	                "telephone": ["telephone"],
	                "fax": ["fax"],
	                "uri": ["uri"],
	                "email": ["sdmx@xyz.org"]
	            }]
	        },
	        "links": [{
	            "comment for href": "# include complete URL as used by the client",
	            "href": "http://www.myorg.org/ws/data/ECB_ICP1/M.PT.N.071100.4.INX",
	            "rel": "request",
	            "title": {"en": "Link to the url that returns this response"},
	            "type": "application/json"
	        }]
	    },
	    "errors": [{
	        "code": 123,
	        "title": {"en": "Invalid number of dimensions in parameter key"}
	    }],
	    "data": {
	        "structure": {
	            "links": [
	                {
	                    "href": "https://sdw-wsrest.ecb.europa.eu/service/datastructure/ECB/ECB_EXR1/1.0",
	                    "urn": "urn:sdmx:org.sdmx.infomodel.datastructure.DataStructure=ECB:ECB_EXR1(1.0)",
	                    "rel": "datastructure",
	                    "title": {"en": "resolvable uri to datastructure"}
	                },
	                {
	                    "href": "https://sdw-wsrest.ecb.europa.eu/service/dataflow/ECB/EXR",
	                    "urn": "urn:sdmx:org.sdmx.infomodel.datastructure.Dataflow=ECB:EXR(1.0)",
	                    "rel": "dataflow",
	                    "title": {"en": "resolvable uri to dataflow"}
	                },
	                {
	                    "href": "https://sdw-wsrest.ecb.europa.eu/service/provisionagreement/ECB/PA_EXR",
	                    "urn": "urn:sdmx:org.sdmx.infomodel.provisionagreement.ProvisionAgreement=ECB:PA_EXR(1.0)",
	                    "rel": "provisionagreement",
	                    "title": {"en": "resolvable uri to provision agreement"}
	                }
	            ],
	            "name": {"en": "dataflow name"},
	            "description": {"en": "dataflow description"},
	            "dimensions": {
	                "comment for dataset": "# dataSet is used only if grouping of dimensions with single values",
	                "dataSet": [
	                    {
	                        "id": "FREQ",
	                        "name": {"en": "Frequency"},
	                        "description": {"en": "Description for the dimension"},
	                        "comment for keyPosition": "# 0-based position of dimension in key in user request url",
	                        "keyPosition": 0,
	                        "comment for role": "# restricted list of dimension and attribute roles (time, frequency, geo, unit, scalefactor, referenceperiod, ...)",
	                        "role": "frequency",
	                        "values": [{
	                            "id": "D",
	                            "name": {"en": "Daily"}
	                        }]
	                    },
	                    {
	                        "id": "CURRENCY_DENOM",
	                        "name": {"en": "Currency denominator"},
	                        "description": {"en": "Description for the dimension"},
	                        "keyPosition": 2,
	                        "values": [{
	                            "id": "EUR",
	                            "name": {"en": "Euro"}
	                        }]
	                    },
	                    {
	                        "id": "EXR_TYPE",
	                        "name": {"en": "Exchange rate type"},
	                        "description": {"en": "Description for the dimension"},
	                        "keyPosition": 3,
	                        "values": [{
	                            "id": "SP00",
	                            "name": {"en": "Spot rate"}
	                        }]
	                    },
	                    {
	                        "id": "EXR_SUFFIX",
	                        "name": {"en": "Series variation - EXR context"},
	                        "description": {"en": "Description for the dimension"},
	                        "keyPosition": 4,
	                        "values": [{
	                            "id": "A",
	                            "name": {"en": "Average or standardised measure for given frequency"}
	                        }]
	                    }
	                ],
	                "comment for series": "# only if dimensionAtObservation <> allDimensions",
	                "series": [{
	                    "id": "CURRENCY",
	                    "name": {"en": "Currency"},
	                    "description": {"en": "Description for the dimension"},
	                    "keyPosition": 1,
	                    "role": "unit",
	                    "values": [
	                        {
	                            "id": "NZD",
	                            "name": {"en": "New Zealand dollar"}
	                        },
	                        {
	                            "id": "RUB",
	                            "name": {"en": "Russian rouble"}
	                        }
	                    ]
	                }],
	                "comment for observation": "# only for dimensions used at observation level",
	                "observation": [{
	                    "id": "TIME_PERIOD",
	                    "name": {"en": "Time period or range"},
	                    "description": {"en": "Description for the dimension"},
	                    "keyPosition": 5,
	                    "role": "time",
	                    "values": [
	                        {
	                            "id": "2013-01-18",
	                            "name": {"en": "2013-01-18"},
	                            "start": "2013-01-18T00:00:00Z",
	                            "end": "2013-01-18T23:59:59Z"
	                        },
	                        {
	                            "id": "2013-01-21",
	                            "name": {"en": "2013-01-21"},
	                            "start": "2013-01-21T00:00:00Z",
	                            "end": "2013-01-21T23:59:59Z"
	                        }
	                    ]
	                }]
	            },
	            "attributes": {
	                "comment for dataSet": "# only for attributes returned at dataset level",
	                "dataSet": [{
	                    "id": "TIME_FORMAT",
	                    "name": {"en": "Time Format"},
	                    "description": {"en": "Description for the attribute"},
						"relationship": {
							"none": {
							}
						},
	                    "role": "TIME_FORMAT",
	                    "default": "P1Y",
	                    "values": [{
	                        "id": "P1Y",
	                        "name": {"en": "Annual"}
	                    }]
	                }],
	                "comment for series": "# only for attributes returned at series level",
	                "series": [{
	                    "id": "ID",
	                    "name": {"en": "Attribute name"},
	                    "description": {"en": "Description for the attribute"},
						"relationship": {
							"dimensions": [
								"FREQ", "CURRENCY", "CURRENCY_DENOM", "EXR_TYPE", "EXR_SUFFIX"
							]
						},
	                    "role": null,
	                    "default": "ID1",
	                    "comment for values": "# inclusion of attachment level and its format to be decided, e.g. \"attachment\": [ true, true, true, true, true, true, false ],",
	                    "values": [
	                        {
	                            "comment for id": "# id property is optional to allow for uncoded attributes",
	                            "id": "ID1",
	                            "name": {"en": "New Zealand dollar (NZD)"}
	                        },
	                        {
	                            "id": "ID2",
	                            "name": {"en": "Russian rouble (RUB)"}
	                        }
	                    ]
	                }],
	                "observation": [{
	                    "id": "OBS_STATUS",
	                    "name": {"en": "Observation status"},
	                    "description": {"en": "Description for the attribute"},
						"relationship": {
							"primaryMeasure": "OBS_VALUE"
						},
	                    "role": null,
	                    "comment for default": "# optional",
	                    "default": "A",
	                    "comment for values": "# a null attribute can be used to shorten the message by using O index later in message",
	                    "values": [
	                        null,
	                        {
	                            "id": "A",
	                            "name": {"en": "Normal value"},
	                            "description": {"en": "Normal value"}
	                        }
	                    ]
	                }]
	            },
	            "annotations": [{
	                "title": "AnnotationTitle provides a title for the annotation.",
	                "type": "AnnotationType is used to distinguish between annotations.",
	                "text": {"en": "AnnotationText contains the text of the annotation."},
	                "id": "Non-standard identification of an annotation.",
	                "links": [{
	                    "href": "http://www.myorg.org/ws/uri/for/this/annotation",
	                    "rel": "description"
	                }]
	            }]
	        },
	        "dataSets": [
	            {
	                "links": [{
	                    "href": "https://sdw-wsrest.ecb.europa.eu/service/datastructure/ECB/ECB_EXR1/1.0",
	                    "urn": "urn:sdmx:org.sdmx.infomodel.datastructure.DataStructure=ECB:ECB_EXR1(1.0)",
	                    "rel": "datastructure"
	                }],
	                "action": "Information",
	                "comment for reportingBegin": "# optional first time period in returned message",
	                "reportingBegin": "2012-05-04",
	                "comment for reportingEnd": "# optional last time period in returned message",
	                "reportingEnd": "2012-06-01",
	                "comment for validFrom": "# optional only for version history",
	                "validFrom": "2012-01-01T10:00:00Z",
	                "comment for validTo": "# optional only for version history",
	                "validTo": "2013-01-01T10:00:00Z",
	                "comment for publicationYear": "# optional only for publication release calendars",
	                "publicationYear": "2005",
	                "comment for publicationPeriod": "# optional only for publication release calendars",
	                "publicationPeriod": "2005-Q1",
	                "comment for annotations": "# optional as per annotations",
	                "annotations": [0],
	                "comment for attributes": "# optional as per attributes at dataset level",
	                "attributes": [0],
	                "comment for series": "# 1st alternative (only if series level (dimensionAtObservation <> allDimensions))",
	                "series": {
	                    "0": {
	                        "annotations": [],
	                        "attributes": [0],
	                        "observations": {
	                            "0": [
	                                1.5931,
	                                0
	                            ],
	                            "1": [
	                                1.5925,
	                                0
	                            ]
	                        }
	                    },
	                    "1": {
	                        "annotations": [0],
	                        "attributes": [1],
	                        "observations": {
	                            "0": [
	                                40.3426,
	                                0
	                            ],
	                            "1": [
	                                40.3,
	                                0
	                            ]
	                        }
	                    }
	                }
	            },
	            {
	                "links": [{
	                    "href": "https://sdw-wsrest.ecb.europa.eu/service/datastructure/ECB/ECB_EXR1/1.0",
	                    "urn": "urn:sdmx:org.sdmx.infomodel.datastructure.DataStructure=ECB:ECB_EXR1(1.0)",
	                    "rel": "datastructure"
	                }],
	                "action": "Information",
	                "comment for observations": "# 2nd alternative (only if no series level (dimensionAtObservation == allDimensions))",
	                "observations": {
	                    "0:0": [
	                        1.5931,
	                        0
	                    ],
	                    "0:1": [
	                        1.5925,
	                        0
	                    ],
	                    "1:0": [
	                        40.3426,
	                        0,
	                        0
	                    ],
	                    "1:1": [
	                        40.3,
	                        0,
	                        0
	                    ]
	                }
	            },
	            {
	                "links": [{
	                    "href": "https://sdw-wsrest.ecb.europa.eu/service/datastructure/ECB/ECB_EXR1/1.0",
	                    "urn": "urn:sdmx:org.sdmx.infomodel.datastructure.DataStructure=ECB:ECB_EXR1(1.0)",
	                    "rel": "datastructure"
	                }],
	                "action": "Information",
	                "comment for dataSet(s)": "# In case that the server does not group dimensions with single values at dataset level",
	                "comment for series": "# 1st alternative (only if series level (dimensionAtObservation <> allDimensions))",
	                "series": {
	                    "0:0:0:0:0": {
	                        "annotations": [],
	                        "attributes": [0],
	                        "observations": {
	                            "0": [
	                                1.5931,
	                                0
	                            ],
	                            "1": [
	                                1.5925,
	                                0
	                            ]
	                        }
	                    },
	                    "0:0:0:0:1": {
	                        "annotations": [0],
	                        "attributes": [1],
	                        "observations": {
	                            "0": [
	                                40.3426,
	                                0
	                            ],
	                            "1": [
	                                40.3,
	                                0
	                            ]
	                        }
	                    }
	                }
	            },
	            {
	                "links": [{
	                    "href": "https://sdw-wsrest.ecb.europa.eu/service/datastructure/ECB/ECB_EXR1/1.0",
	                    "urn": "urn:sdmx:org.sdmx.infomodel.datastructure.DataStructure=ECB:ECB_EXR1(1.0)",
	                    "rel": "datastructure"
	                }],
	                "action": "Information",
	                "comment for dataSet(s)": "# 2nd alternative (only if no series level (dimensionAtObservation == allDimensions))",
	                "observations": {
	                    "0:0:0:0:0:0": [
	                        1.5931,
	                        0
	                    ],
	                    "0:0:0:0:0:1": [
	                        1.5925,
	                        0
	                    ],
	                    "0:0:0:0:1:0": [
	                        40.3426,
	                        0,
	                        0
	                    ],
	                    "0:0:0:0:1:1": [
	                        40.3,
	                        0,
	                        0
	                    ]
	                }
	            },
	            {
	                "links": [{
	                    "href": "https://sdw-wsrest.ecb.europa.eu/service/datastructure/ECB/ECB_EXR1/1.0",
	                    "urn": "urn:sdmx:org.sdmx.infomodel.datastructure.DataStructure=ECB:ECB_EXR1(1.0)",
	                    "rel": "datastructure"
	                }],
	                "action": "Information",
	                "comment for dataSet(s)": "# In case the client is using the detail parameter and the server supports it. # Detail parameter: serieskeysonly. No observation values, attributes or annotations.",
	                "observations": {
	                    "0:0": [],
	                    "0:1": [],
	                    "1:0": [],
	                    "1:1": []
	                }
	            },
	            {
	                "links": [{
	                    "href": "https://sdw-wsrest.ecb.europa.eu/service/datastructure/ECB/ECB_EXR1/1.0",
	                    "urn": "urn:sdmx:org.sdmx.infomodel.datastructure.DataStructure=ECB:ECB_EXR1(1.0)",
	                    "rel": "datastructure"
	                }],
	                "action": "Information",
	                "comment for dataSet(s)": "# Detail parameter: dataonly. No attributes or annotations.",
	                "observations": {
	                    "0:0": [1.5931],
	                    "0:1": [1.5925],
	                    "1:0": [40.3426],
	                    "1:1": [40.3]
	                }
	            },
	            {
	                "links": [{
	                    "href": "https://sdw-wsrest.ecb.europa.eu/service/datastructure/ECB/ECB_EXR1/1.0",
	                    "urn": "urn:sdmx:org.sdmx.infomodel.datastructure.DataStructure=ECB:ECB_EXR1(1.0)",
	                    "rel": "datastructure"
	                }],
	                "action": "Information",
	                "comment for dataSet(s)": "# Detail parameter: nodata. No observation values, just attributes and annotations.",
	                "observations": {
	                    "0:0": [0],
	                    "0:1": [0],
	                    "1:0": [
	                        0,
	                        0
	                    ],
	                    "1:1": [
	                        0,
	                        0
	                    ]
	                }
	            }
	        ]
	    }
	}
```
