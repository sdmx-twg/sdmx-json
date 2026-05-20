# Data type conversions between SDMX and JSON

For the different non-localised data types of components, the following JSON
types can be used for the instances of their values:

| SDMX data types                               | JSON data type |
| --------------------------------------------- | -------------- |
| "Numeric", "Long", "Short", "Float", "Double" | "number"       |
| "Integer", "Count"                            | "integer"      |
| "Boolean"                                     | "boolean"      |
| all other SDMX data types                     | "string"       |

Multi-valued values use a JSON array of the corresponding above JSON type.  
Localised values use a specific JSON object based on "string"s.  
Whenever the component has no value, the JSON type `null` can be used.

For **intentionally missing** reference metadata values, the following special
values are to be used:

- For JSON "number" data type: `"NaN"` (string)
- For all other data types: `"#N/A"` (string)

??? example

    ```json
            "value": "NaN"

            "value": "#N/A"
    ```
