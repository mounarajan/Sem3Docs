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

This API covers data across a wide range of product categories including pet supplies, electronics and vehicles, to name a few. Your application is, however, likely to require data from only a handful of categories. 

The category tree, addressed in this section, allows you to narrow your searches to only those product categories that interest you, thereby giving you the benefit of **relevant search results** and **low API query response times**.

### The Category Tree 

All products in the Semantics3 database are provided with a category ID (**cat_id**) parameter which identifies the nature of the product. These categories are laid out in the form of a multi-level category tree, with each category possessing a parent, a child, or both. 

Here is a segment of the tree from the “Electronics” branch. The number provided in brackets is the unique cat_id that identifies the given category name.

<pre><code>1.  Electronics (13658)
    2.  Accessories & Supplies (16055)
    2.  Camera & Photo (20773)
    2.  Television & Video (399)
    2.  Portable Audio & Video (17761)
    2.  Service & Replacement Plans (979)
    2.  Security & Surveillance (1124)
    2.  Car & Vehicle Electronics (18623)
    2.  Electronics Warranties (1061)
    2.  Computers & Accessories (4992)
        3.  External Data Storage (10562)
        3.  Desktops (4672)
        3.  External Components (2870)
        3.  Tablets (23042)
        3.  Printers (12091)
          4.  Laser Printers (23702)
          4.  Inkjet Printers (12151)
          4.  Dot Matrix Printers (23905)
          4.  All-In-One Printers (24768)
          4.  Photo Printers (1202)
          4.  Plotters (16398)
        3.  Scanners (14047)
        3.  Webcams (5858)
        3.  Servers (2534)
        3.  Video Projectors (13932)
        3.  Networking Products (3235)
        3.  Netbooks (23130)
        3.  Warranties & Services (23681)
        3.  PDAs, Handhelds & Accessories (13918)
        3.  Monitors (12460)
        3.  Computer Components (22208)
        3.  Game Hardware (5467)
        3.  Laptops (12855)
        3.  Computer Accessories (5660)
    2.  eBook Readers & Accessories (10100)
    2.  GPS & Navigation </code></pre>

In this example, “Electronics” is the highest parent category, those with an index of “2” are immediate children, those with an index of “3” are grand children and so on.

As mentioned, each product is tagged with a cat_id, which may lie at any level of the tree; the lower down the tree a given cat_id of a product, the more specific the categorization of that product. 

### Using “cat_id” [IMPORTANT] 

Let’s say your interest in the API only concerns electronics products; by including the following filter, you can restrict your API responses to the “Electronics” field:

<pre><code>GET https://api.semantics3.com/v1/products?q={"cat_id":13658,"name":"NAME"}</code></pre>

where the ID 13658 refers to “Electronics”. The “name” parameter, used here as a placeholder, is one of the several query parameters that will be discussed in detail later in the document.

This filter will ensure that the search responses are limited to products that either:

- Have **exactly** the specified cat_id, or,

- Have a cat_id that is a **child** of the specified cat_id (i.e., in this example, all children of “Electronics” including all the fields listed in the sample category tree above).

