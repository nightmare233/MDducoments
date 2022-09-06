  | Change Version | API Version | Change nots | Change Date | Author |Architect Team Reviewer | 
  | - | - | - | - | - |- |
  | 1.0 | v1 |Outreach API | 2022-5-18 | Leon,Frank,Yogesh|  Allon|

## Summary

### Contact API 

####  Contact
  - GET /contact/contacts/ - [Get the list of Contact](#get-the-list-of-contact).  
  - POST /contact/contacts:query - [Query contact with conditions](#query-contact-with-conditions).  
  - GET /contact/contacts/{id} - [Get a single Contact](#get-a-single-contact).  
  - POST /contact/contacts/ - [Create a new Contact](#create-a-new-contact).  
  - PUT /contact/contacts/{id} - [Update the Contact](#update-the-contact).  
  - DELETE /contact/contacts/{id} - [Delete the Contact](#delete-the-contact). 
  - DELETE /contact/contacts - [Batch delete contacts](#batch-delete-contacts). 
  - POST /contact/contacts/{id}:merge - [Merge Contact](#merge-contacts). 
  - GET /contact/contacts/importTemplate - [Download Template](#download-template). 
  - POST /contact/contacts:import - [Import Contacts](#import-contacts). 
  - Get /contact/contactImportRecords/{id} - [Get contact import record](#Get-contact-import-record). 
  - POST /contact/contacts:export - [Export Contacts](#export-contacts). 

####  Contact Identity
  - Get /contact/contactIdentities/{id} - [Get contact identity](#get-contact-identity).  
  - GET /contact/contactIdentities:identify - [Identify contact identities with type and value](#Identify-contact-identities-with-type-and-value).
  - POST /contact/contactIdentities:query - [Query contact identities with conditions](#query-contactidentities-with-conditions).  
  - POST /contact/contactIdentities - [Create a new Contact identity](#create-a-new-contact-identity).      
  - PUT /contact/contactIdentities/{id} - [Update the Contact identity](#update-the-contact-identity). 
  - DELETE /contact/contactIdentities/{id} - [Remove the Contact identity](#remove-the-contact-identity). 

####  Contact Field
  - GET /contact/fields - [Get the list of Contact Field](#get-the-list-of-contact-field).
  - GET /contact/fields/{id} - [Get a single Contact Field](#get-a-single-contact-field).  
  - POST /contact/fields - [Create a new Contact Field](#create-a-new-contact-field).  
  - PUT /contact/fields/{id} - [Update a Contact Field](#update-a-contact-field).  
  - DELETE /contact/fields/{id} - [Delete a Contact Field](#delete-a-contact-field).  
  - POST /contact/fields:reorder - [Reorder Contact Fields](#reorder-contact-fields).  

## Endpoints



### Get the list of Contact
`GET /contact/contacts/`

#### Parameters

#### Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `keywords` | string | No  | search scope includes: contact name/identity id/identity value |  
  | `contactIdentityType` | string | No  | contact identity type |  
  | `contactIdentityValue` | string | No  | contact identity value | 
  | `isIncludeDeleted` | boolean | No  | if including the deleted contact, default value: `false` | 
  | `pageIndex` | int | No  | page index | 
  | `pageSize` | int | No  | page size, default is 10, if the value is 0, will return all matched contacts. | 
  | `sortBy` | string | No  | sort by | 
  | `sortOrder` | string | No  | `asc`,`desc`, default `asc` |  

#### Request Body


#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`contacts` |[Contact](#contact-object)[]  |Yes|  An array of [Contact](#contact-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
  "contacts": [
	{
    "id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
    "name": "Tom cruise",
    "firstName": "Tom",
    "lastName": "Cruise",
    "createTime": "2022-03-18 01:12:32.0000000",
    "lastUpdatedTime": "2022-03-18 01:12:32.0000000",
    "mergeToContactId": "00000000-0000-0000-0000-000000000000",
    "customFields": { 
        "description": "Moive actor",
        "alias": "tomcruise",
        "title": "Senior Moive actor",
        "company": "Comm100",
        "fax": "",
        "phone": "",
        "mailingAddress": "",
        "city": "",
        "stateOrProvince": "",
        "countryOrRegion": "",
        "postalOrZipCode": "",
        "timeZone": ""
    },
    "contactIdentities":
      [{
          "id":"760a3dfb-f776-4dc8-99cb-7fb288bdf1eb",
          "contactId":"d5a7c487-7ac8-4b07-99e6-b85c3c6e56ab",
          "contactIdentityType":"SMS",
          "value":"+033214561",
          "avatarUrl":"",
          "infoUrl":"",
          "displayName":"",
          "originalContactPageUrl":"",
          "isDeleted":false
      }],
      "isDeleted":false
	}],
  "previousPage":null,
  "nextPage":"http://demo.comm100.io/contacts?pageIndex=2&pageSize=50&siteId=10100000",
  "total":255
}
```

### Query contact with conditions
`POST /contact/contacts:query`

#### Parameters

##### Path Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `pageIndex` | int | No  | page index | 
  | `pageSize` | int | No  | page size, default is 10, the max value is 500. | 
  | `sortBy` | string | No  | sort by | 
  | `sortOrder` | string | No  | `asc`,`desc`, default `asc` |  

##### Request Body
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `keywords` | string | No  | keywords |  
  | `contactIdentityType` | string | No  | contact identity type |  
  | `filterConditionMetType` | string | no |  Contact Filter Logical Expresssion of this Condition. `All`, `Any`, `LogicalExpression` | 
  | `filterLogicalExpresssion` | string | no |  Contact Filter Logical Expression | 
  | `filterConditions` | [ContactFilterCondition object](#contactFilterCondition-object)[] | no | An array of [ContactFilterCondition object](#contactFilterCondition-object)| 
  | `Ids` | guid[] | No  | Contact ID array, if pass this parameter, the keywords and filter conditions don't work | 
  | `isIncludeDeleted` | boolean | No  | if including the deleted contact, default value: `false` | 

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`contacts` |[Contact](#contact-object)[]  |Yes|  An array of [Contact](#contact-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
  "contacts": [
	{
    "id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
    "name": "Tom cruise",
    "firstName": "Tom",
    "lastName": "Cruise",
    "createTime": "2022-03-18 01:12:32.0000000",
    "lastUpdatedTime": "2022-03-18 01:12:32.0000000",
    "mergeToContactId": "00000000-0000-0000-0000-000000000000",
    "customFields": { 
        "description": "Moive actor",
        "alias": "tomcruise",
        "title": "Senior Moive actor",
        "company": "Comm100",
        "fax": "",
        "phone": "",
        "mailingAddress": "",
        "city": "",
        "stateOrProvince": "",
        "countryOrRegion": "",
        "postalOrZipCode": "",
        "timeZone": ""
    },
    "contactIdentities":
      [{
          "id":"760a3dfb-f776-4dc8-99cb-7fb288bdf1eb",
          "contactId":"d5a7c487-7ac8-4b07-99e6-b85c3c6e56ab",
          "contactIdentityType":"SMS",
          "value":"+033214561",
          "avatarUrl":"",
          "infoUrl":"",
          "displayName":"",
          "originalContactPageUrl":"",
          "isDeleted":false
      }],
    "isDeleted":false
	}],
  "previousPage":null,
  "nextPage":"http://demo.comm100.io/contacts?pageIndex=2&pageSize=50&siteId=10100000",
  "total":255
}
```

### Get a single Contact
`GET /contact/contacts/{id}`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Contact |  
  | `isIncludeDeleted` | boolean | No  | if including the deleted contact, default value: `false` | 

#### Response
The Response body contains data with the [Contact](#contact-object) object structure:

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
  "id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
  "name": "Tom cruise",
  "firstName": "Tom",
  "lastName": "Cruise",
  "createTime": "2022-03-18 01:12:32.0000000",
  "lastUpdatedTime": "2022-03-18 01:12:32.0000000",
  "mergeToContactId": "00000000-0000-0000-0000-000000000000",
  "customFields": {
        "description": "Moive actor",
        "alias": "tomcruise",
        "title": "Senior Moive actor",
        "company": "Comm100",
        "fax": "",
        "phone": "",
        "mailingAddress": "",
        "city": "",
        "stateOrProvince": "",
        "countryOrRegion": "",
        "postalOrZipCode": "",
        "timeZone": ""
    },
    "contactIdentities":
      [{
          "id":"760a3dfb-f776-4dc8-99cb-7fb288bdf1eb",
          "contactId":"d5a7c487-7ac8-4b07-99e6-b85c3c6e56ab",
          "contactIdentityType":"SMS",
          "value":"+033214561",
          "avatarUrl":"",
          "infoUrl":"",
          "displayName":"",
          "originalContactPageUrl":"",
          "isDeleted":false
      }],
    "isDeleted":false
}
```

### Create a new Contact
`POST /contact/contacts/`

#### Parameters
No path parameters

#### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `name` | string | yes |  The name of the Contact. |  
  | `contactIdentity` | [contactIdentities](#contact-Identity-object) | yes | Contact identities array. |  
  | `customField` | [custom fields](#custom-field-value-object) | yes |  value of contact custom fields. |  
  
example:
```Json 
{
    "name":"Test",
    "contactIdentities":[
        {
            "contactIdentityType":"Email",
            "value":"test@1234.com",
        }
    ],
    "customFields":{
        "company": "Comm100",
    }
}

```

#### Response
The Response body contains data with the [Contact](#contact-object) object structure:

```Json 
  HTTP/1.1 201 Created
  Content-Type: application/json
{
  "id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
  "name": "Tom cruise",
  "createTime": "2022-03-18 01:12:32.0000000",
  "lastUpdatedTime": "2022-03-18 01:12:32.0000000",
  "mergeToContactId": "00000000-0000-0000-0000-000000000000",
  "customFields": { 
      "company": "Comm100"
  },
  "contactIdentities":
    [{
        "id":"760a3dfb-f776-4dc8-99cb-7fb288bdf1eb",
        "contactId":"d5a7c487-7ac8-4b07-99e6-b85c3c6e56ab",
        "contactIdentityType":"Email",
        "value":"Test@1234.com",
        "avatarUrl":"",
        "infoUrl":"",
        "displayName":"",
        "originalContactPageUrl":""
    }],
    "isDeleted":false
}
```

### Update the Contact
`PUT /contact/contacts/{id}`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Contact |  

#### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description |     
  | - | - | - | - | 
  | `name` | string | yes |  The name of the Contact. |  
  | `contactIdentity` | [contactIdentities](#contact-Identity-object)[] | yes | Contact identities array. |  
  | `customField` | [custom fields](#custom-field-value-object) | yes |  value of contact custom fields. |  

example:
```Json 
{
  "id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
  "name": "Tom cruise 2",
  "createTime": "2022-03-18 01:12:32.0000000",
  "mergeToContactId": "00000000-0000-0000-0000-000000000000",
  "customFields": { 
      "company": "Comm100"
    },
  "contactIdentities":
    [{
        "id":"760a3dfb-f776-4dc8-99cb-7fb288bdf1eb",
        "contactId":"d5a7c487-7ac8-4b07-99e6-b85c3c6e56ab",
        "contactIdentityType":"Email",
        "value":"Test@1234.com",
        "avatarUrl":"",
        "infoUrl":"",
        "displayName":"",
        "originalContactPageUrl":"",
        "isDeleted":false
    }],
    "isDeleted":false
}
```

#### Response
The Response body contains data with the [Contact](#contact-object) object structure:


### Delete the Contact
`DELETE /contact/contacts/{id}`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  | The unique id of the Contact |  

#### Response
No response

### Batch delete contacts
`DELETE /contact/contacts`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `ids` | Guid | Yes  | The ID array of the Contacts |  

#### Response
No response

### Merge Contacts
`POST /contact/contacts/{id}:merge`

#### Parameters

##### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description |     
  | - | - | - | - | 
  | `contactId` | Guid | yes | The original ID of the Contact. |   

#### Response
No response

### Download Template
`GET /contact/contacts/importTemplate`

#### Parameters
No parameter

#### Response
- template file

### Import Contacts
`POST /contact/contacts:import`

#### Parameters

##### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description |     
  | - | - | - | - | 
  | `uploadFile` | file | yes | The import file URL |     
  | `duplicateControlOption` | string | no | Avaiable options: `merge`,`replace`,`skip`, default is `merge` |   

#### Response
  HTTP STATUS 202 (Accepted), includes the import record URI and ID.

### Get contact import record
`GET /contact/contactsImportRecords/{id}`

#### Parameters
   | Name | Type | Required | Description |     
  | - | - | - | - | 
  | `id` | guid | no | The ID of contact import record, the response of contact import request contains this ID. |  

#### Response
  | Name | Type | Required | Description |     
  | - | - | - | - | 
  | `Contact import record` | [Contact import record](#contact-import-record-object) | no | Contact import record object |   

### Export Contacts
`POST /contact/contacts:export`

#### Parameters
##### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description |     
  | - | - | - | - | 
  | `ids` | guid[] | no | The selected Contact ID array. |   

#### Response
- Cvs file

### Get Contact Identity
`GET /contact/ContactIdentities/{id}`

#### Parameters
##### Request parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  |`id` | guid | Yes | The ID of Contact identity |
  | `isIncludeDeleted` | boolean | No  | if including the deleted contact, default value: `false` | 

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact identity` |[Contact Identity](#contact-identity-object)  | Yes | Contact identity |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
  {
          "id":"760a3dfb-f776-4dc8-99cb-7fb288bdf1eb",
          "contactId":"d5a7c487-7ac8-4b07-99e6-b85c3c6e56ab",
          "contactIdentityType":"SMS",
          "value":"+033214561",
          "avatarUrl":"",
          "infoUrl":"",
          "displayName":"",
          "originalContactPageUrl":"",
          "isDeleted":false
    }
```

### Identify contact identities with type and value
`GET /contact/ContactIdentities:identify`
#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `value` | string | Yes  |  Contact identity value |
  | `identityType` | string | No |  Contact identity type |
  | `isIncludeDeleted` | boolean | No  | if including the deleted contact, default value: `false` | 

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact identity` |[Contact Identity](#contact-identity-object)[] | Yes | Contact identity array |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
    {
        "contactIdentities": [
            {
                "id": "213c2529-0112-ed11-a83d-00155de0e608",
                "contactId": "00a58408-ccb9-475c-ba6d-3c12e5317c9e",
                "contactIdentityType": "SMS",
                "value": "865550950",
                "avatarUrl": "",
                "infoUrl": "",
                "displayName": "",
                "originalContactPageUrl": "",
                "isDeleted": false
            },
            {
                "id": "223c2529-0112-ed11-a83d-00155de0e608",
                "contactId": "c4f19620-b724-4619-9aec-bc6847436ef8",
                "contactIdentityType": "SMS",
                "value": "865550951",
                "avatarUrl": "",
                "infoUrl": "",
                "displayName": "",
                "originalContactPageUrl": "",
                "isDeleted": false
            }
        ]
    }
```

### Query contactidentities with conditions  
`POST /contact/contactIdentities:query`
#### Parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `ids` | guid [] | No  |  Contact identity ID array, if this parameter has value, the value and identityType will be ignored.|
  | `value` | string | No  |  Contact identity value, support fuzzy search |
  | `identityType` | string | No |  Contact identity type |
  | `isIncludeDeleted` | boolean | No  | if including the deleted contact, default value: `false` | 

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact identity` |[Contact Identity](#contact-identity-object)[] | Yes | Contact identity array |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
    {
        "contactIdentities": [
            {
                "id": "213c2529-0112-ed11-a83d-00155de0e608",
                "contactId": "00a58408-ccb9-475c-ba6d-3c12e5317c9e",
                "contactIdentityType": "SMS",
                "value": "865550950",
                "avatarUrl": "",
                "infoUrl": "",
                "displayName": "",
                "originalContactPageUrl": "",
                "isDeleted": false
            },
            {
                "id": "223c2529-0112-ed11-a83d-00155de0e608",
                "contactId": "c4f19620-b724-4619-9aec-bc6847436ef8",
                "contactIdentityType": "SMS",
                "value": "865550951",
                "avatarUrl": "",
                "infoUrl": "",
                "displayName": "",
                "originalContactPageUrl": "",
                "isDeleted": false
            }
        ]
    }
```

### Create a new Contact identity
`POST /contact/contactIdentities`
#### Parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact identity` |[Contact Identity](#contact-identity-object) | Yes | Contact identity |

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact identity` |[Contact Identity](#contact-identity-object) | Yes | Contact identity |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
  {
    "id":"760a3dfb-f776-4dc8-99cb-7fb288bdf1eb",
    "contactId":"d5a7c487-7ac8-4b07-99e6-b85c3c6e56ab",
    "contactIdentityType":"SMS",
    "value":"+033214561",
    "avatarUrl":"",
    "infoUrl":"",
    "displayName":"",
    "originalContactPageUrl":""
    }
```

### Update the Contact identity
`PUT /contact/contactIdentities/{id}` 
#### Parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact identity` |[Contact Identity](#contact-identity-object) | Yes | Contact identity |

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact identity` |[Contact Identity](#contact-identity-object) | Yes | Contact identity |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
  {
    "id":"760a3dfb-f776-4dc8-99cb-7fb288bdf1eb",
    "contactId":"d5a7c487-7ac8-4b07-99e6-b85c3c6e56ab",
    "contactIdentityType":"SMS",
    "value":"+033214561",
    "avatarUrl":"",
    "infoUrl":"",
    "displayName":"",
    "originalContactPageUrl":""
    }
```
 
### Remove the Contact identity
`DELETE /contact/contactIdentities/{id}` 
#### Parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  |`id` | contact identity id | Yes | Contact identity ID |

#### Response
No response


### Get the list of contact field
`GET /contact/fields/`

#### Parameters
No parameters

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact fields` |[Contact Field](#contact-field-object)[]  |Yes| [Contact](#contact-field-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
  [{
    "id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
    "isSystem": "true",
    "isIdentity":"false",
    "identityType":"",
    "isRequired": "false",
    "isReadOnly": "false",
    "type": "text",
    "name": "Company",
    "length": 256,
    "helpText": "company name",
    "defaultValue": "Comm100",
    "order": 5,
    "fieldOptions":[]
  }]
```

### Get a single contact field
`GET /contact/fields/{id}`

#### Parameters
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`id` | Guid  |Yes| contact field ID |

#### Response
The Response body contains data with the [Contact Field](#contact-field-object) object structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contact field` |[Contact Field](#contact-field-object)  |Yes| [Contact](#contact-field-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
  "id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
  "isSystem": "true",
  "isIdentity":"false",
  "identityType":"",
  "isRequired": "false",
  "isReadOnly": "false",
  "type": "text",
  "name": "Company",
  "length": 256,
  "helpText": "company name",
  "defaultValue": "Comm100",
  "order": 5,
  "fieldOptions":[]
}
```

### Create a new contact field
`POST /contact/fields/`

#### Parameters
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  | `name` | string | unique name of the field |
  | `isRequired` | bool | Whether the field is required or not. |
  | `isReadOnly` | string | Whether the field is readyonly or not. |
  | `type` | string | Type of the field. Allowed values are "text", "textArea", "email", "url", "date", "integer", "float" |
  | `length` | integer | Field length |
  | `helpText` | string | Help text of the field. |
  | `defaultValue` | string | Default value of the field. |
  | `order` | integer | |
  | `fieldOptions` | [fieldOptions](#field-option-object)[] | Reference to Field Option. |

#### Response
The Response body contains data with the [Contact Field](#contact-field-object) object structure:


### Update a Contact Field
`PUT /contact/fields/{id}`

#### Parameters
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`id` | Guid  |Yes| contact field ID |
  | `name` | string | unique name of the field |
  | `isRequired` | bool | Whether the field is required or not. |
  | `isReadOnly` | string | Whether the field is readyonly or not. |
  | `type` | string | Type of the field. Allowed values are `text`, `textArea`, `email`, `url`, `date`, `integer`, `float` |
  | `length` | integer | Field length |
  | `helpText` | string | Help text of the field. |
  | `defaultValue` | string | Default value of the field. |
  | `order` | integer | |
  | `fieldOptions` | [fieldOptions](#field-option-object)[] | Reference to Field Option. |

#### Response
The Response body contains data with the [Contact Field](#contact-field-object) object structure:


### Delete a Contact Field
`Delete /contact/fields/{id}`

#### Parameters
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`id` | Guid  |Yes| contact field ID |

#### Response
No Response

### Reorder Contact Fields
`POST /contact/fields:reorder`
#### Parameters
  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`id` | Guid[]  | Yes | contact field ID array |

#### Response
No Response

## Model

### Contact Object
Contact is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the contact. |  
  | `name` | string | yes |  The name of the contact. |  
  | `firstName` | string | no | The first name of the contact |
  | `lastName` | string | no | The last name of the contact |
  | `createTime` | datetime | no |  The create time of the Contact. |  
  | `lastUpdatedTime` | datetime | no |  The last updated time of the Contact. |  
  | `mergeToContactId` | Guid | no |  The Contact id of merge to. |  
  | `customFields` | [Contact Custom Field Value](#contact-custom-field-value-object) | no | custom field value | 
  | `contactIdentities` | [Contact Identity](#contact-identity-object)[] | yes | custom field value array | 
  | `isDeleted` | boolean | if is deleted |

### Contact Identity Object
Contact Identity is represented as simple flat JSON objects with the following keys:

| Name | Type | Description | 
| - | - | - |
| `id` | string | read-only, the id of identity |
| `type` | string | required, contact identity type |
| `value` | string | required, the value of one identity, should be unique |
| `avatar` | string | optional, the avatar of one identity |
| `identityInfoUrl` | string | optional, the original identity info url of one identity |
| `originalContactPageUrl` | string | optional, the original contact page url |
| `displayName` | string | the name of identity |
| `isDeleted` | boolean | if is deleted |


### Contact custom field value object
| Name | Type | Description | 
| - | - | - | 
| `name` | string | the name of custom field |
| `value` | string | the value of custom field |

### Contact field object
| Name | Type | Description | 
| - | - | - | 
| `id` | guid | the id of custom field |
| `name` | string | unique name of the field |
| `isSystem` | bool | Whether the field is a system field or not. |
| `isIdentity` | bool | Whether the field is an identity field or not. |
| `identityType` | string | `visitor`, `email`, `SMS`, `facebook`, `twitter`, `wechat`, `SSOID`, `externalID`, `whatsApp`, `instagram`, `telegram`, `line`. Available when Is Identity is true. |
| `isRequired` | bool | Whether the field is required or not. |
| `isVisible` | bool | Whether the field is visible in UI or not. |
| `isReadOnly` | string | Whether the field is readyonly or not. |
| `type` | string | Type of the field. Allowed values are `text`, `textArea`, `email`, `url`, `date`, `date time`, `integer`, `float`, `radio`, `checkbox`, `dropdownlist`, `checkboxlist`, `timezone` |
| `length` | integer | Field length |
| `helpText` | string | Help text of the field. |
| `defaultValue` | string | Default value of the field. |
| `order` | integer | |
| `fieldOptions` | [fieldOptions](#field-option-object)[] | Reference to Field Option. |

### Field option object
| Name | Type | Description | 
| - | - | - | 
| `id` | guid | Id of the field option. |
| `fieldId` | guid | Id of the field which the option belongs to. |
| `value` | string | Value of the option. |
| `order` | integer | Order of the option. |
| `displayText` | string | Display text of the option. |

### Contact import record object
| Name | Type | Description | 
| - | - | - | 
| `id` | guid | Id of the field option. |
| `createdById` | guid | The Agent ID of createdBy. |
| `status` | string | enum: `in-process`, `completed`, `error` |
| `createdtime` | datetime | created time |
| `completedtime` | datetime | completed time |
| `result` | string | import result |
| `failedRecords` | string | json content of failed records |
| `importFileUrl` | string | the URL of imported file. |
