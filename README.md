Sia API
-------
>`Version 2 Draft 1 9/11/2015`
> 
>
Reference:

>- http://blog.octo.com/en/design-a-rest-api/
>- http://labs.omniti.com/labs/jsend
>- http://daringfireball.net/projects/markdown/

TODO:
> - Wallet API
> - Block Explorer
> - Statistics
> - Consensus

Authentication
------
>[JWT Authentication](http://blog.brainattica.com/restful-json-api-jwt-go/)

Authenication required with **siad** to access API.

`<Header Example>`

TODO: Continue research if this is the best solution

Errors
---
>[Error Specification](http://apigee.com/about/blog/technology/restful-api-design-what-about-errors)

HTTP errors availalbe
200, 201, 304, 400, 401

TODO: Show examples or include examples in each API section?

**Response**
```
HTTP error 401

{  
    status : "error",
    code : "401",
    message : "File not found in path specified.",
    object : "filePath",
    info : "api_url"
 }
 ```

`GET /v2/client`
----

Returns details about the status of the client.

**Parameters**

`None`

**Example JSON Response**
```
{  
    status : "success",
    data : {
         "version" : 0.3.3, 
         "renting" : true, 
         "mining" : false,  
         "hosting" : false,
         "latest-version" : 0.4.2,
         "payment-address" : "<example_sc_address>",
         "peers" : 8,
         "hosts" : 109,
         "files" : 44,
         "contracts" : 103,
         "SC" : 0,
         "polo-sc-price" : 0.00000103,
         "uptime" : "<time_in_seconds>"
    }
 }
 ```

`POST /v2/configure`
---
Set client configuration

**Parameters**

| Name  | description  | required  | default | type |
|:---|:-------|:---|:---|:---|
| address  | Your SC address  | y  |   | string |
| minFileSizeBytes  | File size in bytes (1024)  | n | 1 GB (1024 MB)  | int64 |
| price | Price in SC you wish to sell  | y  | no default | int64,long |
| minDurationBlocks |  <need_description> | n | 250 | |
| maxDurationBlocks  | <need_description>  | n |   |  |
| windowsSize  | <need_description>  |   | n |  |

**JSON**
```
{ 
    "address" : "<example_sc_address",
    "minFileSizeBytes" : 0,
    "price: {
                "key" : "price_in_sc_per_gb",
                "value" : 0 }
    "price_in_sc_per_gb" : 0,
    "price_in_btc_per_gb" : 0.00000111,
    "duration" : {
                "durationType" : "week",
                "durationValue" : 5}
    "maxDurationBlocks" : 5000,
    "windowsSize" : 0,
    "collateral" : 0,
    "maxParallelUpload" : 4,
    "maxParallelDownload" : 4
    }
 }
 ```

**Response**

```
{  
    status : "success",
    message : "Renter configuration updated successfully",
    data : {
    }
 }
```

`GET /v2/configure`
----
TODO: Verify parameters

**Parameters**

>`none`

**JSON Response**
```
{ 
    status : "success",
    data : {
        "address" : "<example_sc_address",
        "minFileSizeBytes" : 0,
        "price: {
                    "key" : "price_in_sc_per_gb",
                    "value" : 0 }
        "price_in_sc_per_gb" : 0,
        "price_in_btc_per_gb" : 0.00000111,
        "duration" : {
                    "durationType" : "week",
                    "durationValue" : 5}
        "maxDurationBlocks" : 5000,
        "windowsSize" : 0,
        "collateral" : 0
    }
 }
 ```

`GET /v2/hosts`
-----
Returns all hosts

**Parameters**

| Name  | description  | required  | default | type |
|:---|:---|:---|:---|:---|
| rpp | Records Per Page | y | 20 | int |
| page | Current Page | y | 1 | int |
| status | Filters active and/or inactive hosts | n | all | string |


```
{ 
    "rpp" : 50,
    "page" : 1,
    "status" : "all"
}
```

***Response***

```
{ 
    status: "success"
    data : [
        { "IP": "192.168.1.1", "port" : "9982", "trustLevel" : 103, "price_sc" : 255 },
        { "IP": "192.168.1.2", "port" : "9982", "trustLevel" : 2, "price_sc" : 1 },
        { "IP": "192.168.1.3", "port" : "9982", "trustLevel" : 445, "price_sc" : 15 }
    ]
}
```


`PUT /v2/hosts`
----
Adds new host

**Parameters**

**Example**

**Response**

`GET /v2/files`
-----
Returns details for all files you have access to.

**Parameters**

| Name  | description  | required  | default | type |
|:---|:---|:---|:---|:---|
| rpp | Records Per Page | y | 20 | int |
| page | Current Page | y | 1 | int |
| status | downloading / uploading / deleted | n | all | string |

```
{ 
    "rpp" : 50,
    "page" : 1,
    "status" : "all"
}
```
**Response**
| Name  | description   |  | type |
|:---|:---|:---|:---|
| uniqueFileId | Records Per Page |  | string |
| siaFileName | Current Page |  | string |
| TxRxBytes | The original file name |  | string |
| fileSizeInBytes | The original file name |  | string |
| fileType | The file extention |  | string |
| status | avalable,downloading,uploading,not found |  | string |
| originalFilePath | The original file path |  | string |
| owner | SC address of original owner |  | string |
| cost | cost in SC |  | uint64 |
| blocksRemaining | Block Remaining until contract expiration |  | int |
| autoRenewContract | Automatically renews contract when contract expires |  | boolean |
***JSON***
```
{ 
    status : "success",
    data : {
        "files" : [
            { "uniqueFileId" : "a93as9fa09th", "siaFileName" : "", "originalFileName" : "",
            "TxRxBytes" : 0, "fileSizeInBytes" : 100000, "fileType" : "jpg",
            "status" : "avalable,downloading,uploading,not found", "originalFilePath: "",
            "owner" : "sc_address", "cost" : 0, "blocksRemaining" : 2000, "autoRenewContract" : true
            },
            {}
            ]
    }
 }
```

`POST /v2/file`
----
Uploads a file (adds to the upload queue)

Parameters

```
{ 
    "siaFileName" : "this_is_a_picture.jpg",
    "originalFileName" : "me_in_paris.jpg",
    "fileType" : "jpg",
    "bytesUploaded" : 0,
    "fileSizeInBytes" : 100000,
    "filePath: "c:\files",
    "delayUpload" : true,
    "delayMinutes" : 5,
    "maxPrice" : "price_in_sc",
    "minDuration" : 1000,
    "maxDuration" : 5000
 }
```

***Response***

```
{ 
    status : "success",
    uniqueFileName : "a93as9fa09th"
 }
 ```


`DELETE /v2/file`
---
Deletes a specific file from the Sia blockchain.

***Parameters***

```
{ "uniqueFileId" : "a93as9fa09th"}
```

***Response***

```
{ 
    status : "success",
    message : "File deleted successfully"
 }
```

```
{ 
    status : "error",
    message : "File not found"
 }
```

`GET /v2/file`
----
Returns details for specific file.

>siaPath : if specified, downloads .sia file to share with others.
downloadPath : Path where file will be downloaded. 
keepPath : Overrides downloadPath and uses original path.
download : Downloads files. 

Parameters

```
{ 
    "uniqueFileId" : "a93as9fa09th",
    "siaASCII" : "",
    "siaFullPath" : "c:\folder\file_name.sia",
    "download" : true,
    "downloadPath" : "",
    "keepOriginalPath" : true
 }
```

Response

Same as /v2/files but with one line files

`UPDATE /v2/file`
----
Updates a specific files detail within the renter.

Parameters

```
{ 
    "uniqueFileId" : "a93as9fa09th",
    "siaFileName" : "paris.jpg"
 }
```

Response

```
{ 
    success : "true",
    data : {
        "files" : [
            { "uniqueFileId" : "a93as9fa09th",
            "siaFileName" : "",
            "originalFileName" : "",
            "TxRxBytes" : 0,
            "fileSizeInBytes" : 100000,
            "fileType" : "jpg",
            "status" : "avalable | downloading | uploading | not found | ",
            "originalFilePath: "c:\files",
            "owner" : "sc_address",
            "cost" : 0,
            "blocksRemaining" : 2000
            },
            {}
            ]
    }
 }
```





