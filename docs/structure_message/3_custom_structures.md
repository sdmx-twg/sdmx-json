# Custom Structure Instances

A Custom Structure Definition (CSD) lets an agency define a brand new kind of
maintainable artefact that does not exist in the SDMX Information Model — a
pivot table layout, a glossary, a report template. Because the shape of those
artefacts is invented by the agency rather than fixed by SDMX, the official
SDMX-JSON structure schema cannot describe them.

What the official schema does describe is the *definition* itself
(`data/customStructureDefinitions`, validated by `CustomStructureDefinitionType`)
and the parts of an *instance* that every SDMX artefact has
(`data/customStructures`, validated by `CustomStructureInstanceType`): `id`,
`agencyID`, `version`, `name`, `links`, and the mandatory
`customStructureDefinition` field that says which definition the instance
follows. `CustomStructureInstanceType` is deliberately **open** — it does not set
`unevaluatedProperties: false` — so the agency's own fields are allowed through
without being checked.

Everything else has to come from a second schema, **derived from the CSD**. Given
a definition that declares a `dataflow` property and a repeating `rows` property,
you can work out mechanically that instances must carry a `dataflow` field
holding a dataflow URN and may carry a `rows` array. That derived schema is what
the `*-schema.json` files in this folder are. They are not part of the SDMX
distribution and never will be: there is one per CSD version, they are produced by
software from the CSD, and they are only meaningful to systems that know about
that particular CSD.

This gives a two-level validation model, which is intentional:

| Validating with | Checks |
| --- | --- |
| The official structure schema alone | The message, the definitions in full, and the standard maintainable parts of each instance. Custom content passes untouched. |
| The official schema **plus** the derived schemas | The above, and the custom content of every instance whose definition you hold. |

An instance of a CSD you have never seen is therefore never rejected — it is
simply not checked in depth, and the dangling reference is caught later by
referential integrity checking. This mirrors the `processContents="lax"` wildcard
used by SDMX-ML for the same purpose.

# Generating Schema for Custom Instances

The rules below turn one CSD into one JSON Schema. They are deterministic: two
implementations following them produce equivalent schemas.

Throughout, `<structure-schema>` stands for
`https://json.sdmx.org/2.2/sdmx-json-structure-schema.json`. Every example is
taken from the files in this folder: the running example builds up
`pivot_table_1.0.0-schema.json` from `pivot_table_csd.json`, piece by piece, and
where the pivot table does not exercise a rule the glossary
(`custom_item_scheme_csd.json` → `glossary_1.0.0-schema.json`) is used instead.
Long descriptions are trimmed from the fragments to keep them readable.

## 1. What the generated schema validates

Generate **one schema per CSD version**, and have it validate **one instance
object** — a single member of the message's `data/customStructures` array — not a
whole message. Combining the instance schemas into a message-level schema is a
separate step, covered in the next section.

```json
{
    "$id": "urn:sdmx:org.sdmx.infomodel.csd.CustomStructureDefinition=IMF:PIVOT_TABLE(1.0.0)",
    "$schema": "https://json-schema.org/draft/2019-09/schema",
    "description": "Dynamically generated schema for urn:sdmx:org.sdmx.infomodel.csd.CustomStructureDefinition=IMF:PIVOT_TABLE(1.0.0) …",
    "$ref": "#/$defs/PivotTable",
    "$defs": {
        "PivotTable":       { "…": "the CSD itself, named by its sdmxClassName, section 2" },
        "RowColType":       { "…": "a customType, named by its id, section 3" },
        "SliceType":        { "…": "a customType, named by its id, section 3" },
        "AbstractRowColType": { "…": "a base definition, used by RowColType and SliceType, section 4" }
    }
}
```

**$id**  - a stable *$id* property is required so it can be referenced by the wrapper schema. 
A URL resolving to the published location of the schema can be used. If the schema is not
published, the URN of the CSD that the schema is generated for should be used.

**$schema** - fixed value `https://json-schema.org/draft/2019-09/schema`

**description** - optional description

**$ref** - `#/$defs/<SdmxClassName>`, using the `sdmxClassName` defined in the CSD

**$defs** - one entry per Custom Type defined in the CSD, one additional entry for
the SdmxClassName defined in the CSD, and one `Abstract` entry for each Custom Type
which is extended by another (see section 4).

`AbstractRowColType` is the one name in that list which does not come from the CSD. 
It is generated because SliceType extends RowColType, an `Abstract[custom type]` is required
to provide the base properties which both SliceType and RowColType share. Derive the
name predictably — `Abstract` prefixed to the Custom Type's `id` — and check it does not
collide with a Custom Type `id` in the CSD, since those are the names a modeller controls.