Mosts queries to the Semantics3 products API require that you supply a cat_id field. Refer to this [update](https://www.semantics3.com/docs/#update-1-upc-lookups-91) for details.	

Currently, you can only search amongst one category of products at a time; we will be introducing the ability to search amongst multiple categories in future versions of the API. If you’d like urgent beta access to this functionality, please contact us at [api@semantics3.com](mailto:api@semantics3.com).

### Category Tree Endpoint 

Just as “Electronics” is identified by cat_id “13658″, every category in the category is associated with a unique cat_id; these IDs can be determined by querying the category tree, in one of the following three ways:

Specify the **name** of the category that you’re interested in and obtain a list of similarly named categories:

<pre><code>GET https://api.semantics3.com/v1/categories?q={"name":NAME}</code></pre>

If you’d like to view category details associated with a **specific cat_id**:

<pre><code>GET https://api.semantics3.com/v1/categories?q={"show_cat_id":CAT_ID}</code></pre>

Retrieve all the immediate children associated with a given **parent category**; this can help in moving down the category tree or simply in better understanding the context of a particular category:

<pre><code>GET https://api.semantics3.com/v1/categories?q={"parent_cat_id":PARENT_CAT_ID}</code></pre>

The very first level of the tree can be obtained by setting PARENT_CAT_ID to 1 in the query above. This query will return IDs of the following categories.

<pre><code>1.   Appliances (18892)
2.   Arts, Crafts & Sewing (20717)
3.   Baby Products (21995)
4.   Beauty (13157)
5.   Books (12597)
6.   Clothing & Accessories (17366)
7.   Electronics (13658)
8.   Footwear (8551)
9.   Grocery & Gourmet Food (18203)
10.  Health & Personal Care (17534)
11.  Home & Kitchen (12908)
12.  Industrial & Scientific (23767)
13.  Jewelry (4798)
14.  Mobile Phones (915)
15.  Movies & TV (15532)
16.  Music (18735)
17.  Musical Instruments (24271)
18.  Office Products (9205)
19.  Patio & Garden (4702)
20.  Pet Care (6411)
21.  Software (10539)
22.  Sporting Equipment (2717)
23.  Tools & Home Improvement (21949)
24.  Toys & Games (16279)
25.  Vehicles (934)
26.  Video Games (11932)</code></pre>

The categories shown above lie at the top of the category tree. All other categories in the tree are children of one of these categories (hence, if you wish to keep your product searches as broad as possible, specify the cat_id of one of the above categories in your searches). In order to explore the entire tree step-by-step, execute the query above with parent_cat_id as 1, extract the cat_ids of each of the children and recursively iterate through the tree.

*Note*: The cat_id “1″ is a pseudo category used for the purpose of displaying the first level of the category tree and should not be used for requests made using the primary API endpoint.

# 03 **Metadata Fields**

### Skeleton of a Typical Response 

The following is a skeleton of a typical response obtained from the Semantics3 Products API. 

<pre><code>{
   "cat_id":"<CAT_ID>",
   "category":"<CATEGORYNAME>",
   "sem3_id":"<SEM3_ID>",
   "name":"NAME",
   "upc":"<UPC>",
   "weight":"<WEIGHT>",
   "updated_at":"<UPDATED_AT>",
   "brand":"<BRAND>",
   ......,
   ......,
   ......,
   ......,
   ......,
   "sitedetails":
   [
      {
         "sku":"<SKU>",
         "latestoffers":
         [
            {
               "currency":"<CURRENCY>",
               "id":"<OFFER_ID>",
               "price":"<PRICE>"
            },
            ......,
            ......,
            ......
         ]
        ......,
        ......
      }
   ]
}</code></pre>

The response skeleton shows only some of the fields that the API returns, such as “brand”, “sitedetails”, “sem3_id” and more; this portion of the documentation details **all possible metadata fields** that can be found in JSON responses provided by this API.

Most of the fields discussed in the following sections double up as **searchable query parameters**, i.e., they are both found as JSON fields in the API responses and can be supplied in the API request string to form queries. For example, the “brand” field shown in the response skeleton above can also be used as a query parameter in this manner:

<pre><code>GET https://api.semantics3.com/v1/products?q={"cat_id":13658,"brand":"Toshiba"}</code></pre>

### Key Notes 

- All parameters (including those provided in the example column of the table below) are to be encapsulated into a single URL encoded JSON string appended to the Products API endpoint, i.e., https://api.semantics3.com/v1/products.
- **Every query** to the products endpoint must contain [the “cat_id” field](https://www.semantics3.com/docs/#using-cat_id-34). **UPDATE 1**: This requirement has been relaxed for lookups with unique identifier fields (“UPC”, “EAN” and “sem3_id”), i.e., if one of these fields is provided, cat_id need not be provided. **UPDATE 2**: cat_id is not required for [free-text searches](https://www.semantics3.com/docs/#update-6-free-text-search-118) or for [cat_id independent searches](https://www.semantics3.com/docs/#update-7-more-cat_id-independent-fields-122).
- The **searchable** column in the tables shown below specifies whether or not the referenced field doubles up as a searchable query parameter field. If **Y***, then the field can be specified in the search string and if **N***, it should not and is only a descriptive field in the response text.
- The **query behavior** column in the tables below describes how the referenced field is processed internally when supplied as a search query parameter. String fields are wired to either return **exact** matches or **approximate** (queries are tokenized and then ranked by degree of relevance) matches. Integer fields support either **exact** matches or **range queries** (range queries are described in detail [here](https://www.semantics3.com/docs/#range-queries-41)).

### Section 1 (Universal Fields) 

The fields listed in this section are universal to all instances of the product, i.e., fields such as name, description, height and weight do not vary on the basis of who is selling the product and where the product is being sold.

The following fields can be used in API queries in this form:

<pre><code>GET https://api.semantics3.com/v1/products?q={"FIELDNAME":"VALUE","cat_id":"CAT_ID"}</code></pre>

where FIELDNAME refers to the field name and VALUE is your query input. The cat_id field must always be provided (Update: Unless [UPC, EAN, sem3_id](https://www.semantics3.com/docs/#update-1-upc-lookups-91), [free-text search](https://www.semantics3.com/docs/#update-6-free-text-search-118) or a [cat_id independent field](https://www.semantics3.com/docs/#update-7-more-cat_id-independent-fields-122) is involved). 

| Field Name  | Description  | Data Type    | Searchable | Query Behavior | Sample Query Snippet |
|:-----------|:------------|:------------| :---------| :-------- | :-------------- |
| *sem3_id*        |         	Internal Semantics3 ID of the product. You can also lookup multiple “sem3_id”s in one go as described in this [update](https://www.semantics3.com/docs/#update-4-lookup-multiple-sem3_ids-101).  |     String     | Y | Exact | {“sem3_id”: “1xXNQo9RkGkiymkoGegICq”} |
| *brand* | Brand name of the product. | String | Y | Approximate | {“brand”:”Toshiba”} |
| *cat_id* | Category ID of the category to which the product is assigned. Query by this cat_id to retrieve similar products. | String | Y | Refer to the [Categories section](https://www.semantics3.com/docs/#using-cat_id-34) for details. | - |
| *category* | Name of the category with which the product is associated. This is purely a text search, and unlike searches on “cat_id”, does not search among child nodes in the category tree. Oftentimes, this field provide more specificity than the category names available through the category tree. (Refer to the [Categories section](https://www.semantics3.com/docs/#using-cat_id-34) for specifics about cat_id searches). | String | Y | Approximate | {“category”:”television”} |
| *color* | Color of the product. | String | Y | Approximate | {“color”:”green”} |
| *created_at* | Time at which this product entry was made in Semantics3’s database. | Unix Timestamp | Y | Range | {“created_at”: {“gt”:1325397600}} |
| *description* | Detailed description of the product. | String | Y | Approximate | {“description”:”brand new laptop replacement screen”} |
| *ean* | 13-digit International Article Number (EAN) of the product. International Standard Book Numbers (ISBN) are also displayed against the ean field, since ISBN-13 is a subset of EAN. The “ean” field is included for quick and intuitive reference; we recommend that you utilize the “gtins” field, also described in this table, as far as possible. | String | Y | Exact | {“ean”:”5053178147751″} |
| *features* | Features of the product, in hash (key-value) form. Values are always in scalar form, except when the key is “blob; all unstructured features data is stored as a value of the blob key in either array or scalar form. | String (Hash) | Y | Approximate | {“features”:”aspherical”} |
| *gtins* | 14-digit Global Trade Item Numbers (GTINs) associated with the product. UPCs (GTIN-12) and EANs (GTIN-13) are subsets of the GTIN-14 system. A product may have multiple UPC/EAN/GTIN IDs associated with it; while the “upc” and “ean” fields display any one of these IDs for quick use, the GTIN field displays all associated IDs in an array. A search against the “gtins” field is equivalent to a search against the “upc” or “ean” field. | String (Array) | Y | Exact | {“gtins”:”00812942011958″} |
| *geo* | Country to which you would like to limit your results. [Only two values](https://www.semantics3.com/docs/index.php/section/updates/update-9-geocountry-support/) are valid for this field: “usa” (default) and “uk”. At the moment, each product is linked to only one country; however, this field is returned as an array to accommodate the future possibility of a one product to many countries relationships. | String (Array) | Y | Exact | {“geo”:”usa”} |
| *height* | Height of the product (in millimeters). | Double | Y | Range | {“height”:{“gt”:250}} |
| *images* | Array of URLs pointing to images of the product. These images are hosted on Semantics3′s servers. | String (Array) | N | - | - |
| *images_total* | Number of image URLs contained in the images array. Use this field if you wish to restrict your responses to products that have your desired number of associated image URLs.| Integer | Y | Range | {“images_total”:{“gte”:2}} |
| *length* | Length of the product(in millimeters).| Double | Y | Range | {“length”:{“lt”:250}} |
| *manufacturer* | Manufacturer of the product.| String | Y | Approximate |  	{“manufacturer”:”Atlas”} |
| *model* | Model of the product.| String | Y | Approximate |  	{“model”:”Satellite”} |
| *mpn* | Manufacturer part number of the product.| String | Y |  	Exact | {“mpn”:” 5053178147751″} |
| *name* | Name of the product.| String | Y | Approximate | {“name”: “TOSHIBA L655D-S5076RD Laptop”} |
| *offers_total* | **DEPRECATED** Total number of offers recorded for the given product, across all websites. An “offer” is a snapshot of the price of the product at a particular time. Offers vary on fields such as seller, price, availability and condition. This API (Products API) returns only a limited number offers in each response; the offers_total field doesn’t reflect this number, rather, it represents the total number of offers that have been gathered over time for the given product, all of which can be accessed through the [offers endpoint](https://www.semantics3.com/docs/#offers-price-histories-104). Additional details are provided in the [Section 2](https://www.semantics3.com/docs/#section-2-sitedetails-fields-38).**DEPRECATED**| Integer | Y | Range | {“offers_total”:{“gte”:3}} |















 













 
