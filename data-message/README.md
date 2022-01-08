# SDMX-JSON Data Message

As one of the standard SDMX data exchange formats, SDMX-JSON Data Message is 
a JSON (JavaScript Object Notation) based data exchange message format 
designed for and therefore responding to the main use case of data discovery 
and visualisation on the web.

JSON is a widely used standardised lightweight data-interchange format. 
SDMX-JSON with a simple and dense but generic message format that supports 
different types of data structures will ease writing software for the web 
that easily consumes SDMX data resources. It should allow for an efficient 
client-server exchange with few roundtrips, small message sizes and fast 
parsing. The format may be used in a wide variety of programming languages 
and application programs. Testing and development of the format has focused 
on JavaScript applications running in web browsers. 

The current SDMX-JSON Data Message format 2.0.1 supports the SDMX 3.0 Information Model 
and works together with the SDMX 3.0 RESTful Web Services API for data queries. 

The SDMX-JSON format is streamable from a supporting web service in order to 
limit the amount of server resources. 

See:
- docs: for the descriptive documentation
- samples: for message samples
- tools: for the normative JSON schema usable for validation purposes