## 2. The Root Type

```json
"PivotTable": {
    "type": "object",
    "allOf": [
        { "$ref": "<structure-schema>#/$defs/CustomStructureInstanceType" },
        { "$ref": "<structure-schema>#/$defs/specificationExtensions" },
        {
            "properties": {
                "customStructureDefinition": {
                    "const": "urn:sdmx:org.sdmx.infomodel.csd.CustomStructureDefinition=IMF:PIVOT_TABLE(1.0.0)"
                },
                "dataflow": { "$ref": "<structure-schema>#/$defs/DataflowReferenceType" },
                "rows":     { "type": "array", "minItems": 1, "items": { "$ref": "#/$defs/RowColType" } },
                "cols":     { "type": "array", "minItems": 1, "items": { "$ref": "#/$defs/RowColType" } },
                "slice":    { "type": "array", "minItems": 1, "items": { "$ref": "#/$defs/SliceType" } }
            },
            "required": ["dataflow"]
        }
    ],
    "unevaluatedProperties": false
}
```

The root type is the SdmxClassName of the CSD, with the following properties:

**type** - fixed value `object`

**allOf** - array of three entries

1. `{"$ref": "<structure-schema>#/$defs/CustomStructureInstanceType"}` - brings in
   `id`, `agencyID`, `version`, `name`/`names`, `description`/`descriptions`,
   `links`, `annotations`, `isPartial` and `customStructureDefinition`.
2. `{"$ref": "<structure-schema>#/$defs/specificationExtensions"}` - allows `x-`
   extension fields.
3. A branch declaring the CSD's own properties, following the rules in section 5.

**unevaluatedProperties**  fixed value `false`

As the root type is also a custom type, the rules in section 5 apply to the `properties` section. In addition
this section requires the `customStructureDefinition` property which defines the URN of the CSD which this schema 
is generated for.


## 3. Custom Types

```json
"RowColType": {
    "type": "object",
    "allOf": [
        { "$ref": "<structure-schema>#/$defs/NameableType" },
        { "$ref": "<structure-schema>#/$defs/specificationExtensions" },
        { "$ref": "#/$defs/AbstractRowColType" }
    ],
    "required": ["id"],
    "unevaluatedProperties": false
}
```

A custom type has the following properties:

**type** - fixed value `object`

**allOf** - array with the following entries

1. `{"$ref": "<base-ref>"}` — bring in properties from the base type, see the `base-ref` table for the valid replacement options.
2. `{"$ref": "<structure-schema>#/$defs/specificationExtensions"}` — allows `x-` extension fields.
3. `{"$ref": "#/$defs/Abstract<CustomType>"}` for each Custom Type this one extends, see the rules in section 4.
4. The Custom Type's own properties, following the rules in section 5. These go in an
   `{"$ref": "#/$defs/Abstract<ThisCustomType>"}` when another Custom Type extends this
   one, and in an inline branch otherwise.

Entry 3 is absent unless the Custom Type extends another, and entry 4 is a `$ref`
rather than an inline branch when the Custom Type is itself extended, so the array
usually holds three entries rather than four. `RowColType` above is base +
extensions + `AbstractRowColType`, because `SliceType` extends it and so it keeps
its own properties in the abstract type; `SliceType` in section 4 is base +
extensions + `AbstractRowColType` + an inline branch for `position`; and `TermType`
below, which neither extends nor is extended, is base + extensions + an inline
branch.

**unevaluatedProperties**  fixed value `false`

Replace `<base-ref>` with a `$ref` value from the table below, based on the Custom Type's `extends` type.

   | `extends` | `$ref` | Also add |
   | --- | --- | --- |
   | `Annotatable` (or absent, with no `sdmxClassName`) | `<structure-schema>#/$defs/AnnotableType` | — |
   | `Identifiable` (or absent, with an `sdmxClassName`) | `<structure-schema>#/$defs/IdentifiableType` | `"required": ["id"]` |
   | `Nameable` | `<structure-schema>#/$defs/NameableType` | `"required": ["id"]` |


### Recursion

A custom type may reference itself or its container. `$ref` handles this
naturally; just make sure your generator does not try to inline types and loop
forever. The glossary's `TermType` declares a `children` property whose
representation is `TermType`, which becomes:

