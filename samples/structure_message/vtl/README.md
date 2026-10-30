# VTL samples

These samples illustrate the VTL (Validation and Transformation Language) artefacts of the SDMX-JSON structure message: the mappings between VTL and SDMX, the rulesets, the user defined operators, the transformation schemes and the VTL executions. They are the JSON counterparts of the SDMX-ML samples `samples/VTL Transformations/` of [sdmx-twg/sdmx-ml#77](https://github.com/sdmx-twg/sdmx-ml/pull/77), and keep the content of their SDMX-ML sources: the same artefacts, identifiers, references and VTL statements. Only whitespace is normalised, for example the indentation of the ruleset and operator definitions and the line breaks of a few descriptions of the deprecated samples. As JSON has no comments, the explanations given by the XML comments of the SDMX-ML files are included below.

- The agencies used by the samples 4 to 9 are `SDMX` and `EXAMPLE`. Both are declared in the agency scheme `SDMX:AGENCIES` that each of these messages includes, and every URN of these messages resolves to an artefact or to an item of the same message.
- In the samples 4 to 9, `meta.prepared` is `2026-10-29T00:00:00Z`, the receiver is `SDMX`, the sender is `EXAMPLE` (`SDMX` in `vtl-sample-5.json`) and the transformation schemes use VTL 2.2 (`vtlVersion` "2.2").
- The prefix `deprecated-` marks the samples that use constructs deprecated in SDMX 3.2.

## vtl-sample-4.json

SDMX-ML source: `VTL_Sample_4.xml`.

Example 1 of the section "Retrieving the data of the input VTL Data Sets" of the SDMX Technical Notes: the difference between two dataflows, with a VTL execution that retrieves the data of the inputs.

- The transformation scheme `EXAMPLE:TS_DIFF` calculates the difference between the dataflows `EXAMPLE:DF_A` and `EXAMPLE:DF_B` (`ds_diff <- ds_a - ds_b`). Its VTL mapping scheme `EXAMPLE:VTL_MAP` maps the inputs `ds_a` and `ds_b` and the output `ds_diff` to dataflows through VTL dataset mappings (`vtlDatasetMappings`), whose `isInput` property gives the direction of the mapping. Under the Basic mapping, the dimensions and the time dimension of the data structure definition become VTL Identifiers and `OBS_VALUE` a VTL Measure. `ds_a` and `ds_b` are inputs, whose definitions and data are obtained from SDMX; `ds_diff` is an output, whose data are converted into SDMX.
- The alias of the output is `ds_diff`, not the `diff` of the Technical Notes: `diff` is a VTL keyword, and the example of the Technical Notes was changed accordingly.
- The VTL execution `EXAMPLE:EXEC_DIFF` (`vtlExecutions`) says how the VTL engine retrieves the data of the inputs, so that `TS_DIFF` can be executed on the basis of its SDMX definition. It is maintained independently of `TS_DIFF`.
    - The query extraction (`queryExtractions`) of `ds_a` is a query to a web service implementing version 1.5 of the SDMX REST API (`sdmxApiVersion` "1.5"): the VTL engine builds the query for `DF_A`, the dataflow referenced by the mapping of `ds_a`. The query parameter `key` is fixed (`A.USA.` selects the annual data of the USA for any `INDICATOR`), `startPeriod` has a default value (2015) that can be overridden at run time, and `endPeriod` has no default value, so it must be provided at run time.
    - The URL extraction (`urlExtractions`) of `ds_b` is the analogous retrieval, written as a URL template (`urlTemplate`): the tokens in curly braces are replaced by the values of the query parameters having the same names. `endPeriod` has no default, so it must be provided at run time.
- If `startPeriod=2020` and `endPeriod=2023` are provided at run time for both inputs, the VTL engine retrieves the data of `ds_a` and `ds_b` through the following queries:

    ```text
    https://sdmx.example.org/rest/data/EXAMPLE,DF_A,1.0/A.USA.?startPeriod=2020&endPeriod=2023
    https://sdmx.example.org/rest/data/EXAMPLE,DF_B,1.0/A.USA.?startPeriod=2020&endPeriod=2023
    ```

## vtl-sample-5.json

SDMX-ML source: `VTL_Sample_5.xml`.

Examples 2 and 3 of the same section of the Technical Notes: two VTL Data Sets mapped to the same data structure definition, and two VTL executions of the same transformation scheme, one by its maintenance agency `SDMX` and one by the agency `EXAMPLE` on its own data. The message is sent by `SDMX`, and the SDMX artefacts use semantic versions (`1.0.0`).

- The transformation scheme `SDMX:TS_POPRATIO` calculates the ratio between the male and the female population (`ratio <- pop_m [ sub SEX = "M" ] / pop_f [ sub SEX = "F" ]`).
    - `pop_m` and `pop_f` are distinct VTL Data Sets mapped to the same data structure definition, `SDMX:POP(1.0.0)` (`dataStructure` of the VTL dataset mappings): the mapping is many-to-many, and a VTL dataset mapping can reference a data structure definition.
    - The `sub` clauses drop the `SEX` Identifier, whose value is fixed in each input by the key of its data extraction. Without them, the division would not match any data points, because VTL composes the data points having the same values for all the Identifiers. The result has the Identifiers `REF_AREA`, `AGE` and `TIME_PERIOD` and the Measure `OBS_VALUE`, as the data structure definition of `SDMX:DF_POPRATIO`.
    - `OBS_VALUE` is an Integer with `minValue` 0 in the population data (VTL integer) and a Double with `minValue` 0 in the ratio (VTL number, as the division of two integers is a number). `TIME_PERIOD` is a GregorianYear (VTL date) with `startTime` 1950.
- The dataflow `EXAMPLE:DF_POP` of the agency `EXAMPLE` uses the data structure definition `SDMX:POP(1.0.0)` maintained by `SDMX`.
- The VTL execution `SDMX:EXEC_POPRATIO` is the execution by `SDMX` on a web service implementing version 2.1 of the SDMX REST API, which has data queries by data structure definition. As the two inputs reference the same URN, the run-time values are addressed to them through their aliases. Each input has its own fixed key (`*.M.*` and `*.F.*`) and a default filter on the time period (`c[TIME_PERIOD]` with the value `ge:2010`). With `c[TIME_PERIOD]=ge:2015` provided at run time for both inputs, the queries are:

    ```text
    https://sdmx.example.org/rest/data/datastructure/SDMX/POP/1.0.0/*.M.*?c[TIME_PERIOD]=ge:2015
    https://sdmx.example.org/rest/data/datastructure/SDMX/POP/1.0.0/*.F.*?c[TIME_PERIOD]=ge:2015
    ```

- The VTL execution `EXAMPLE:EXEC_POPRATIO` is the execution by `EXAMPLE` of the same transformation scheme on its own data, without copying the transformation scheme and its VTL mapping scheme. The `context` of its query extractions is its own dataflow `EXAMPLE:DF_POP(1.0)`, which uses the same data structure definition as the mapped artefact. The web service implements version 1.5 of the SDMX REST API, which has no data queries by data structure definition. Without run-time values, the queries are:

    ```text
    https://data.example.org/rest/data/EXAMPLE,DF_POP,1.0/.M.?startPeriod=2010
    https://data.example.org/rest/data/EXAMPLE,DF_POP,1.0/.F.?startPeriod=2010
    ```

## vtl-sample-6.json

SDMX-ML source: `VTL_Sample_6.xml`.

A VTL 2.2 program using a VTL Value Domain mapped to a codelist and a viral propagation.

- A VTL codelist mapping (`vtlCodelistMappings`) maps the VTL Value Domain `cl_area` to the codelist `SDMX:CL_AREA(2.0)`, whose codes are the values of the Value Domain. The codelist is partial (`isPartial` true): only some codes are included, to keep the sample smaller.
- `REF_AREA` is not coded in the data structure definition `EXAMPLE:DSD_X(1.0)`: it is an Alpha string of two letters (`minLength` and `maxLength` 2), so the transformation checks its values against the codelist with `in cl_area` (`ds_invalid <- ds_x [ filter not ( REF_AREA in cl_area ) ]`).
- The ruleset scheme `EXAMPLE:RS_X` contains a viral propagation (VTL 2.2) defined on the Variable `OBS_STATUS` (`rulesetType` "viral", `rulesetScope` "variable"). It makes the SDMX data attribute `OBS_STATUS` a viral VTL Attribute, and tells the VTL operators how to combine its values: a missing value prevails over an estimated one, which prevails over a normal one. The transformation scheme `EXAMPLE:TS_AREACHK` references the ruleset scheme, so the `+` operator of `ds_total <- ds_x + ds_y` keeps `OBS_STATUS` in the result, combining the values of the inputs through the viral propagation `OBS_STATUS_PROP`, and the result has the data structure definition of `EXAMPLE:DF_TOTAL`. Without the viral propagation, `OBS_STATUS` would be a non-viral VTL Attribute and the result would not have it.
- The sample has no VTL execution: it is optional, and without it the data of the inputs are provided to the VTL engine at run time.

## vtl-sample-7.json

SDMX-ML source: `VTL_Sample_7.xml`.

A VTL 2.2 program using the three types of VTL rulesets. The ruleset scheme `EXAMPLE:RS_POP` contains:

- `DPR_VALUE`, a datapoint ruleset defined on the Variables `OBS_VALUE` and `OBS_STATUS` (`rulesetScope` "variable"), that is, on the components having these names in the VTL Data Sets to which it is applied, which need no mapping.
- `HR_BENELUX`, a hierarchical ruleset defined on the Value Domain `cl_area` (`rulesetScope` "valuedomain"), which a VTL codelist mapping maps to the codelist `EXAMPLE:CL_AREA(1.0)`: the values of the Value Domain are the codes of the codelist.
- `VP_OBS_STATUS`, a viral propagation (`rulesetType` "viral", introduced in VTL 2.2) defined on the Variable `OBS_STATUS`, which makes it a viral VTL Attribute: a missing value prevails over an estimated one, which prevails over a normal one.

The transformation scheme `EXAMPLE:TS_POP_CHECKS` validates the population data and calculates an aggregate:

- `check_datapoint(ds_pop, DPR_VALUE all_measures)` applies `DPR_VALUE` to `ds_pop` and returns, for each data point and rule (`ruleid`), the original measures, the boolean outcome of the rule (`bool_var`) and, when the rule is not satisfied, its `errorcode` and `errorlevel`. `OBS_STATUS` is kept, as it is a viral VTL Attribute.
- The data structure definition `EXAMPLE:DSD_POP_VALIDATION(1.0)` is that of the result of `check_datapoint` with the `all_measures` output (Basic mapping). The Identifier `ruleid` and the Measures `bool_var` (boolean), `errorcode` (string) and `errorlevel` (integer) added by the operator correspond to `RULEID`, `BOOL_VAR`, `ERRORCODE` and `ERRORLEVEL`, because the VTL regular names are case insensitive. The facets `maxLength`, `minValue` and `maxValue` restrict the values of the string and integer components.
- `hierarchy(ds_pop, HR_BENELUX rule REF_AREA)` calculates `BENELUX` as `BE + LU + NL` through `HR_BENELUX`, applied to the Identifier `REF_AREA`. The values of `OBS_STATUS` of the components are combined through the viral propagation `VP_OBS_STATUS`.

## vtl-sample-8.json

SDMX-ML source: `VTL_Sample_8.xml`.

A VTL 2.2 program using user defined operators, one of which invokes another.

- The user defined operator scheme `EXAMPLE:UDO_TRADE` defines `trade_balance`, `share` and `export_share`, which invokes `share`. The operators are defined on generic operands (the parameters), which are passed at the invocation.
- The transformation scheme `EXAMPLE:TS_TRADE` references the scheme and calculates the trade balance and the share of the exports in the trade from the dataflows of exports and imports.
- As `export_share` invokes `share`, the scheme contains both: the user defined operator schemes of a transformation scheme must contain all the operators used by its transformations, also indirectly.
- The amounts are Decimals with two decimals (VTL number), by ReportingTimePeriod (VTL time_period).

## vtl-sample-9.json

SDMX-ML source: `VTL_Sample_9.xml`.

VTL Data Sets whose names are written between single quotes in the VTL statements.

- The aliases `power` and `diff` coincide with VTL keywords (the `power` and `diff` operators), so the transformation scheme `EXAMPLE:TS_ENERGY` writes them between single quotes (`'diff' <- 'power' - consumption`), while the `vtlAlias` of their VTL dataset mappings contains the name without the quotes (a `vtlAlias` cannot contain single quotes), also when the VTL statements need them.
- `consumption` is a regular VTL name, which the statements can write with or without single quotes: one statement writes it without quotes and the other with quotes (`ratio <- 'power' / 'consumption' * 100`). Both forms denote the same VTL Data Set, and the VTL engines identify the mapped dataflow in both cases.
- The amounts are Decimals with one decimal (VTL number), by GregorianYearMonth (VTL date).

## Deprecated samples

`deprecated-vtl-sample-1.json` to `deprecated-vtl-sample-3.json` are the JSON counterparts of `deprecated-VTL_Sample_1.xml` to `deprecated-VTL_Sample_3.xml`. They use constructs deprecated in SDMX 3.2, which remain valid: the old VTL mappings (`vtlMappings`), the name personalisation and custom type schemes, and the references from the ruleset schemes to the VTL mapping schemes. Their representation in SDMX 3.2 is described in the section "Conversion of definitions based on deprecated constructs" of the chapter on the Validation and Transformation Language of the SDMX Technical Notes.

Like the samples 4 to 9, they keep the content of their SDMX-ML sources (only whitespace is normalised), which predate SDMX 3.2: the sender and the receiver are `SDMX`, `meta.prepared` is `2020-06-05T12:00:00.000Z`, the schemes use VTL 2.0, and there is no agency scheme, so the agency `SDMX` of their artefacts is not declared. Every URN of these messages resolves to an artefact or to an item of the same message.

### deprecated-vtl-sample-1.json

SDMX-ML source: `deprecated-VTL_Sample_1.xml`.

Illustrates the use of VTL rulesets to validate an SDMX dataflow. The validation process adds additional identifiers and measures to the resulting VTL dataset indicating, on a row by row basis, the validation rule applied and the outcome in terms of valid or invalid. If invalid, details of the error found and its severity are added. A second transformation takes the resulting dataset, filtering out high-severity validation errors, and persists the result back to an SDMX dataflow using VTL mappings.

Deprecated constructs it uses:

- The old VTL mappings (`vtlMappings`), with their mapping methods (`toVtlMapping` and `fromVtlMapping`, here `Basic` and `M2A`) and their mappings of concepts (`concept`).
- The name personalisation scheme (`namePersonalisationSchemes`), referenced by the `namePersonalisationScheme` property of the transformation scheme.
- The reference from the ruleset scheme to the VTL mapping scheme (`vtlMappingScheme` of the ruleset scheme).

Notes:

- The codelists are smaller than they would be: `SDMX:CL_SECTOR(1.0)` has no codes and `SDMX:CL_AREA(1.0)` has only some countries (all the EU countries would be added to cater for the `EU` code).
- The references of this sample all resolve within the message.

### deprecated-vtl-sample-2.json

SDMX-ML source: `deprecated-VTL_Sample_2.xml`.

Illustrates aggregation of an SDMX dataflow using VTL hierarchical rulesets, filtering and user defined operators. After the initial aggregation, the unaggregated observations are removed from the result dataset. A user defined operator is subsequently applied to calculate a percentage measure, with the final persistent result mapped from VTL to an SDMX dataflow.

Deprecated constructs it uses:

- The old VTL mappings (`vtlMappings`), with their mapping methods (`Pivot` and `Unpivot`) and their mappings of concepts (`concept`).
- The custom type scheme (`customTypeSchemes`), referenced by the `customTypeScheme` property of the transformation scheme.
- The reference from the ruleset scheme to the VTL mapping scheme (`vtlMappingScheme` of the ruleset scheme).

Notes:

- The codelist `SDMX:CL_AREA(1.0)` is partial (`isPartial` true): all the EU countries would be added to cater for the `EU` code, but were removed to keep the sample smaller.
- The references of this sample all resolve within the message.

### deprecated-vtl-sample-3.json

SDMX-ML source: `deprecated-VTL_Sample_3.xml`.

Illustrates the calculation of a GDP per capita dataset from an input mapped SDMX dataflow containing GDP and population indicators on a country by country basis.

Deprecated constructs it uses:

- The old VTL mappings (`vtlMappings`) with the sub space and the super space of the mapped dataflows (`toVtlSubSpace` and `fromVtlSuperSpace`).
- The references to the parts of the dataflows in the VTL statements (ordered concatenation notation, for example `'DF31/GDP.US'`).

Notes:

- The transformation scheme uses an SDMX specific naming convention that requires pre-processing before parsing the VTL expressions. As explained in the SDMX Technical Notes, the dataset identifiers of the sample are a shortcut of the VTL `sub` operator: `'DF31/GDP.US'` is equivalent to `DF31[sub INDICATOR=GDP, COUNTRY=US]`. This convention works combined with the mappings of the sample, which indicate the keys that are used for the dataflows. It is not safe to use outside an SDMX context, as the dataflows may have no meaning there.
- The codelist `SDMX:CL_AREA(1.0)` is partial (`isPartial` true): all world countries, regions and areas should appear in it, but were removed to keep the sample smaller.
- The references of this sample all resolve within the message.
