# Appendix A. Full Example with Comments {.unnumbered}

```json
{
  "header": {

    # dynamically generated GUI
    "id": "62b5f19d-f1c9-495d-8446-a3661ed24753",

    # extraction time from db (=now in SQL query), include timezone!
    "prepared": "2012-11-29T08:40:26Z",

    # optional with default false
    "test": false,

    "sender": {
      "id": "ECB",
      "name": "European Central Bank",
      "contact": [
        {
          "name": "Statistics hotline",
          "department": "Statistics Department",
          "role": "helpdesk",
          "telephone": ["+00-00-99999"],
          "fax": ["+00-00-88888"],
          "uri": ["http://www.xyz.org"],
          "email": ["statistics@xyz.org"]
        }
      ]
    },

    # receiver is optional, info from user record if authenticated
    "receiver": {
      "id": "SDMX",
      "name": "SDMX",
      "contact": [
          {
              "name": "name",
              "department": "department",
              "role": "role",
              "telephone": ["telephone"],
              "fax": ["fax"],
              "uri": ["uri"],
              "email": ["sdmx@xyz.org"]
          }
      ]
    },

    "request": {
      # include complete URL as used by the client
      "uri": "http://www.myorg.org/ws/data/ECB_ICP1/M.PT+FI.N.000000+071100.4.INX?
      startPeriod=2009-01&dimensionAtObservation=AllDimensions"
    }
  },
  "errors": [
    {
      "code": 123,
      "message": "Invalid number of dimensions in parameter key"
    }
  ],
  "structure": {
    # resolvable uri to dataflow
    "uri": "http://sdw-ws.ecb.europa.eu/dataflow/ECB/EXR/1.0",

    "name": "dataflow name",
    "description": "dataflow description",
    "dimensions": {

      # dataSet is used only if grouping of dimensions with single values
      "dataSet": [
        {
          "id": "FREQ",
          "name": "Frequency",
          "description": "Description for the dimension",

          # 0-based position of dimension in key in user request url
          "keyPosition": 0,

          # restricted list of dimension and attribute roles (time, frequency,
          # geo, unit, scalefactor, referenceperiod, ...)
          "role": "frequency",

          "values": [
            {
              "id": "D",
              "name": "Daily"
            }
          ],
        }, {
          "id": "CURRENCY_DENOM",
          "name": "Currency denominator",
          "description": "Description for the dimension",
          "keyPosition": 3,
          "values": [
            {
              "id": "EUR",
              "name": "Euro"
            }
          ]
        }, {
          "id": "EXR_TYPE",
          "name": "Exchange rate type",
          "description": "Description for the dimension",
          "keyPosition": 4,
          "values": [
            {
              "id": "SP00",
              "name": "Spot rate"
            }
          ]
        }, {
          "id": "EXR_SUFFIX",
          "name": "Series variation - EXR context",
          "description": "Description for the dimension",
          "keyPosition": 5,
          "values": [
            {
              "id": "A",
              "name": "Average or standardised measure for given frequency"
            }
          ]
        }
      ],

      # only if dimensionAtObservation <> allDimensions
      "series": [
        {
          "id": "CURRENCY",
          "name": "Currency",
          "description": "Description for the dimension",
          "keyPosition": 2,
          "role": "unit",
          "values": [
            {
              "id": "NZD",
              "name": "New Zealand dollar"
            }, {
              "id": "RUB",
              "name": "Russian rouble",
            }
          ]
        }
      ],

      # only for dimensions used at observation level
      "observation": [
        {
          "id": "TIME_PERIOD",
          "name": "Time period or range",
          "description": "Description for the dimension",
          "role": "time",
          "values": [
            {
              "id": "2013-01-18",
              "name": "2013-01-18",
              "start": "2013-01-18T00:00:00Z",
              "end": "2013-01-18T23:59:59Z"
            },
            {
              "id": "2013-01-21",
              "name": "2013-01-21",
              "start": "2013-01-21T00:00:00Z",
              "end": "2013-01-21T23:59:59Z"
            }
          ]
        }
      ]
    },
    "attributes": {

      # only for attributes returned at dataset level
      "dataSet": [],

      # only for attributes returned at series level
      "series": [
        {
          "id": "ID",
          "name": "Attribute name",
          "description": "Description for the attribute",
          "role": null,
          "default": null,

          # inclusion of attachment level and its format to be decided
          # e.g. "attachment": [ true, true, true, true, true, true, false ],

          "values": [
            {
              # id property is optional to allow for uncoded attributes
              "id": null,
              "name": "New Zealand dollar (NZD)"
            },
            {
              "id": null,
              "name": "Russian rouble (RUB)"
            }
          ]
        }
      ],
      "observation": [
        {
          "id": "OBS_STATUS",
          "name": "Observation status",
          "description": "Description for the attribute",
          "role": null,

          # optional
          "default": "A",

          "values": [
            # a null attribute can be used to shorten the message by
            # using O index later in message
            null,

            {
              "id": "A",
              "name": "Normal value",
              "description": "Normal value"
            }
          ]
        }
      ]
    },
    "annotations": [
      {
        "title": "AnnotationTitle provides a title for the annotation.",
        "type": "AnnotationType is used to distinguish between annotations
        designed to support various uses.",
        "uri": "http://www.myorg.org/ws/uri/for/this/annotation",
        "text": "AnnotationText holds a language-specific string containing
        the text of the annotation.",
        "id": "The id attribute provides a non-standard identification of an
        annotation. It can be used to disambiguate annotations."
      }
    ]
  },
  "dataSets": [
    {
      "action": "Information",

      # optional first time period in returned message
      "reportingBegin": "2012-05-04",

      # optional last time period in returned message
      "reportingEnd": "2012-06-01",

      # optional only for version history
      "validFrom": "2012-01-01T10:00:00Z",

      # optional only for version history
      "validTo": "2013-01-01T10:00:00Z",

      # optional only for publication release calendars
      "publicationYear": "2005",

      # optional only for publication release calendars
      "publicationPeriod": "2005-Q1",

      # optional as per annotations
      "annotations": [0],

      # optional as per attributes at dataset level
      "attributes": [0],

      # 1st alternative (only if series level
      # (dimensionAtObservation <> allDimensions))

      "series": {
        "0": {
          "annotations": [],
          "attributes": [0],
          "observations": {
            "0": [1.5931, 0],
            "1": [1.5925, 0]
          }
        },
        "1": {
          "annotations": [ 34 ],
          "attributes": [1],
          "observations": {
            "0": [40.3426, 0],
            "1": [40.3000, 0]
          }
        }
      }
    },
    {
      "action": "Information",

      # 2nd alternative (only if no series level
      #  (dimensionAtObservation == allDimensions))

      "observations": {
          "0:0": [1.5931, 0],
          "0:1": [1.5925, 0],
          "1:0": [40.3426, 0],
          "1:1": [40.3000, 0]
      }
    },

    # In case that the server does not group dimensions
    # with single values at dataset level

    {
      "action": "Information",

      # 1st alternative (only if series level
      # (dimensionAtObservation <> allDimensions))

      "series": {
         "0:0:0:0;0": {
            "attributes": [0],
            "observations": {
                "0": [1.5931, 0],
                "1": [1.5925, 0]
            }
         },
         "0:0:0:0;1": {
            "attributes": [1],
            "observations": {
                "0": [40.3426, 0],
                "1": [40.3000, 0]
            }
         }
      }
    },
    {
      "action": "Information",

      # 2nd alternative (only if no series level
      # (dimensionAtObservation == allDimensions))

      "observations": {
          "0:0:0:0:0:0": [1.5931, 0],
          "0:0:0:0:0:1": [1.5925, 0],
          "0:0:0:0:1:0": [40.3426, 0],
          "0:0:0:0:1:1": [40.3000, 0]
      }
    },

    # In case the client is using the detail parameter
    # and the server supports it

    {
      "action": "Information",

      # Detail parameter: serieskeysonly. No observation values,
      # attributes or annotations.

      "observations": {
          "0:0": [],
          "0:1": [],
          "1:0": [],
          "1:1": []
      ]
    },
    {
      "action": "Information",

      # Detail parameter: dataonly. No attributes or annotations.

      "observations": {
          "0:0": [1.5931],
          "0:1": [1.5925],
          "1:0": [40.3426],
          "1:1": [40.3000]
      ]
    },
    {
      "action": "Information",

      # Detail parameter: nodata. No observation values
      # just attributes and annotations.

      "observations": {
          "0:0": [0],
          "0:1": [0],
          "1:0": [0],
          "1:1": [0]
      ]
    }
  ]
}
```