```json
"TermType": {
    "type": "object",
    "allOf": [
        { "$ref": "<structure-schema>#/$defs/NameableType" },
        { "$ref": "<structure-schema>#/$defs/specificationExtensions" },
        {
            "properties": {
                "definition": { "type": "array", "minItems": 1,
                                "items": { "$ref": "<structure-schema>#/$defs/LocalisedValueType" } },
                "children":   { "type": "array", "minItems": 1,
                                "items": { "$ref": "#/$defs/TermType" } }
            },
            "required": ["definition"]
        }
    ],
    "required": ["id"],
    "unevaluatedProperties": false
}
```

## 4. Abstract base types

```json
"AbstractRowColType": {
    "type": "object",
    "properties": {
        "level":       { "type": "string", "pattern": "^(0|[1-9][0-9]*)$" },
        "dimension":   { "$ref": "<structure-schema>#/$defs/idType" },
        "headingText": { "type": "string" },
        "headingCode": { "$ref": "<structure-schema>#/$defs/CodeReferenceType" }
    },
    "not": { "required": ["headingText", "headingCode"] },
    "required": ["dimension"]
}
```


```json
"SliceType": {
    "type": "object",
    "allOf": [
        { "$ref": "<structure-schema>#/$defs/NameableType" },
        { "$ref": "<structure-schema>#/$defs/specificationExtensions" },
        { "$ref": "#/$defs/AbstractRowColType" },
        { "properties": { "position": { "type": "string", "pattern": "^(0|[1-9][0-9]*)$" } } }
    ],
    "required": ["id"],
    "unevaluatedProperties": false
}
```

When a custom type has an `extendsType` to extend a sibling, both types share the same base properties 
(those defined in the extended type).  These shared properties are captured in a shared  `Abstract[CustomType]`.
Both the Custom Type and extending Custom Type references this object as a `$ref` in the `allOf` array.

Note that `SliceType` repeats the base (`NameableType`) rather than referencing
`RowColType`. Do **not** `$ref` the parent *type* from the subtype: the parent
closes itself with `unevaluatedProperties: false` and would reject `position`.

## 5. Properties

Each declared property becomes one field. The field name is the property `id`.

### 5a. Cardinality decides array or single value

- **`maxOccurs` absent means unbounded**, not 1. A single-valued property must
  state `"maxOccurs": 1` explicitly.
- **`minOccurs` absent means 1**, i.e. mandatory.

From those two:

| Definition says | Instance reports | Generate |
| --- | --- | --- |
| `maxOccurs: 1` | the value directly | the value schema itself |
| anything else | an array of values | `{"type": "array", "items": <value schema>}` |

For the array case, set `minItems` to `minOccurs` when that is 2 or more, and to
`1` otherwise — an empty array is never a meaningful way to report "no values";
omit the field instead. Set `maxItems` to `maxOccurs` when it is stated. Finally,
if `minOccurs` is 1 or more, add the field to the containing object's `required`
list, in both the array and the single-value case.

A property that belongs to a `mutuallyExclusive` set is effectively optional
whatever its `minOccurs` says, because at most one member of the set may appear.
Leave those out of `required`.

**Example.** Three properties from the samples, and the instance each one
produces:

| Definition | Reads as | Generated | In the instance |
| --- | --- | --- | --- |
| `{"id": "dataflow", "maxOccurs": 1}` | 1..1 | value schema, in `required` | `"dataflow": "urn:…"` |
| `{"id": "rows", "minOccurs": 0}` | 0..unbounded | array, `minItems: 1`, not required | `"rows": [ { … } ]` |
| `{"id": "definition"}` | 1..unbounded | array, `minItems: 1`, in `required` | `"definition": [ { … } ]` |

```json
"dataflow":   { "$ref": "<structure-schema>#/$defs/DataflowReferenceType" },
"rows":       { "type": "array", "minItems": 1, "items": { "$ref": "#/$defs/RowColType" } },
"definition": { "type": "array", "minItems": 1, "items": { "$ref": "<structure-schema>#/$defs/LocalisedValueType" } }
```

with `"required": ["dataflow"]` on the pivot table and `"required": ["definition"]`
on the term.

### 5b. Representation decides the value schema

A property carries at most one representation. In the examples below, "value
schema" means the schema you then wrap — or not — according to rule 5a.

**No representation** → `{"type": "string"}`. A value property with no restriction
on its data type or facets is described by its id and cardinality alone:

```json
{ "id": "note", "minOccurs": 0, "maxOccurs": 1 }
```
```json
"note": { "type": "string" }
```

**`format`** → a value in its *lexical* form, so still `{"type": "string"}`, with
the facets translated:

| Facet | Generate |
| --- | --- |
| `minLength` / `maxLength` | `minLength` / `maxLength` |
| `pattern` | `pattern` (anchored with `^` and `$`) |
| `dataType`, `minValue`, `maxValue`, `decimals` | a `pattern` matching the lexical form and range |

