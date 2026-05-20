# Handling indexes

The purpose of using indexes is to avoid the repetition of space-consuming
values of measures, attributes, dimensions and annotations. For the first 2
types, whenever their values are provided directly in the `values` array of the
component definition itself, then the datasets must only use the corresponding
element indexes in those arrays instead of the real values. Dimensions and
annotations will always use the indexes.

Let's say that the following data content of a message needs to be processed:

```json
{
  "structures": [
    {
      "id": "ECB_EXR_WEB",
      "links": [
        {
          "href": "https://sdw-wsrest.ecb.europa.eu/service/dataflow/ECB/EXR/1.0",
          "rel": "dataflow"
        }
      ],
      "dimensions": {
        "dataSet": [
          {
            "id": "FREQ",
            "name": "Frequency",
            "names": { "en": "Frequency" },
            "keyPosition": 0,
            "values": [
              {
                "id": "D",
                "name": "Daily",
                "names": { "en": "Daily" }
              }
            ]
          },
          {
            "id": "CURRENCY_DENOM",
            "name": "Currency denominator",
            "names": { "en": "Currency denominator" },
            "keyPosition": 2,
            "values": [
              {
                "id": "EUR",
                "name": "Euro",
                "names": { "en": "Euro" }
              }
            ]
          },
          {
            "id": "EXR_TYPE",
            "name": "Exchange rate type",
            "names": { "en": "Exchange rate type" },
            "keyPosition": 3,
            "values": [
              {
                "id": "SP00",
                "name": "Spot rate",
                "names": { "en": "Spot rate" }
              }
            ]
          },
          {
            "id": "EXR_SUFFIX",
            "name": "Series variation - EXR context",
            "names": { "en": "Series variation - EXR context" },
            "keyPosition": 4,
            "values": [
              {
                "id": "A",
                "name": "Average or standardised measure",
                "names": { "en": "Average or standardised measure" }
              }
            ]
          }
        ],
        "series": [
          {
            "id": "CURRENCY",
            "name": "Currency",
            "names": { "en": "Currency" },
            "keyPosition": 1,
            "values": [
              {
                "id": "NZD",
                "name": "New Zealand dollar",
                "names": { "en": "New Zealand dollar" }
              },
              {
                "id": "RUB",
                "name": "Russian rouble",
                "names": { "en": "Russian rouble" }
              }
            ]
          }
        ],
        "observation": [
          {
            "id": "TIME_PERIOD",
            "name": "Time period or range",
            "names": { "en": "Time period or range" },
            "values": [
              {
                "id": "2013-01-18",
                "name": "2013-01-18",
                "names": { "en": "2013-01-18" }
              },
              {
                "id": "2013-01-21",
                "name": "2013-01-21",
                "names": { "en": "2013-01-21" }
              }
            ]
          }
        ]
      },
      "attributes": {
        "dataSet": [],
        "series": [
          {
            "id": "TITLE",
            "name": "Series title",
            "names": { "en": "Series title" },
            "values": [
              {
                "name": "New Zealand dollar (NZD)",
                "names": { "en": "New Zealand dollar (NZD)" }
              },
              {
                "name": "Russian rouble (RUB)",
                "name": { "en": "Russian rouble (RUB)" }
              }
            ]
          }
        ],
        "observation": [
          {
            "id": "OBS_STATUS",
            "name": "Observation status",
            "names": { "en": "Observation status" },
            "values": [
              {
                "id": "A",
                "name": "Normal value",
                "names": { "en": "Normal value" }
              }
            ]
          }
        ]
      },
      "annotations": [
        {
          "title": "Sample series annotation title",
          "type": "example",
          "text": "Sample series annotation text",
          "texts": { "en": "Sample series annotation text" },
          "id": "ABC123456"
        },
        {
          "title": "Sample observation annotation title",
          "type": "example",
          "text": "Sample observation annotation text",
          "texts": { "en": "Sample observation annotation text" },
          "id": "XYZ98765"
        }
      ],
      "dataSets": [0]
    }
  ],
  "dataSets": [
    {
      "structure": 0,
      "action": "Information",
      "series": {
        "0": {
          // 0 is the index of the first value of (series-level) CURRENCY dimension: "NZD"
          "annotations": [0], // 0 is the index of the first value of annotations: "ABC123456"
          "attributes": [0], // 0 is the index of the first value of the (first) (series-level) TITLE attribute: "New Zealand dollar (NZD)"
          "observations": {
            "0": [1.5931, 0], // "0" corresponds to the first value of (obs-level) TIME_PERIOD dimension: "2013-01-18"
            // 1.5931 is the corresponding observation value
            // 0 is the index of the first value of (obs-level) OBS_STATUS attribute: "A"
            "1": [1.5925, 0] // "1" corresponds to the second value of (obs-level) TIME_PERIOD dimension: "2013-01-21"
            // 1.5925 is the corresponding observation value
            // 0 is the index of the first value of (obs-level) OBS_STATUS attribute: "A"
          }
        },
        "1": {
          // 1 is the index of the second value of (series-level) CURRENCY dimension: "RUB"
          "attributes": [1], // 1 is the index of the second value of the (first) (series-level) TITLE attribute: "Russian rouble (RUB)"
          "observations": {
            "0": [40.3426, 0], // "0" corresponds to the first value of (obs-level) TIME_PERIOD dimension: "2013-01-18"
            // 40.3426 is the corresponding observation value
            // 0 is the index of the first value of (obs-level) OBS_STATUS attribute: "A"
            "1": [40.3, 0, 1] // "1" corresponds to the second value of (obs-level) TIME_PERIOD dimension: "2013-01-21"
            // 40.3000 is the corresponding observation value
            // 0 is the index of the first value of (obs-level) OBS_STATUS attribute: "A"
            // 1 is the index of the second value of annotations: "XYZ98765" (because there is no other obs-level attribute defined)
          }
        }
      }
    }
  ]
}
```

