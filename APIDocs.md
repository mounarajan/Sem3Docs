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
| *offers_total* | &#124;**DEPRECATED**&#124; Total number of offers recorded for the given product, across all websites. An “offer” is a snapshot of the price of the product at a particular time. Offers vary on fields such as seller, price, availability and condition. This API (Products API) returns only a limited number offers in each response; the offers_total field doesn’t reflect this number, rather, it represents the total number of offers that have been gathered over time for the given product, all of which can be accessed through the [offers endpoint](https://www.semantics3.com/docs/#offers-price-histories-104). Additional details are provided in the [Section 2](https://www.semantics3.com/docs/#section-2-sitedetails-fields-38).&#124;**DEPRECATED**&#124;| Integer | Y | Range | {“offers_total”:{“gte”:3}} |
| *price* | each product, made available in the [sitedetails section](https://www.semantics3.com/docs/#section-2-sitedetails-fields-38) and through the [offers endpoint](https://www.semantics3.com/docs/#offers-price-histories-104). However, you may face the need to attribute a single quick-and-dirty representative price point to each product. This “price” is selected from among all the offers and list prices associated with the product across several sites using several rules refined over time. It can be used for sorting by price, easily filtering by price and more. The currency associated with this field is reflected in the “price_currency”. Note that if you are not on one of our premium plans, the source of the “price” field may not be visible to you, since you will not have access to all sitedetails entries and associated offers.| Double | Y | Range | {“price”:{“gte”:49.99}} |
| *price_currency* | Reflects the [ISO 4217](http://en.wikipedia.org/wiki/ISO_4217) currency value associated with the “price” field. This field has been added to resolve confusion over the “price” field. It is not query-able. Look to the “geo” field to restrict your searches to specific geographies.| String | N | - | - |
| *size* | Size of the product.| String | Y |  	Approximate | {“size”:”small”} |
| *upc* | 12-digit Universal Product Code (UPC) code of the product. The “upc” field is included for quick and intuitive reference; we recommend that you utilize the “gtins” field, also described in this table, as far as possible.| String | Y | Exact |{“upc”:”812942011958″} |
| *updated_at* | Time at which the data in this product response was last refreshed from its parent sources.| Unix Timestamp | N | Range | 	{“updated_at”: {“gt”:1325397600}} |
| *variation_id* | Often, the same product may be available in multiple colors, formats and sizes (this is particularly true of clothing and footwear). If a product returned by the API contains a ‘variation_id’ field, you can query by this field to retrieve all associated variations. Note, by default, unless variation_id is specified, responses to the products API contain only one variation of each product.| String | Y | Exact | 	{“variation_id”: “5VcVscjfRQoqCGiAIWqGoc”} |
| *weight* | Weight of the product(In milligrams).| Double | Y | Range | {“weight”:{“gt”:250,”lt”:500}} |
| *width* | Width of the product (in millimeters).| Double | Y | Range | {“width”:250} |

### Section 2 (Sitedetails Fields) 

The fields described in the [previous section](https://www.semantics3.com/docs/#section-1-universal-fields-37) are universal to the product, i.e., they do not change on the basis of who is selling the product and where the product is being sold.

**This section** tackles fields such as price and product availability, which are highly specific to the websites on which the product is being sold. These fields are organized under the “sitedetails” tag.

Here is an example of a sitedetails response:

<pre><code>"sitedetails": [
               {
                   "latestoffers": [
                       {
                           "currency": "USD",
                           "id": "3iNRIZGppAyGuyywOmkEqM",
                           "price": "7.95",
                           "firstrecorded_at": "2012-09-13T20:20:47Z",
                           "lastrecorded_at": "2012-09-15T12:36:54Z",
                           "seller": "Joe Shop"
                       },
                       {
                           "currency": "USD",
                           "id": "7iNEIZGuykEqMQywOmpAyG",
                           "price": "10.99",
                           "shipping" : "1.99",
                           "firstrecorded_at": "2012-11-07T10:00:23Z",
                           "lastrecorded_at": "2013-01-31T07:14:11Z",
                           "availability": "2 items in stock. Ships in 24 hours",
                           "condition": "New"
                       }
                   ],
                   "name": "amazon.com",
                   "offers_count": 2,
                   "recentoffers_count": 1,
                   "sku": "B0000CF3HB",
                   "url": "http://www.amazon.com/dp/B0000CF3HB"
               },
               {
                   "name": "newegg.com",
                   "latestoffers": [
                       {
                           ......,
                           ......,
                           ......
                       },
                       {
                           ......,
                           ......,
                           ......
                       }
                   ],
                   ......,
                   ......,
                   ......
               }  
           ]</code></pre>

Observe that the **sitedetails** tag is an array, each element of which identifies a particular e-commerce site (eg. amazon.com or newegg.com) on which the product has been listed. Further, observe that each sitedetails array element has a **latestoffers** field; this field is also an array and represents the most recent price “offers” for the referenced product on the referenced site.

**Part A** below tackles all fields that fall under the **sitedetails** tag, except for the **latestoffers** field, which will be dealt with in **Part B**.

### **Part A**

Fields listed in this section fall under the **sitedetails** tag and can queried in this form:

<pre><code>GET https://api.semantics3.com/v1/products?q={"sitedetails":{"FIELDNAME1":"VALUE1","FIELDNAME2":"VALUE2"},"cat_id":"CAT_ID"}</code></pre>

where FIELDNAME1 and FIELDNAME2 refer to specified field names and VALUE1 and VALUE2 are your query inputs.

where FIELDNAME1 and FIELDNAME2 refer to specified field names and VALUE1 and VALUE2 are your query inputs.

You may note that since sitedetails is represented as an array in product responses, each product contains multiple sitedetails elements. Hence, API query behaviour for queries made against this field are nuanced: for a given sitedetails query, the API returns return all products which contain atleast one site element that satisfies all of the FIELDNAME-VALUE filters specified in the query. If, for a given product, one site matches some of the FIELDNAME-VALUE filters and a second site matches the other FIELDNAME-VALUE filters then such a product will not be returned; atleast one site must match all query criteria.

| Field Name  | Description  | Data Type    | Searchable | Query Behavior | Sample Query Snippet |
|:-----------|:------------|:------------| :---------| :-------- | :-------------- |
| *geo* | &#124;**DEPRECATED**&#124; Approximate geographical location at which the referenced site sells its products. &#124;**DEPRECATED – Please refer to the “geo” field in [Section 1](https://www.semantics3.com/docs/#section-1-universal-fields-37)**&#124;| String | N | - | - |
| *listprice* |  	List price on the site. Typically, e-commerce sites present this value as the reference price against which a marked-down price is contrasted.| Double | Y | Range | {“sitedetails”:{“listprice”:{“gte”:569.99}}} |
| *listprice_currency* | Currency associated with the listprice. Currency codes are returned in [ISO 4217](http://en.wikipedia.org/wiki/ISO_4217) format.| String | Y | Exact | {“sitedetails”:{“listprice”:{“gte”:569.99}}} |
| *name* | Name of the website from which this data was obtained.| String | Y | Exact | {“sitedetails”:{“name”:”walmart.com”}} |
| *offers_count* | &#124;**DEPRECATED**&#124; Total number of “offers” recorded for the referenced site, each of which is available through the [offers endpoint](https://www.semantics3.com/docs/#offers-price-histories-104). “Offers” are explained in detail in Part B of this section; be sure to carefully read the note marked “IMPORTANT” in Part B. &#124;**DEPRECATED**&#124; | Integer | Y | Range | {“sitedetails”:{“offers_count”:{“gte”:2}}} |
| *recentoffers_count* | Number of “active” offers recorded when this site was last crawled. A value of “0″ indicates that the product is no longer available for purchase through that link. | Integer | Y | Range | {“sitedetails”:{“recentoffers_count”:{“lte”:2}}} |
| *sku* | Site SKU of the website from which this data was obtained. If one SKU has multiple variations associated with it, then this field returns the SKU provided by the website appended with an underscore and a variation specific identifier. | String | Y | Exact | {“sitedetails”:{“name”: “amazon.com”, “sku”: “B0000CF3HB”}} |
| *url* | URL pointing to the product page on the referenced site. | String | N | - | - |

*Note*: As with all queries in this API, queries made using the sitedetails fields can only be used to **search for relevant products**. They **do not shorten the product response string** to match your query.

For instance, the following query

<pre><code>GET https://api.semantics3.com/v1/products?q={"cat_id":13658,"sitedetails":{"name":"target.com"}</code></pre>

returns “Electronics” (cat_id 13658) products that are or were sold on target.com, but the response string will also contain sitedetails of all the other sites that sell each product. Functionality for restricting response strings is coming soon.

### **Part B**

Fields listed in this section fall under the **latestoffers** tag, which is an important child of the **sitedetails** tag.

Each offer in the **latestoffers** array represents the price of a particular product, at a given time, on a particular website and further characterized by fields such as availability and condition. These fields can be queried in this form:

<pre><code>GET https://api.semantics3.com/v1/products?q={"sitedetails":{"latestoffers":{"FIELDNAME1":"VALUE1"},{"FIELDNAME2":"VALUE2"}}, "cat_id":"CAT_ID"}</code></pre>

where FIELDNAME1 and FIELDNAME2 refer to specified field names and VALUE1 and VALUE2 are your query inputs.

You may note that since latestoffers is represented as an array in product responses, each product contains multiple latestoffers. Hence, API query behaviour for queries made against this field are nuanced: for a given latestoffers query, the API returns return all products which contain atleast one latest/recent offer that satisfies all of the FIELDNAME-VALUE filters specified in the query. If, for a given product, one offer matches some of the FIELDNAME-VALUE filters and a second offer matches the other FIELDNAME-VALUE filters then such a product will not be returned; atleast one offer must match all query criteria.

**IMPORTANT**: The latestoffers array is **capped to 3 entries per site**, i.e., for each website represented in the “sitedetails” array, only the latest 3 offers will be displayed and made available for querying. To retrieve all offers associated with a particular product, use the [offers endpoint](https://www.semantics3.com/docs/#offers-price-histories-104).

| Field Name  | Description  | Data Type    | Searchable | Query Behavior | Sample Query Snippet |
|:-----------|:------------|:------------| :---------| :-------- | :-------------- |
| *availability* | Status of the offer at the time at which it was recorded (e.g., available, out of stock, etc.).| String | Y | Approximate | {“sitedetails”:{“latestoffers”: {“availability”:”in stock”}}} |
| *condition* | The condition of the item that is on offer (e.g., new, refurbished, used, etc.). This field is set only if explicitly mentioned in the source of the data; if this field is empty, the product can be assumed to be of condition “new”.| String | Y | Approximate | {“sitedetails”:{“latestoffers”:{“condition”:”new”}}} |
| *currency* | Currency code associated with the specified price (and shipping price, if present). Currency codes are returned in [ISO 4217 format](http://en.wikipedia.org/wiki/ISO_4217).| String | Y | Exact | {“sitedetails”:{“latestoffers”:{“currency”:”USD”}}} |
| *id* | Unique ID associated with the referenced offer.| String | Y | Exact | {“sitedetails”:{“latestoffers”:{“id”: “1X8rXs6zJc8AisiUEGiM6Q1341220026″}}} |
| *price* | Price at which the product is sold at in the referenced offer. The price field is returned in standard denomination, i.e., all prices in USD are returned in dollars.| Double | Y | Range | {“sitedetails”:{“latestoffers”:{“price”:{“gt”:99.50}}}} |
| *firstrecorded_at* | Time at which this offer was first recorded in Semantics3′s database.| Unix Timestamp | Y | Range | {“sitedetails”:{“latestoffers”:{“firstrecorded_at”:{“gt”:1325397600}}}} |
| *lastrecorded_at* | Time at which this offer was last checked.| Unix Timestamp | Y | Range | {“sitedetails”:{“latestoffers”:{“lastrecorded_at”:{“gt”:1325397600}}}} |
| *seller* | Name of the seller who has put up this offer for the product. This field is particularly relevant to sites that offer the products of third-party sellers.| String | Y | Approximate | {“sitedetails”:{“latestoffers”:{“seller”:”BestBuy”}}} |
| *shipping* | Shipping price associated with this offer.| Double | Y |  	Range | {“sitedetails”:{“latestoffers”:{“shipping”:{“lte”:1.49}}}} |

*Note*: As mentioned in Part A, all sitedetails queries, including those associated with latestoffers, are only used to generate relevant responses; they do not shorten the product response string to match your query.

### Section 3 (Category Specific Fields) 

The fields listed in this section are **applicable only to some categories** in the category tree.

The table below contains an additional column – **relevant categories** – which identifies the categories to which the referenced field is applicable. The categories listed in this column can be derived by searching the tree with “parent_cat_id” set to 1, as demonstrated [here](https://www.semantics3.com/docs/#category-tree-endpoint-35).

*Note*: Each of the following fields is applicable not only to its referenced category/categories but also to **all of its children** in the category tree. For example, the “department” field (refer to the table below) can be used to query both products that are tagged to the “Clothing & Accessories” category (cat_id 17366) as well as to products that are tagged to categories that are children of the “Clothing & Accessories” category; a list of these children can be extracted by recursively [querying the category tree](https://www.semantics3.com/docs/#category-tree-endpoint-35) with parent_cat_id set to 17366. 

These fields can be used in API queries in this form:

<pre><code>GET https://api.semantics3.com/v1/products?q={"FIELDNAME":"VALUE","cat_id":"CAT_ID"}</code></pre>

where FIELDNAME refers to the field name and VALUE is your query input.

| Field Name  | Description | Relevant Categories | Data Type    | Searchable | Query Behavior | Sample Query Snippet |
|:-----------|:------------ | :------------|:------------| :---------| :-------- | :-------------- |
| *actor* | Actor(s) of the movie/TV show. Multiple values are separated by commas. | Movies & TV [15532] | String | Y | Approximate | {“actor”:”Sean Connery”} |
| *artist* | Arist(s) associated with the referenced music item. | Music [18735] | String | Y | Approximate | {“artist”:”Ravi Shankar”} |
| *author* | Author of the book. | Books [12597] | String | Y | Approximate | {“author”:”Tolkien”} |
| *department* | Department under which the product falls. | Baby Products [21995], Beauty [13157], Clothing & Accessories [17366] | String | Y | Approximate | {“department”:”mens”} |
| *director* | Director(s) of the movie/TV show. Multiple values are separated by commas. | Movies & TV [15532] | String | Y | Approximate | {“director”:”James Cameron”} |
| *format* | Format in which the product is delivered. | Books [12597], Movies & TV [15532], Music [18735], Video Games [11932] | String | Y | Approximate | {“format”:”cd”} |
| *genre* | Genre associated with the product. | Video Games [11932] | String | Y | Approximate | {“genre”:”Adventure Games”} |
| *label* | Record (label) associated with the item of music. | Music [18735] | String | Y | Approximate | {“label”:”Sony Music”} |
| *language* | Language in which the contents of the product are delivered. | Books [12597], Movies & TV [15532], Music [18735] | String | Y | Approximate | {“language”:”English”} |
| *operatingsystem* | Operating system of the product. | Mobile Phones [915], Software [10539] | String | Y | Approximate | {“operatingsystem: “Windows”} |
| *platform* | Platform (typically software) on which the product runs.| Software [10539], Video Games [11932] | String | Y | Approximate |  	{“platform”:”mac osx 10.7″} |
| *producer* | Producer(s) of the movie/TV show. Multiple values are separated by commas.| Movies & TV [15532] | String | Y | Approximate | {“producer”:”George Lucas”} |
| *publisher* | Publisher of the book.| Books [12597] | String | Y | Approximate | {“publisher”:”Oxford University Press”} |
| *runningtime* | Running time, in minutes, of the movie/TV show.| Movies & TV [15532] | String | Y | Approximate |  	{“runningtime”:{“lte”:100}} |
| *studio* | Studio associated with the movie/TV show.| Movies & TV [15532] | String | Y | Approximate | {“studio”:”Walt Disney”} |
| *writer* | Writer(s) of the movie/TV show. Multiple values are separated by commas.| Movies & TV [15532] | String | Y | Approximate | {“writer”:”Woody Allen”} |

### Nomenclature 

In grasping the various metadata fields associated with each product, you may find it useful to know the nomenclature that dictates the naming of these fields:

- Field names are always in lowercase.
- Field names that end with “_id” represent unique identifiers (e.g. sem3_id and cat_id).
- Field names that end with “_count” represent a local aggregate of the referenced item (e.g. recentoffers_count reflects the number of offers recorded in the most recent check on the given SKU).
- Field names that end with “_total” represent a global aggregate of the referenced item (e.g. offers_total reflects the total number of offers across all websites).
- Field names that start with “variation_” reference variations of the product (e.g. variation_id and variation_includeall).
- All other field names, regardless of the number of words they comprise, contain no underscores, spaces or other separators (e.g. sitedetails, latestoffers, etc.).

# 04 **Useful Query Parameters**

### Range Queries

Range queries support the execution of ‘>’, ‘>=’, ‘<’ and ‘<=’ functions on numerical fields. Ranges can be specified in queries in the following manner:

<pre><code>GET https://api.semantics3.com/v1/products?q={"FIELDNAME":{"RANGEPARAM":VALUE"},"cat_id":CAT_ID}</code></pre>

where **FIELDNAME** refers to the field name, **VALUE** is your query input and **RANGEPARAM** is one of the following:

- **gt** (greater than)
- **gte** (greater than or equal to)
- **lt** (lesser than or equal to)
- **lte** (lesser than or equal to)

For example, the following query returns all “Electronics” products that weigh more than 1000 milligrams and less than or equal to 5000 milligrams:

<pre><code>GET https://api.semantics3.com/v1/products?q={"weight":{"gt":1000,"lte":5000},"cat_id":13658}</code></pre>

If a range parameter is not specified for a numerical field, then an exact match will be carried out instead. For example, the following query returns all “Electronics” products with atleast one offer of exactly 10 USD.

<pre><code>GET https://api.semantics3.com/v1/products?q={"sitedetails":{"latestoffers":{"price":10,"currency":"USD"}}, "cat_id":13658}</code></pre>

### Pagination: Limit 

The number of products shown in your API response can be controlled by the limit query parameter. The **limit** field determines the number of products that will be shown per page for the given query. 

For example, the following query returns “Electronics” products with brand names that match “Toshiba”, 10 at a time:

<pre><code>GET https://api.semantics3.com/v1/products?q={"limit":10,"brand":"Toshiba","cat_id":13658}</code></pre>

Limit is set to its maximum value of **10 by default**.

### Pagination: Offset 

The **offset** keyword refers to the number of results that must be skipped in the response space before **limit** number of products are returned. 

For example, the following query:

<pre><code>GET https://api.semantics3.com/v1/products?q={"offset":15,"limit":10,"brand":"Toshiba","cat_id":13658}</code></pre>

returns elements 16-25 of all “Electronics” products that match the “Toshiba” brand name.

The offset parameter, which is assumed to be “0″ if not specified, is commonly used for the purpose of iteratively paginating through the body of responses.

### Sorting 

By default, products in your API response are sorted by a combination of relevance to your query string, and importance of the product. 

If you wish to **sort** by a specific field, you may do so in the following manner:

<pre><code>GET https://api.semantics3.com/v1/products?q={"sort":{"FIELDNAME":"SORTPARAM"},"cat_id":CAT_ID,"ADDITIONALFIELDNAME":"ADDITIONALFIELDVALUE"}</code></pre>

where **FIELDNAME** refers to the field name on which you wish to sort and **SORTPARAM** is one of the following:

- **asc** (ascending order)
- **dsc** (descending order)

**ADDITIONALFIELDNAME** and **ADDITIONALFIELDVALUE** are placeholders to demonstrate that atleast one query metadata field is necessary.

For example, the following query sorts its name search responses in ascending order of product name:

<pre><code>GET https://api.semantics3.com/v1/products?q={"sort":{"name":"asc"},"name":"Toshiba","cat_id":13658}</code></pre>

Note: Currently, sorting can only be performed along the **name** and **price** fields.

# 05 **Offers (Price) Histories**

### Offers Endpoint 

We now track and provide offer histories for the products in our database. Each product may contain multiple offer entries, differentiated by change in price over time, name of seller and website on which the product is sold and condition of the product. This data can be used to detect price changes patterns and monitor competitors, among other uses. 

Offer history data can be obtained by querying the offers endpoint as follows:

<pre><code>GET https://api.semantics3.com/v1/offers?q={"sem3_id":"4znupRCkN6w2Q4Ke4s6sUC"}</code></pre>

Here is an example of a response that can be obtained from the offers endpoint. 

<pre><code>"results" : [
    {
      "id": "792d8B2B1ckO4Mw8UyOqs6",
      "price": "42.09",
      "condition": "New",
      "firstrecorded_at": 1355077000,
      "lastrecorded_at": 1356038000,
      "sitedetails_name": "amazon.com",
      "sem3_id": "4znupRCkN6w2Q4Ke4s6sUC",
      "seller": "LFleurs Blooming Deals",
      "shipping" : "4.99",
      "currency": "USD",
      "availability": "Ships in 1-2 business days. Ships from TX, United States. Expedited shipping available."
    },
    {
      "id": "5X7DzxfvDEu06MO0CkUuM6",
      "price": "39.99",
      "firstrecorded_at": 1349398900,
      "lastrecorded_at": 1349398900,
      "sitedetails_name": "walmart.com",
      "sem3_id": "4znupRCkN6w2Q4Ke4s6sUC",
      "seller": "Walmart",
      "currency": "USD",
      "availability": "In stock"
    },
    {
      "id": "6Fro0ghqb2GoYs2Omo0k2A",
      "price": "39.95",
      "shipping" : "2.99",
      "firstrecorded_at": 1348654600,
      "lastrecorded_at": 1348654600,
      "sitedetails_name": "frys.com",
      "sem3_id": "4znupRCkN6w2Q4Ke4s6sUC",
      "seller": "Frys",
      "currency": "USD",
      "availability": "Shipping: In stock, ships same Business Day"
    },
   ......,
   ......,
   ......,
   ......,
   ......,
   ......,
   ......,
  ]</code></pre>

*Note*: Offer responses are automatically sorted by recency. Currently, they cannot be sorted by any other field.

The frequency at which a product’s offers data is updated is determined by our internal product importance ranking algorithm. If you’re interested in a group of products which haven’t been updated recently, please contact us at [support@semantics3.com](mailto:support@semantics3.com) and we’ll take necessary action.

### Metadata Fields 

All queries to the offers endpoint must contain the “sem3_id” of the product whose offers are the be retrieved. Thus, all queries must be in this form:

<pre><code>GET https://api.semantics3.com/v1/offers?q={"sem3_id":"SEM3_ID","FIELDNAME":"VALUE"}</code></pre>

where FIELDNAME and VALUE refer to metadata field names and values used to filter the response string. Each of these metadata fields is described in the following table.

You may note that the fields below are highly similar to the “latestoffers” metadata fields described in Part B of [this section](https://www.semantics3.com/docs/#section-2-sitedetails-fields-38).

| Field Name  | Description  | Data Type    | Searchable | Query Behavior | Sample Query Snippet |
|:-----------|:------------|:------------| :---------| :-------- | :-------------- |
| *availability* | Status of the offer at the time at which it was recorded (e.g., available, out of stock, etc.).  | String | Y | Approximate | {“availability”:”in stock”} |
| *condition* | The condition of the item that is on offer (e.g., new, refurbished, used, etc.).  | String | Y | Approximate | {“condition”:”new”} |
| *currency* | Currency code associated with the specified price. Currency codes are returned in [ISO 4217](http://en.wikipedia.org/wiki/ISO_4217) format. This field restricts responses to products that are sold natively in USD; it **does not** perform currency conversion.  | String | Y | Exact | {“currency”:”USD”} |
| *id* | Unique ID associated with the referenced offer.  | String | Y | Exact |  	{“id”: “1X8rXs6zJc8AisiUEGiM6Q1341220026″} |
| *price* | Price of the product in the referenced offer. The price field is returned in standard denomination, i.e., all prices in USD are returned in dollars. | Double | Y | Range | {“price”:{“gt”:99.50}} |
| *firstrecorded_at* | Time at which this offer was first recorded in Semantics3′s database. | Unix Timestamp | Y | Range | {“firstrecorded_at”:{“gt”:1325397600}} |
| *lastrecorded_at* | Time at which this offer was last checked. All responses to the offers endpoint are sorted by this field. This field can be of use if you wish to restrict your responses only to offers gathered ‘x’ hours in the past. | Unix Timestamp | Y | Range | {“lastrecorded_at”:{“gt”:1325397600}} |
| *seller* | Name of the seller who has put up this offer for the product. While querying, you may choose to provide multiple seller names in the form of an array to restrict your offers to any one among a specific group of sellers. | String/String Array | Y | Approximate | {“seller”:["LFleurs","Frys","Walmart"]} |
| *shipping* | Shipping price associated with the referenced offer. | Double | Y | Range | {“shipping”:{“lte”:1.49}} |
| *sitedetails_name* | Name of the site on which the offer was detected. | String | Y | Exact | {“sitedetails_name”:”frys.com”} |
| *sku* | List of SKUs associated with the offer. This field is an array since multiple SKUs from the same domain may share the same offer; rather than create duplicates, we compress the entries into one deduplicated offer. “sku” was introduced in the early half of 2013, hence some of the older offers may not carry a SKU field. Since the offers endpoint also returns “sitedetails_name”, “sku” and “sitedetails_name” together can be used as an effective canonicalized replacement for URL. | String (Array) | Y | Exact | {“sku”:”17472709″} |

# 06 **Updates**

### Update 1: Unique ID Lookups 

For product searches, cat_id need not be specified if one of **UPC**, **EAN**, **sem3_id** or **variation_id** is provided.

Previously, for example, UPC lookups could be run as follows:

<pre><code>GET https://api.semantics3.com/v1/products?q={"upc":"883974958450","cat_id":"13658"}</code></pre>

Now, UPC lookups can be run without specifying cat_id:

<pre><code>GET https://api.semantics3.com/v1/products?q={"upc":"883974958450"}</code></pre>

However, we encourage you to provide cat_id, when available, for quicker lookups.

### Update 2: Exclude Irrelevant Results 

While searching for a specific group of products (e.g. LCD Televisions), you may come across related products (e.g.: LCD Television stands) which aren’t of interest to you and reduce the relevance of your API results. While choosing a more pertinent cat_id from the category tree solves this problem in most cases, oftentimes, you may choose to employ exclusion filters.

Exclusion filters, which can currently only be applied to the product **name** field, exclude products that match specified words from your search results. Thus, where previously products could only be queried by name as follows:

<pre><code>GET https://api.semantics3.com/v1/products?q={"cat_id":"13813","name":"LCD"}</code></pre>

they now also support queries in the form of:

<pre><code>GET https://api.semantics3.com/v1/products?q={"cat_id":"13813","name":{"include:"LCD","exclude":"stand protector accessory kit"}}</code></pre>

The above query will include all product entries that have the word “LCD” in their names but don’t contain the words “protector”, “acccessory”, “kit” or “stand”. Note, when an **exclude** filter is applied, the original name query is to be placed inside the **include** tag.

### Update 3: Include All Variations 

Products may have several associated variations, differentiated by fields like color, size etc. For instance, one model of a shoe (the product) may be available in different size-color combinations (its variations). By default, product API queries (except lookup queries – those that involve [UPC, EAN, sem3_id](https://www.semantics3.com/docs/#update-1-upc-lookups-91), variation_id, [free-text search](https://www.semantics3.com/docs/#update-6-free-text-search-118) or [cat_id independent fields](https://www.semantics3.com/docs/#update-7-more-cat_id-independent-fields-122)) return only one random variation of each matching product.

For example, this query returns only 4 types of shoes for the specified brand, even though each shoe is offered in several colors and sizes:

<pre><code>GET https://api.semantics3.com/v1/products?q={"cat_id":"4378","brand":"Bibi Mimi"}</code></pre>

You may, however, choose to retrieve all color and size combinations (variations) of each of these shoes as follows:

<pre><code>GET https://api.semantics3.com/v1/products?q={"cat_id":"4378","brand":"Bibi Mimi","variation_includeall":1}</code></pre>

In this way, the **variation_includeall** parameter can be used to include product variations in your query results.

### Update 4: Lookup Multiple “sem3_id”s 

You can now retrieve data for upto 10 “**sem3_id”**s in one API query as follows:

<pre><code>GET https://api.semantics3.com/v1/products?q={"sem3_id":["2NnNAztqoGeoQGeSya0y4K", "0xzFQX9Ss8ecMwkMy0C8Ui", "1XgtmTtMgWswmYaGS6Kgyc"]}</code></pre>

This type of query can be particularly handy if you choose to cache “sem3_id”s and make live queries from your website/app to this API.

### Update 5: Prettify Time Fields 

By default, time fields are represented in unix timestamp format (e.g. {“updated_at” : 1357125766}).

Now, you can choose to retrieve all time fields in ISO string format (e.g. {“updated_at” : “2013-01-02T11:22:46Z”}) by setting the “**isotime”** parameter to 1 as follows:

<pre><code>GET https://api.semantics3.com/v1/products?q={"cat_id":13658,"isotime":1}</code></pre>

### Update 6: Free-Text Search 

Search the database, as you would a search engine:

<pre><code>GET https://api.semantics3.com/v1/products?q={"search":"Apple Macbook"}</code></pre>

Filter search results by products from a specific **brand** as follows:

<pre><code>GET https://api.semantics3.com/v1/products?q={"search":"polo shirts","brand":"ralph lauren"}</code></pre>

You can also specify a list of **cat_id** numbers that you’d like to restrict your search to (e.g. 18792 => men’s clothing and 17114 => boys’ clothing):

<pre><code>GET https://api.semantics3.com/v1/products?q={"search":"polo shirts","brand":"ralph lauren","cat_id":[17114,18792]}</code></pre>

Free-text searches are fundamentally “OR” queries, i.e., they will return all products that contain atleast one of the “search” terms, though results most relevant to your search will be ranked the highest.

Note: Free-text searches can be combined with all of the fields listed in [update 7](https://www.semantics3.com/docs/#update-7-more-cat_id-independent-fields-122). They can also be combined with **cat_id**, as shown above, and can be sorted by **price**. All other functionality is restricted.

### Update 7: More “cat_id” Independent Fields 

The cat_id field has, thus far, been mandatory unless one of the following fields is specified (refer to [update 1](https://www.semantics3.com/docs/#update-1-upc-lookups-91) and [update 6](https://www.semantics3.com/docs/#update-6-free-text-search-118)):

- **upc**
- **ean**
- **sem3_id**
- **variation_id**
- **search**

Following several requests from the developer community, the following fields have been added to this list:

- **name**
- **brand**
- **manufacturer**
- **model**
- **mpn**
- **price**
- **color**
- **size**
- **gtins**

For example, henceforth, you can search by **brand** without having to specify a cat_id:

<pre><code>GET https://api.semantics3.com/v1/products?q={"brand":"Dell"}</code></pre>

*Note*: cat_id independent searches can be combined with [unique ID fields](https://www.semantics3.com/docs/#update-1-upc-lookups-91) and [free-text search](https://www.semantics3.com/docs/#update-6-free-text-search-118), or they can be sorted by **price**. To avail of any other functionality, you must specify a **cat_id**.

### Update 8: “url”, “site” and “fields” queries 

Introducing three handy API queries:

**URL Lookup**: Enter e-commerce URLs in any form to retrieve associated product(s). Your input URLs need not be cleaned or canonicalized. [More details](http://blog.semantics3.com/competitors-price-histories-and-product-details-using-just-a-purchase-url/). Additional note: while the query returns only those product(s) associated with the queried URL, the sitedetails field in your response might contain additional URLs associated with the product. In other words, while the URL search returns products linked to the queried URL, it also returns additional URLs associated with these matched products.

<pre><code>GET https://api.semantics3.com/v1/products?q={"url":"http://www.costcentral.com/proddetail/Apple_MacBook_Pro_with_Retina_display/MD212LLA/11804567"}</code></pre>

**Site Search**: Retrieve the product catalogue of a specific domain. If you can’t find the domain you’re looking for, [contact us](mailto:contactus@semantics3.com). Additional note: while the query returns only those product(s) associated with the queried site name, the sitedetails field in your response might contain additional sites associated with the product. In other words, while the site search returns products linked to the queried site, it also returns additional sites associated with these matched products.

<pre><code>GET https://api.semantics3.com/v1/products?q={"site":"costcentral.com"}</code></pre>

**Fields Restriction**: Restrict API results to only those fields that interest you, by specifying an array of desired fields. sem3_id and cat_id are included in all results by default.

<pre><code>GET https://api.semantics3.com/v1/products?q={"site":"costcentral.com","fields":["name","brand"]}</code></pre>

### Update 9: Geo/Country Support 

We’re introducing data from geographical regions other than the United States. For this reason, we’ve introduced the **geo** parameter, which can be used to specify your geographical region of choice.

For instance, this query will return Apple products and associated offers for the United Kingdom: 

<pre><code>GET https://api.semantics3.com/v1/products?q={"brand":"apple","geo":"uk"}</code></pre>

The **geo** field currently supports the following values:

- “usa” -> United States of America [Default]
- “uk” -> United Kingdom

We recommend that you explicitly include the geo tag in all queries that you make to the API.

### Update 10: Logical AND/OR Operators [Beta] 

The logical operators && and || can be used with any field that has “Query Behavior” marked as “Approximate”. You can now run queries as follows:

<pre><code>GET https://api.semantics3.com/v1/products?q={"name":"polo && ralph","color":"black || white"}</code></pre>

Currently, we do no support compound/nested logical statements.































 













 
