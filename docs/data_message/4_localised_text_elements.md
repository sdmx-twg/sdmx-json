# Localised text elements

**Localised best-language-match text strings (static properties matched through
"Lookup"):**

The first best language match according to the user’s preferred language choices
expressed through the HTTP content negotiation (Accept-Language header
parameter) is to be provided for each localised text element. The message does
however not indicate the returned language per localised text element.

This language matching type is called "Lookup", see
<https://tools.ietf.org/html/rfc4647#section-3.4>.

??? example

    ```json
        "name": "Frequency",
    ```

**Localised text objects (variable properties matched through "Filtering"):**

All available language matches according to the user’s preferred language
choices expressed through the HTTP content negotiation (Accept-Language header
parameter) is to be provided for each localised text element.

This language matching type is called "Filtering", see
<https://tools.ietf.org/html/rfc4647#section-3.3>.

??? example

    ```json
        "names": { "en": "Frequency",
            "fr": "Fréquence" },
    ```

The localised text object needs to be present whenever the related localised
best-language-match text strings is present, and especially whenever a localised
text is mandatory in the SDMX Information model. Note that localised text (and
the knowledge about the locale) is mandatory in structure messages when
artefacts are being submitted for storage to a registry or to other databases.
The localised text object is important for use cases where multiple languages
are required or where the information on the language used is required.

In case that there is no language match for a particular localisable element, it
is optional to:

- return the element in a system-default language or alternatively to not return
  the element
- indicate available alternative languages for the element's maintainable
  artefact through links to underlying localised resources

**It is recommended to indicate all languages used anywhere in the message for
localised elements through http Content-Language response header (languages of
the intended audience) and/or through a “contentLanguages” property in the meta
tag.** The main language used can be indicated through the “lang” property in
the meta tag.