There is one `dataSet` in the message, and it contains two `series`.

```json
"0": {
 "annotations": [0],
 "attributes": [0],
 "observations": {
  "0": [1.5931, 0],
  "1": [1.5925, 0]
 }
},
"1": {
 "attributes": [1],
 "observations": {
  "0": [40.3426, 0],
  "1": [40.3000, 0, 1]
 }
}
```

The `structure.dimensions` field tells us that, out of the 6 dimensions, 4 have
the same value for the 2 series and are therefore attached to the `dataSet`
level.

We see that, for the first series, we get the value 0:

```json
"0": { ... }
```

From the structure.dimensions.series information, we know that CURRENCY is the
(only) series dimension.

```json
"series": [
 {
  "id": "CURRENCY",
  "name": "Currency",
  "names": { "en": "Currency" },
  "keyPosition": 1,
  "values": [
   {
    "id": "NZD",
    "name": "New Zealand dollar",
    "names": { "en": "New Zealand dollar" }
   }, {
    "id": "RUB",
    "name": "Russian rouble",
    "names": { "en": "Russian rouble" }
   }
  ]
 }
]
```

The value "0" identified previously is the index of the item in the collection
of values for this component. In this case, the dimension value is therefore
"New Zealand dollar".

Now, for the first observation of the first series, we get the value 0:

```json
 "0": [...],
```

From the `structure.dimensions.observation` information, we know that
TIME_PERIOD is the (only) dimension at `observation` level.

```json
"observation": [
 {
  "id": "TIME_PERIOD",
  "name": "Time period or range",
  "names": { "en": "Time period or range" },
  "values": [
   {
    "id": "2013-01-18",
    "name": "2013-01-18",
    "names": { "en": "2013-01-18" }
   }, {
    "id": "2013-01-21",
    "name": "2013-01-21",
    "names": { "en": "2013-01-21" }
   }
  ]
 }
]
```

The value "0" identified previously is the index of the item in the collection
of values for this component. In this case, the dimension value is therefore
"2013-01-18".

Now, for the first (and only) attribute of the first observation of the first
series, we get the value 0 (here the last value in array):

"0": [1.5931, 0],

From the `structure.attributes.observation` information, we know that OBS_STATUS
is the (only) attribute at `observation` level.

```json
"observation": [
 {
  "id": "OBS_STATUS",
  "name": "Observation status",
  "names": { "en": "Observation status" },
  "values": [
   {
    "id": "A",
    "name": "Normal value",
    "names": { "en": "Normal value" }
   }
  ]
 }
]
```

The value 0 identified previously is the index of the item in the collection of
values for this component. In this case, the attribute value is therefore
"Normal value".

The same logic applies for mapping the other observations, its attributes and
annotations.