Because values are strings, numeric bounds cannot use JSON Schema's `minimum`:

```json
{ "id": "level", "minOccurs": 0, "maxOccurs": 1, "format": { "dataType": "Integer", "minValue": 0 } }
```
```json
"level": { "type": "string", "pattern": "^(0|[1-9][0-9]*)$" }
```

If a range is awkward to express as a regular expression, generate the looser
pattern and leave the exact bound to the system's own validation — schema
validation is not expected to catch everything.

**`format` with `isMultiLingual: true`** → each value is a `LocalisedValueType`,
an object with `locale` and `value`. Such a property is normally left unbounded so
it can carry one value per language, in which case rule 5a makes it an array:

```json
{ "id": "definition", "format": { "dataType": "String", "isMultiLingual": true } }
```
```json
"definition": {
    "type": "array",
    "minItems": 1,
    "items": { "$ref": "<structure-schema>#/$defs/LocalisedValueType" }
}
```

which matches instance content of:

```json
"definition": [
    { "locale": "en", "value": "The total monetary value of all final goods and services …" },
    { "locale": "fr", "value": "La valeur monétaire totale de tous les biens et services finaux …" }
]
```

Note this is *not* the `name`/`names` convention used by the standard SDMX
artefacts: because a multilingual property may itself repeat, the language travels
with each value.

**`customType`** → a `$ref` to the type you generated for it in section 3:

```json
{ "id": "rows", "minOccurs": 0, "customType": "RowColType" }
```
```json
"rows": { "type": "array", "minItems": 1, "items": { "$ref": "#/$defs/RowColType" } }
```

**`reference`** → the value is the URN of the referenced artefact. Pick the
tightest type in the structure schema:

| `reference` holds | Generate |
| --- | --- |
| one class with a matching reference type | that type, e.g. `<structure-schema>#/$defs/DataflowReferenceType` |
| several classes | `anyOf` of their reference types |
| a class with no dedicated reference type, or `Any` | `<structure-schema>#/$defs/urn` |

```json
{ "id": "dataflow", "maxOccurs": 1, "reference": ["Dataflow"] }
```
```json
"dataflow": { "$ref": "<structure-schema>#/$defs/DataflowReferenceType" }
```

and, for a property that may point at either of two classes:

```json
{ "id": "source", "maxOccurs": 1, "reference": ["Dataflow", "DataStructure"] }
```
```json
"source": {
    "anyOf": [
        { "$ref": "<structure-schema>#/$defs/DataflowReferenceType" },
        { "$ref": "<structure-schema>#/$defs/DataStructureReferenceType" }
    ]
}
```

**`indirectReference`** → the value is an identifier, not a URN:

```json
{ "id": "dimension", "maxOccurs": 1, "indirectReference": { "targetClass": "Dimension", "context": "dataflow" } }
```
```json
"dimension": { "$ref": "<structure-schema>#/$defs/idType" }
```

Whether that identifier actually resolves against the artefact named by `context`
— here, whether `AGE` really is a dimension of the dataflow in the instance's
`dataflow` field — cannot be expressed in JSON Schema. Leave it to the system.

### 5c. Mutually exclusive sets

Each set becomes one `not`/`required` clause on the containing object:

```json
"mutuallyExclusive": [ ["headingCode", "headingText"] ]
```
```json
"not": { "required": ["headingText", "headingCode"] }
```

Read it as "it must not be true that both are present". For a set of more than two
members you need one clause per pair, since `required` means *all of*. A set of
`["a", "b", "c"]` becomes:

```json
"allOf": [
    { "not": { "required": ["a", "b"] } },
    { "not": { "required": ["a", "c"] } },
    { "not": { "required": ["b", "c"] } }
]
```

## 6. What is deliberately not generated

- **URNs of objects inside an instance.** The URN of a row, a glossary term and so
  on is derived from the instance and the object's own id, so it is not reported
  and the generated schema declares nothing for it. A row in an instance is just:

  ```json
  { "id": "AGE_COL", "name": "Age", "dimension": "AGE" }
  ```

  and not `"urn": "urn:sdmx:org.sdmx.infomodel.csd.imf.PivotTableRow=OECD:POP_SEX_AGE(1.0.0).AGE_COL"`,
  even though that is the URN the object has. The instance itself, being
  maintainable, may still carry its own URN in `links` in the usual way.
- **Ordering.** JSON object members are unordered, and property order in a CSD
  carries no meaning in JSON.
- **Uniqueness of ids** among sibling identifiable objects, and **resolution** of
  indirect references. Both are the system's job.

