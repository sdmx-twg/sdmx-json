# Introduction to SDMX-JSON Data Message 2.0.0

Let's first start with a brief introduction of the SDMX information model.

In order to make sense of some statistical data, we need to know the concepts
associated with them. For example, on its own the figure 1.2953 is pretty meaningless,
but if we know that this is an exchange rate for the US dollar against the euro on 
23 November 2006, it starts making more sense.

There are two types of concepts: dimensions and attributes. Dimensions, when combined,
allow to uniquely identifying statistical data. Attributes on the other hand do not help
identifying statistical data, but they add useful information (like the unit of measure
or the number of decimals). Dimensions and attributes are known as "components".

The measurement of some phenomenon (e.g. the figure 1.2953 mentioned above) is known as an
"observation" in SDMX. Sometimes, observations can also have several measures, e.g. an 
estimated value can be complemented with the value corresponding to the upper confidence 
limit and the value corresponding to the lower confidence limit of the esimation.
Observations, when exchanged, are grouped together into a "data set". However, there
can also be an intermediate grouping. For example, all exchange rates for the US dollar
against the euro can be measured on a daily basis and these measures can then be
grouped together, in a so-called "time series". Similarly, you can group a collection of
observations made at the same point in time, in a "cross-section" (for example,
the values of the US dollar, the Japanese yen and the Swiss franc against the euro at a
particular date). Of course, these intermediate groupings are entirely optional and you
may simply decide to have a flat list of observations in your data set.

The SDMX information model is much richer than this limited introduction;
however the above should be sufficient to understand the sdmx-json format. For
additional information, please refer to the [SDMX documentation](../../index.md).

Samples, tools and other SDMX-JSON resources are available in the public 
[Github repository](https://github.com/sdmx-twg/sdmx-json).

Before we start, let's clarify a few more things about this guide:

- New fields may be introduced in later versions. Therefore
    consuming applications should tolerate the addition of new fields with ease.
- The ordering of fields in objects is undefined. The fields may appear in any order
    and consuming applications should not rely on any specific ordering. It is safe to consider a
    nulled field and the absence of a field as the same thing.
- Not all fields appear in all contexts. For example response with error status messages
    may not contain fields for data, dimensions and attributes.