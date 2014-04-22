# Table of Contents
### 01. The Basics
- About

- Primary API Endpoint

- Sample Call

- REST-ful API

- JSON Responses

- 1-legged OAuth 1.0 Authentication

- Throttling

- URL Encoding

- Error Codes

- Notes about Common Mistakes

### 02. Categories
- Importance
- The Category Tree
- Using “cat_id” [IMPORTANT]
- Category Tree Endpoint

### 03. Metadata Fields
- Skeleton of a Typical Response
- Key Notes
- Section 1 (Universal Fields)
- Section 2 (Sitedetails Fields)
- Section 3 (Category Specific Fields)
- Nomenclature

### 04. Useful Query Parameters

- Range Queries
- Pagination: Limit
- Pagination: Offset
- Sorting

### 05. Offers (Price) Histories

- Offers Endpoint
- Metadata Fields

### 06. Updates

- Update 1: Unique ID Lookups
- Update 2: Exclude Irrelevant Results
- Update 3: Include All Variations
- Update 4: Lookup Multiple “sem3_id”s
- Update 5: Prettify Time Fields
- Update 6: Free-Text Search
- Update 7: More “cat_id” Independent Fields
- Update 8: “url”, “site” and “fields” queries
- Update 9: Geo/Country Support
- Update 10: Logical AND/OR Operators [Beta]

# 01 **The Basics**

### About

This page contains detailed technical documentation for **Version 1** of the **Semantics3 Products API**. Refer to our [Quick Start Guide](http://www.semantics3.com/quickstart) if you are just getting started.

This API gives you comprehensive access to data across millions of products; use this API to build sophisticated models to track prices across a wide variety of products, easily build shopping comparison engines, enrich your e-commerce stores with rich metadata and [more](http://www.semantics3.com/usecases).

### Primary API Endpoint 

All calls the Semantics3 Products API in production environments should be made as HTTP GET requests to: 

  <pre><code>GET https://api.semantics3.com/v1/</code></pre>

The authentication, throttling mechanisms and examples in this document assume usage of the above endpoint. 

We also provide a **testing/development endpoint** to make it easy for you to get familiar with the API; the usage of this endpoint as well as associated authentication and throttling mechanisms is described in the [Quick Start Guide](https://www.semantics3.com/quickstart) and **is not covered in this document**.

### Sample Call 

The following is a sample Semantics3 API request:


   <pre><code> GET https://api.semantics3.com/v1/products?q={"cat_id":13658,"brand":"Toshiba","model":"Satellite"}</code></pre>

Query fields and mechanisms are described in detailed in further sections.

### REST-ful API 

Semantics3’s API is designed to be REST-ful; its URLs follow predictable patterns and responses carry appropriate HTTP response codes, among other features that characterize the REST architectural style.

### JSON Responses 

All responses from this API, including error messages, will be returned in **JSON** format.

### 1-legged OAuth 1.0 Authentication 

Semantics3 uses the [1-legged OAuth model](https://github.com/Mashape/mashape-oauth/blob/master/FLOWS.md#oauth-10a-one-legged), as described in the IETF OAuth 1.0 Protocol ([RFC 5849](http://tools.ietf.org/html/rfc5849)), as its authentication mechanism.

All requests must be authorized with a valid **API key** and **API secret key**. Always be careful never to publish your API secret key as it uniquely identifies your account and will allow anyone with your API key to gain access to your account.

Please refer to the [OAuth Community Site](http://oauth.net/code/) to access libraries in the programming language of your choice. For any questions, please contact us at [support@semantics3.com](mailto:support@semantics3.com) and we’ll get back to you right away.

### Throttling 

Every application is subject to both IP based and API key based concurrent request throttling.

**IP Throttling**: If a single IP makes more than 30 requests a second, new requests from the API will be dropped and HTTP 5XX error codes will be sent.

**API Key Throttling**: API key based requests are throttled according to the quota available for your plan. If you make more requests than your daily quota, you will begin to receive HTTP 5XX error codes. If you intend to make more requests than your plan allows for, please upgrade to a higher plan or purchase add-on packs through the billing page in your dashboard.

### URL Encoding 

All requests made to the Semantics3 API **must** be URL encoded. 

*Note*: Examples shown on this page have not been URL encoded for the purpose of readability.

### Error Codes 

Semantics3 uses conventional [HTTP status codes](http://en.wikipedia.org/wiki/List_of_HTTP_status_codes) to indicate success or failure of API requests. In general, codes in the **2xx** range indicate success, codes in the **4xx** range indicate an error that resulted from the provided information (e.g. a required parameter was missing), and codes in the **5xx** range indicate an error with Semantics3′s servers.

You are most likely to come across one of the following errors:

- **400 (Bad Request):** Your JSON request was either of invalid format or was not URL encoded.
- **401 (Unauthorized):** You failed to authenticate.
- **429 (Too Many Requests):** Your request was throttled.
- **500 (Internal Server Error):** Error with Semantics3′s servers.

The response text for error conditions typically contains two fields in the supplied JSON response, **code** and **message**, that describe in further detail the error that has occurred.

### Notes about Common Mistakes

- All query filters are to be sent in the form of a JSON string.
- All requests must be signed with the appropriate OAuth keys.
- All requests must be URL encoded.
- All string fields must be enclosed within double quotes.
- Complex queries are to be constructed by separating multiple query parameters with commas, as is the standard JSON convention.

# 02 **Categories**

### Importance


 













 