# Extending the SDMX JSON Schema

The generated schemas each validate one instance object. To validate a whole
message you need a small wrapper that applies the official schema to the message
and the right generated schema to each instance. `csd_validation-schema.json` in
this folder is a worked example, holding two CSDs.

A system composes this file at runtime from the CSDs it currently knows about,
and regenerates it whenever that set changes. It is the JSON counterpart of
`csd_validation.xsd` in the SDMX-ML samples, which does the same job with one
`xs:import` per known CSD.

It has two parts, combined with `allOf` so that both apply to the same message:

**Part one** references the official structure schema:

```json
{ "$ref": "https://json.sdmx.org/2.2/sdmx-json-structure-schema.json" }
```

**Part two** navigates to `data` → `customStructures` → `items` and adds one
`if`/`then` per known CSD:

```json
{
    "if": {
        "properties": {
            "customStructureDefinition": {
                "const": "urn:sdmx:org.sdmx.infomodel.csd.CustomStructureDefinition=IMF:PIVOT_TABLE(1.0.0)"
            }
        },
        "required": ["customStructureDefinition"]
    },
    "then": { "$ref": "urn:sdmx:org.sdmx.infomodel.csd.CustomStructureDefinition=IMF:PIVOT_TABLE(1.0.0)" }
}
```

Five points are worth understanding before you write your own:

- **The conditionals are combined with `allOf`, not `oneOf`.** Each is evaluated
  independently, and an `if` that does not match simply contributes nothing.
  `oneOf` would require exactly one branch to succeed, which breaks the moment a
  message contains instances of two different CSDs — each instance would fail the
  branch belonging to the other one.
- **`"required": ["customStructureDefinition"]` inside the `if` is essential.**
  JSON Schema's `properties` says nothing about fields that are absent, so
  without it an instance with no definition reference would satisfy the `const`
  vacuously and match *every* branch at once.
- **Unknown definitions are allowed through.** An instance referencing a CSD you
  do not hold matches no `if`, so only part one applies to it. This is the lax
  behaviour, and it is a feature: you do not need every agency's CSDs to process a message.
- **The `const` and the `$ref` are the same string.** Because a generated schema
  is identified by the URN of the CSD it came from, the value you match on is the
  value you dispatch to. Composing this file is then just a loop over the
  definitions the system holds.
- **The version is part of the key.** Because the URN includes `(1.0.0)`,
  instances of different versions of the same definition can appear in one
  message and each is routed to its own generated schema.

Because the dispatch reads `customStructureDefinition`, that field must be present
on every instance you want checked in depth. It is mandatory in any case —
`CustomStructureInstanceType` requires it, and a reader cannot resolve the
definition without it — but it is worth being aware that an instance which omits
it is not merely invalid, it is also unrouted, so its content goes unchecked. The
instance's *own* URN plays no part in this and need not be present.

# Examples

Two custom structure definitions, one of each `base`, with the derived schema and
a conforming instance for each. They are the JSON equivalents of the samples in
`Custom Structure Definition` in the [sdmx-ml](https://github.com/sdmx-twg/sdmx-ml)
repository, so the two formats can be compared side by side.

| File | What it is |
| --- | --- |
| `pivot_table_csd.json` | A CSD with `base: Maintainable`, defining a pivot table layout over a dataflow. Shows all four property representations, an `extendsType` chain (`SliceType` extends `RowColType`) and a `mutuallyExclusive` set. |
| `pivot_table_1.0.0-schema.json` | The schema derived from it, following the rules above. |
| `pivot_table_instance.json` | A pivot table maintained by OECD, conforming to the IMF definition. |
| `custom_item_scheme_csd.json` | A CSD with `base: ItemScheme`, defining a glossary. Shows the reserved `items` property, a multilingual property, and a custom type that references itself to build a hierarchy. |
| `glossary_1.0.0-schema.json` | The schema derived from it. |
| `glossary_instance.json` | A glossary maintained by ECB, conforming to the IMF definition. |
| `csd_validation-schema.json` | The wrapper described in the previous section, covering both definitions. |

To validate the two instances in full, run them against
`csd_validation-schema.json` with any JSON Schema draft 2019-09 validator. The
two generated schemas are identified by URN, so register them with the validator
from these files rather than expecting it to fetch them; the
`https://json.sdmx.org/2.2/...` references to the official structure schema
resolve to `schemas/structure_message/` in this repository. Running them against
`../../../schemas/structure_message/sdmx-json-structure-schema.json` instead
checks the maintainable parts and the reference to the definition only — a useful
way to see the difference the derived schemas make.
