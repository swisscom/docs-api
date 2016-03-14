# SmartMessaging API

## Table of Contents
1. [Overview](#overview)
2. [API - Resources](#API)
	1. [/messaging/v1/carrierinfo/](#/messaging/v1/carrierinfo/)
	2. [/messaging/v1/shortname/shortids/{shortId}](#/messaging/v1/shortname/shortids/{shortId})
	3. [/messaging/v1/shortname/providers/{providerName}](#/messaging/v1/shortname/providers/{providerName})
	4. [/messaging/v1/sms/](#/messaging/v1/sms/)
	5. [/messaging/v1/sms/{messageId}](#/messaging/v1/sms/{messageId})
	6. [/messaging/v1/tokenvalidation/](#/messaging/v1/tokenvalidation/)
	7. [/messaging/v1/tokenvalidation/{msisdn}/{token}](#/messaging/v1/tokenvalidation/{msisdn}/{token})
3. [API - Error Handling](#ERROR)


<div id='overview'/>
## Overview

The SmartMessaging API provides different resources around the topic mobile messaging. 

**CarrierInfo**
Use this resource to resolve extended carrier information for a given phone number. Is it a Swisscom number or does the number depends to a domestic or international carrier? The resource also provides information about customers contract (prepaid or postpaid).

**ShortName**
Swisscom provides short numbers as alias for a real MSISDN (e.g. for marketing phone services). This resource allows you to resolve detail information abut the provider, who owns the given short number. It is also possible resolve all short numbers for a provider.

**Sms**
Resource to send a SMS. It is also possible to receive a status notification.

**Token Validation**
Simple  resource to validate a phone number by token.  To validate a number a well specified SMS message with included textual token will be sent to your customers mobile. Now the customer is able forward this token to your service (HTML from, Mail, Callcenter). After a quick validation by this resource you can be sure that phone number depends to your customer.

<div id='API'/>
## API - Resources

<div id='/messaging/v1/carrierinfo/'/>
### /messaging/v1/carrierinfo/

#### **get** *(secured)*: Get carrier information and contract information about a number. 
Either a ClientID is used for authentication or an OAuth Token.    
If a ClientID is used only the carrier info is displayed (swisscom or not).
If an OAuth Token is used the contract type is displayed as well (prepaid or postpaid).
In OAuth case, the scope **read-carrier-info** is needed.

##### **Request** 
###### **Headers**

 - client_id (Id to authenticate client)
 - Accept (application/json)

###### **Query-Parameters**

- tel (the phone number to get info on.)

##### **Success Responses**
###### **200 Response (application/json)** 
<pre><code>{
  "tel": "+41712345678",
  "carrier": "swisscom",
  "contractType": "postpaid"
}</code></pre>
<pre><code>{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "id": "https://api.swisscom.com/messaging/v1/sms/messaging-sms-post.json",
    "title": "Carrierinfo Schema",
    "type": "object",
    "properties": {
        "tel": {
            "type": "string",
            "description": "The telephone number for which the carrier info is resolved"
        },
        "carrier": {
            "type": "string",
            "description": "carrier of the above telephone number"
        },
        "contractType": {
            "type": "string",
            "enum": [
				"unknown",
				"prepaid",
				"postpaid"
            ],
            "description": "prepaid or postpaid; this is an optional element"
        }
    },
    "required": [
        "tel", "carrier"
    ]
}</code></pre>

##### **Error Responses**

[400](#400), [401](#401), [403](#403), [404](#404), [405](#405), [406](#406), [500](#500), [503](#503), 

<div id='/messaging/v1/shortname/shortids/{shortId}'/>
### /messaging/v1/shortname/shortids/{shortId}

#### **get**: Get provider information which stands behind a short number

##### **Request** 
###### **Headers**

 - client_id (Id to authenticate client)
 - Accept (application/json)

###### **URI Parameters**
 - shortId (The shortId to resolve.)

##### **Success Responses**
###### **200 Response (application/json)** 
<pre><code>{
   "id":1270,
   "shortID":"50051",
   "systemtype":"sms",
   "shortidstate":"blocked",
   "hotlinephone":"0800800800",
   "hotlineemail":"swisscom@support.mondiamedia.de",
   "hotlinetime":"Mo - Fr 08:00 - 17:00h\nSa\nSo",
   "providername":"Mondia Media Group GmbH",
   "provideraddress1":"Kehrwieder 8",
   "provideraddress2":null,
   "providerzip":"20457",
   "providercity":"Hamburg",
   "descriptionDE":"Für weitere Informationen bitte nur via email swisscom@support.mondiamedia.de",
   "descriptionFR":"Pour plus des info, veuillez envoyer une email Ã  swisscom@support.mondiamedia.de",
   "descriptionIT":"Per oltre informazioni vi preghiamo di mandare una email : swisscom@support.mondiamedia.de",
   "descriptionEN":"Please send your request only by using the emailadr. swisscom@support.mondiamedia.de"
}</code></pre>
<pre><code>{
  "$schema": "http://json-schema.org/draft-04/schema",
  "type": "object",
  "properties": {
    "id": {
      "type":["integer", "null"]
    },
    "shortID": {
      "type":["string", "null"]
    },
    "systemtype": {
      "enum" : [
        "sms",
        "easypay",
        null]
    },
    "shortidstate": {
      "enum": [
        "available", 
        "active", 
        "reserved", 
        "blocked", 
        "temporarilyblocked",
        null] 
    },
    "hotlinephone": {
      "type": [
        "string",
        "null"
      ]
    },
    "hotlineemail": {
      "type": [
        "string",
        "null"
      ]
    },
    "hotlinetime": {
      "type": [
        "string",
        "null"
      ]
    },
    "providername": {
      "type": [
        "string",
        "null"
      ]
    },
    "provideraddress1": {
      "type": [
        "string",
        "null"
      ]
    },
    "provideraddress2": {
      "type": [
        "string",
        "null"
      ]
    },
    "providerzip": {
      "type": [
        "string",
        "null"
      ]
    },
    "providercity": {
      "type": [
        "string",
        "null"
      ]
    },
    "descriptionDE": {
      "type": [
        "string",
        "null"
      ]
    },
    "descriptionFR": {
      "type": [
        "string",
        "null"
      ]
    },
    "descriptionIT": {
      "type": [
        "string",
        "null"
      ]
    },
    "descriptionEN": {
      "type": [
        "string",
        "null"
      ]
    }
  }
}</code></pre>

##### **Error Responses**

[401](#401), [403](#403), [404](#404), [405](#405), [500](#500), [503](#503)

<div id='/messaging/v1/shortname/providers/{providerName}'/>
### /messaging/v1/shortname/providers/{providerName}

#### **get**: Get all short numbers of a given provider

##### **Request** 
###### **Headers**

 - client_id (Id to authenticate client)
 - Accept (application/json)

###### **URI Parameters**
 - providerName (The name of provider to resolve.)

##### **Success Responses**
###### **200 Response (application/json)** 

<pre><code>[
   {
      "name": "Mondia Media Group GmbH",
      "address1": "Kehrwieder 8",
      "address2": null,
      "zip": "20457",
      "city": "Hamburg",
      "shortids": 
      [
         "50051"
      ]
   },
   {
      "name": "Mondia Media Group GmbH",
      "address1": null,
      "address2": null,
      "zip": "",
      "city": null,
      "shortids": 
      [
         "SG",
         "SGR01"
      ]
   }
]</code></pre>
<pre><code>{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "type": "array",
    "items": {
        "type": "object",       
        "properties": {
            "address1": {
                "type": [
                    "string",
                    "null"
                ]
            },
            "address2": {
                "type": [
                    "string",
                    "null"
                ]
            },
            "city": {
                "type": [
                    "string",
                    "null"
                ]
            },
            "name": {
                "type": [
                    "string",
                    "null"
                ]
            },
            "shortids":
            {
                "type": "array",
                "items": {
                    "type": "string"
                }
            },
            "zip": {
                "type": [
                    "string",
                    "null"
                ]
            }
        }
    }
}</code></pre>

##### **Error Responses**

[401](#401), [403](#403), [404](#404), [405](#405), [500](#500), [503](#503)

<div id='/messaging/v1/sms/'/>
### /messaging/v1/sms/

#### **post**: Send an SMS

##### **Request** 
###### **Headers**

 - client_id (Id to authenticate client)
 - Content-Type (application/json)
 - Accept (application/json)

###### **Body** (application/json)
<pre><code>{
	"from" : "Swisscom",
	"to" : "+4171234567",
	"text" : "Greetings from Swisscom!"
	"callbackUrl" : "http://notification.com/service"
}</code></pre>
<pre><code>{
    "$schema": "http://json-schema.org/draft-04/schema",
    "type": "object",
    "required": [
      "to", "text"
    ],
    "properties": {
        "from": {
            "description": "Sender name or number. Valid MSISDN number or alpha numeric text with max. 11 Characters",
            "type": "string"
        },
        "to": {
            "description": "Recipient number. A valid number starts with 0, 00 or + and must contain 7 to 15 digits. Local numbers (prefix 0) are handled with international code +41 by default.", 
            "type": "string"
        },
        "text": {
            "description": "Appears as text in SMS.",
            "type": "string"
        },
        "callbackUrl": {
            "description": "A valid callback URL (HTTP) for delivery notification. The notification call will be send (PUT method with the header.location which contains messageID) for each SMS status change",
            "type": "string"
        }
    }
}</code></pre>

##### **Success Responses**
###### **201 Response** 
<pre><code>no content</code></pre>

##### **Error Responses**

[400](#400), [401](#401), [403](#403), [404](#404), [405](#405), [406](#406), [429](#429), [500](#500), [503](#503), 

<div id='/messaging/v1/sms/{messageId}'/>
### /messaging/v1/sms/{messageId}

#### **get**: Get Status information for a SMS.

##### **Request** 
###### **Headers**

 - client_id (Id to authenticate client)
 - Accept (application/json)

###### **URI Parameters**
 - messageId (messageId of SMS. You will receive this is on notification request.)

##### **Success Responses**
###### **200 Response (application/json)** 
<pre><code>{
  "messageId":"0001",
  "status":"201",
  "description":"Delivered with delivery confirmation"
}</code></pre>

<pre><code>{
  "$schema": "http://json-schema.org/draft-04/schema",
  "type": "object",
  "required": [
    "messageId",
    "status"
  ],
  "properties": {
    "messageId": {
      "type": "string"
    },
    "status": {
      "type": "string"
    },
    "description": {
      "type": "string"
    }
  }
}</code></pre>

##### **Error Responses**

[401](#401), [403](#403), [404](#404), [405](#405), [500](#500), [503](#503), 

<div id='/messaging/v1/tokenvalidation/'/>
### /messaging/v1/tokenvalidation/

#### **post**: This call will start the SMS Token validation process by sending a validation token to a MSISDN. The validation token is a random string of characters that is sent to the user's mobile phone. After making this call, your application can then verify the user recieved the validation token by calling Validate SMS Token.

##### **Request** 
###### **Headers**

 - client_id (Id to authenticate client)
 - Content-Type (application/json)
 - Accept (application/json)

###### **Body** (application/json)
<pre><code>{
   "to":"+41791544781",
   "text":"Verification code: %TOKEN% \r\n Expires in 60 seconds.",
   "tokenType":"SHORT_ALPHANUMERIC",
   "expireTime":60,
   "tokenLength":6
}</code></pre>
<pre><code>{
    "$schema": "http://json-schema.org/draft-04/schema",
    "type": "object" ,
    "required": [
      "to"
    ],
    "properties": {
        "to": {
            "description": "Mobile number which should be verified. A valid number starts with 0, 00 or + and must contain 7 to 15 digits. Local numbers (prefix 0) are handled with international code +41 by default. ",
            "type": "string"
        },
        "text": {
            "description": "Appears as text in SMS. %TOKEN% is the placeholder for the generated token.",
            "type": "string"
        },
        "tokenType": {
            "description": "Describes what kind of token should be generated.",
            "enum": [
                "SHORT_NUMERIC",
                "SHORT_ALPHANUMERIC",
                "SHORT_SMALL_AND_CAPITAL",
                "LONG_CRYPTIC"
            ]
        },
        "expireTime": {
            "description": "Expire time of the token in seconds.",
            "type": "integer",
            "minimum": 0,
            "maximum": 86400
        },
        "tokenLength": {
            "description": "The size of the token. It is mandatory for all SHORT_* tokenTypes.",
            "type": "integer",
            "minimum": 1,
            "maximum": 12
        }
    }
}</code></pre>

##### **Success Responses**
###### **201 Response** 
<pre><code>no content</code></pre>

##### **Error Responses**

[400](#400), [401](#401), [403](#403), [404](#404), [405](#405), [406](#406), [429](#429), [500](#500), [503](#503), 

<div id='/messaging/v1/tokenvalidation/{msisdn}/{token}'/>
### /messaging/v1/tokenvalidation/{msisdn}/{token}

#### **get**: Validate the token sent to the MSISDN number by the Send SMS Token call. This call is made after the Send SMS Token call to validate the token sent to the end user via SMS.

##### **Request** 
###### **Headers**

 - client_id (Id to authenticate client)
 - Accept (application/json)

###### **URI Parameters**
 - msisdn (The token depending phone number. A valid number starts with 0, 00 or + and must contain 7 to 15 digits. Local numbers (prefix 0) are handled with international code +41 by default.)
 - token (Token to verify.)

##### **Success Responses**
###### **200 Response** 
<pre><code>{
    "validation": "SUCCESS"
}</code></pre>

<pre><code>{
    "$schema": "http://json-schema.org/draft-04/schema",
    "type": "object" ,
    "required": [
        "validation"
    ],                        
    "properties": {
        "validation": {
            "description": "Whether token validation was successful",
            "enum": [
              "SUCCESS",
              "FAILED"
            ]
        }
    }
}</code></pre>

##### **Error Responses**

[400](#400), [401](#401), [403](#403), [404](#404), [405](#405), [406](#406),  [500](#500), [503](#503), 

<div id='ERROR'/>
## API - Error Handling

#### Error Response Schema
<pre><code>{
  "$schema": "http://json-schema.org/draft-03/schema",
  "type": "object" ,
  "properties": {
    "uuid": {
      "description": "The UUID of the client software",
      "type": "string",
      "required": false
    },
    "status": {
      "description": "The http status code, e.g. 400",
      "type": "string",
      "required": true
    },
    "code": {
      "description": "The error code, e.g. INVALID_PARAMETER",
      "type": "string",
      "required": true
    },
    "message": {
      "description": "The error message",
      "type": "string",
      "required": true
    },
    "detail": {
      "description": "The error detail",
      "type": "string",
      "required": false
    }
  }
}  </code></pre>

#### Error Response Examples
<div id='400'/>
##### **400** (Bad Request. Invalid request.)
<pre><code>{
  "status": "400",
  "code": "INVALID_REQUEST",
  "message": "The request could not be understood due to malformed syntax. Correct the request syntax and try again.",
  "detail": ""
}</code></pre>
<div id='401'/>
##### **401** (Unauthorized. No permission to resource or invalid api key.)
<pre><code>{
  "status": "401",
  "code": "INVALID_AUTHENTICATION_CREDENTIALS",
  "message": "Authentication credentials missing or incorrect. Check that you are using the correct authentication method and that you have permission to the given resource and your app key is approved.",
  "detail": ""
}</code></pre>
<div id='403'/>
##### **403** (Forbidden. No permission for requested resource.)
<pre><code>{
  "status": "403",
  "code": "FORBIDDEN_RESOURCE",
  "message": "The server understood the request, but is refusing to fulfill it. Check that you have permission to the given resource.",
  "detail": ""
}</code></pre>
<div id='404'/>
##### **404** (Not Found. The server has not found anything matching the Request-URI.)
<pre><code>{
  "status": "404",
  "code": "INVALID_REQUEST_RESOURCE",
  "message": "The requested resource does not exist. Verify that the resource URI is correctly spelled.",
  "detail": ""
}</code></pre>
<div id='405'/>
##### **405** (Method Not Allowed. The resource is not accessible via the given method. For example, the client sent a POST request to a resource that only implements GET.)
<pre><code>{
  "status": "405",
  "code": "INVALID_REQUEST_METHOD",
  "message": "The resource is not accessible via the given method. Check the documentation or the Allow header for allowed methods.",
  "detail": ""
}</code></pre>
<div id='406'/>
##### **406** (Invalid Header Accept. The provided Accept-type is not equal the accepted. E.g. application/zip instead of text/json.)
<pre><code>{
  "status": "406",
  "code": "INVALID_HEADER_ACCEPT",
  "message": "The requested resource is only capable of generating content not acceptable according to the Accept headers sent in the request. Make sure the request has an Accept header with a media type supported by the API. Check the developer portal for supported media types.",
  "detail": ""
}</code></pre>
<div id='429'/>
##### **429** (Too Many Requests. Quota for this service has been exhausted. Message must contain further details.)
<pre><code>{
  "status": "429",
  "code": "EXPIRED_QUOTA",
  "message": "Your quota for this service has been exhausted. Try again later or contact support at developer-support@swisscom.com.",
  "detail": ""
}</code></pre>
<div id='500'/>
##### **500** (Internal Server Error. Server is misconfigured, or generic error message.)
<pre><code>{
  "status": "500",
  "code": "INTERNAL_SERVER_ERROR",
  "message": "The server encountered an unexpected condition which prevented it from fulfilling the request. Try again later or contact support at developer-support@swisscom.com.",
  "detail": ""
}</code></pre>
<div id='503'/>
##### **503** (Service Unavailable. The server is currently unable to handle the request due to temporary overloading or maintenance. Typically back-end is unavailable.)
<pre><code>{
  "status": "503",
  "code": "UNAVAILABLE_SERVICE",
  "message": "The server is currently unable to handle the request due to temporary overloading or maintenance. Try again later or contact support at developer-support@swisscom.com.",
  "detail": ""
}</code></pre>

