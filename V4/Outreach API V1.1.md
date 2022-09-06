  | Change Version | API Version | Change nots | Change Date | Author |Architect Team Reviewer | 
  | - | - | - | - | - |- |
  | 1.1 | v1 |Outreach API | 2022-9-5 | Frank, Saurabh Kashyap |  |

## Summary

### Outreach Call API  

#### Outreach Campaign
  - GET /outreach/campaigns/ - [Get the list of Outreach Campaign](#get-the-list-of-outreach-campaign). 
  - GET /outreach/campaigns/{id} - [Get a single Outreach Campaign](#get-a-single-outreach-campaign). 
  - POST /outreach/campaigns/ - [Create a new Outreach Campaign](#create-a-new-outreach-campaign).  
  - PUT /outreach/campaigns/{id} - [Update the Outreach Campaign](#update-the-outreach-campaign).  
  - DELETE /outreach/campaigns/{id} - [Delete the Outreach campaign](#delete-the-outreach-campaign). 
  - POST /outreach/campaigns/{id}:send - [Send the Outreach campaign](#Send-the-outreach-campaign). 
  
#### Outreach Message
  - GET /outreach/messages/ - [Get the list of Outreach Message](#get-the-list-of-outreach-message). 
  - GET /outreach/messages/{id} - [Get a single Outreach Message](#get-a-single-outreach-message). 
  - POST /outreach/messages - [Create a new Outreach Message](#create-a-new-outreach-message). 
  - PUT /outreach/messages/{id} - [Update the Outreach Message](#update-the-outreach-message).
  - GET /outreach/unattachedMessages/ - [Get the list of Outreach Message that needs to be attached by unichannel](#get-the-list-of-unattached-outreach-message). 

### Outreach Callback API 

The CallbackURL can be any valid URL that implements this API, and it is configured in the system when a new Outreach Message is created. 
  - POST /{callbackURL} - [Outreach Message Callback](#outreach-message-callback). 
  

###  Outbound Unichannel API 
  - POST /outboundunichannel/messages/ - [Create a new Outbound message](#create-a-outbound-message). 

###  Outbound Unichannel Callback API 
The CallbackURL can be any valid URL that implements this API, and it is configured in the system when a Outbound Message is created. 
  - POST /{callbackURL} - [Outbound Message Callback](#outbound-message-callback). 
  
### Ticketing API 
  - POST /ticketing/tickets/{id}/messages - [attach a message to the ticket](#attach-a-message-to-the-ticket). 

## Endpoints

### Get the list of Outreach Campaign
`GET /outreach/campaigns/`

#### Parameters
  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `channel` | string | no |  App channel of this Outreach Campaign, allowed values: `SMS`,`Email`,`WhatsApp`. |  
  | `channelAccountId` | Guid | no |  Channel account id of this Outreach Campaign. |  
  | `pageIndex` | int | No  | page index | 
  | `pageSize` | int | No  | page size, default is 10, if the value is 0, will return all matched contacts. | 
  | `sortBy` | string | No  | sort by | 
  | `sortOrder` | string | No  | `asc`,`desc`, default `asc` |  

#### Response
The Response body contains data with the following structure:

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
|`campaigns` |[OutreachCampaign](#outreachcampaign-object)[]  |Yes|  An array of [OutreachCampaign](#outreachcampaign-object)  |

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
	"campaigns": [{
		"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"name": "test-campaign",
		"description": "test campaign",
		"channel": "Email",
		"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
		"message": "Hello, please fill in your application form by the end of this week!", 
		"subject":"Test subject",
		"isMessageAutoAttachedToTicket": "true",
		"timeToAutoAttachToTicket": "whenTheMessageIsSent",
		"contactFilterConditionMetType": "any",
		"contactFilterLogicalExpresssion": "",
		"contactFilterConditions": [{
			"id":"6538285a-b00f-4063-b37a-cf14170aaa36",
			"campaignId":"f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
			"FieldName": "AAA",
			"Operator": "is",
			"Value": "BBBB",
			"Order": "0"
		}],
		"scheduledStartTime": "",
		"lastUpdatedTime": "2022-07-26T09:31:36.693Z"
	}],
	"previousPage":null,
  	"nextPage":"http://demo.comm100.io/campaigns?pageIndex=2&pageSize=50&siteId=10100000",
  	"total":255
}
```

### Get a single Outreach Campaign
`GET /outreach/campaigns/{id}`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outreach Campaign |  

#### Response
The Response body contains data with the [OutreachCampaign](#outreachcampaign-object) structure:


```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
	"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"name": "test-campaign",
	"description": "test campaign",
	"channel": "Email",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"message": "Hello, please fill in your application form by the end of this week!",
	"subject":"Test subject",
	"isMessageAutoAttachedToTicket": "true",
	"timeToAutoCreateNewTicket": "whenTheMessageIsSent",
	"contactFilterConditionMetType": "any",
	"contactFilterLogicalExpresssion": "",
	"contactFilterConditions": [{
		"id":"6538285a-b00f-4063-b37a-cf14170aaa36",
		"campaignId":"f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"FieldName": "AAA",
		"Operator": "is",
		"Value": "BBBB",
		"Order": "0"
	}],
	"scheduledStartTime": "",
	"lastUpdatedTime": "2022-07-26T09:31:36.693Z"
}
```

### Create a new Outreach Campaign
`POST /outreach/campaigns/`

#### Parameters
No parameters

#### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `name` | string | yes |  Name of this Outreach Campaign. |  
  | `description` | string | no |  Description of this Outreach Campaign. |  
  | `channel` | string | yes |  App channel of this Outreach Campaign, allowed values: `SMS`,`Email`,`WhatsApp`. |  
  | `channelAccountId` | Guid | yes |  Channel account id of this Outreach Campaign. |  
  | `message` | string | yes |  Message of this Outreach Campaign. |  
  | `subject` | string | yes |  Email subject of this Outreach Campaign. |  
  | `isMessageAutoAttachedToTicket` | bool | no |  Whether a message need to auto attached to ticket. |   
  | `timeToAutoAttachToTicket` | string | no |  Allowed values are "whenTheMessageIsSent", "whenContactRepliesTheMessage". |  
  | `contactFilterConditionMetType` | string | no |  Allowed values are "all", "any", "logicalExpression". |  
  | `contactFilterLogicalExpresssion` | string | no |  Contact Filter Logical Expresssion of this Condition. |  
  | `contactFilterConditions` | [ContactFilterCondition](#contactfiltercondition-object)[] | no | An array of [ContactFilterCondition](#contactfiltercondition-object)| 
  | `scheduledStartTime` | datetime | no |  Time when the schedule start. |  
  
example:
```Json 
{
	"name": "test-campaign",
	"description": "test campaign",
	"channel": "Email",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"message": "Hello, please fill in your application form by the end of this week!",
    "subject":"Test subject",
	"isMessageAutoAttachedToTicket": "true",
	"timeToAutoAttachToTicket": "whenTheMessageIsSent",
	"contactFilterConditionMetType": "any",
	"contactFilterLogicalExpresssion": "",
	"contactFilterConditions": [{
		"id":"6538285a-b00f-4063-b37a-cf14170aaa36",
		"campaignId":"f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"FieldName": "AAA",
		"Operator": "is",
		"Value": "BBBB",
		"Order": "0"
	}],
	"scheduledStartTime": "",
	"lastUpdatedTime": "2022-07-26T09:31:36.693Z"
}
```

#### Response
The Response body contains data with the [OutreachCampaign](#outreachcampaign-object) structure:


```Json 
  HTTP/1.1 201 Created
  Content-Type: application/json
{
	"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"name": "test-campaign",
	"description": "test campaign",
	"channel": "Email",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"message": "Hello, please fill in your application form by the end of this week!",
    "subject": "Test subject",
	"isMessageAutoAttachedToTicket": "true",
	"timeToAutoAttachToTicket": "whenTheMessageIsSent",
	"contactFilterConditionMetType": "any",
	"contactFilterLogicalExpresssion": "",
	"contactFilterConditions": [{
		"id":"6538285a-b00f-4063-b37a-cf14170aaa36",
		"campaignId":"f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"FieldName": "AAA",
		"Operator": "is",
		"Value": "BBBB",
		"Order": "0"
	}],
	"scheduledStartTime": "",
	"lastUpdatedTime": "2022-07-26T09:31:36.693Z"
}
```

### Update the Outreach Campaign
`PUT /outreach/campaigns/{id}`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outreach Campaign |  

#### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - |  
  | `name` | string | no |  Name of this Outreach Campaign. |  
  | `description` | string | no |  Description of this Outreach Campaign. |  
  | `channel` | string | no |  App channel of this Outreach Campaign, allowed values: `SMS`,`Email`,`WhatsApp`. |  
  | `channelAccountId` | Guid | no |  Channel account id of this Outreach Campaign. |  
  | `message` | string | no |  Message of this Outreach Campaign. |  
  | `subject` | string | no |  Email subject of this Outreach Campaign. |  
  | `isMessageAutoAttachedToTicket` | bool | no |  Whether a message need to auto attached to ticket. |  
  | `timeToAutoAttachToTicket` | string | no |  Allowed values are "whenTheMessageIsSent", "whenContactRepliesTheMessage". |  
  | `contactFilterConditionMetType` | string | no |  Allowed values are "all", "any", "logicalExpression". |  
  | `contactFilterLogicalExpresssion` | string | no |  Contact Filter Logical Expresssion of this Condition. |  
  | `contactFilterConditions` | [ContactFilterCondition](#contactfiltercondition-object)[] | no | An array of [ContactFilterCondition](#contactfiltercondition-object)| 
  | `scheduledStartTime` | datetime | no |  Time when the schedule start. |  

example:
```Json 
{
	"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"name": "test-campaign 2",
	"description": "test campaign",
	"channel": "Email",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"message": "Hello, please fill in your application form by the end of this week!",
    "subject": "Test subject",
	"isMessageAutoAttachedToTicket": "true",
	"timeToAutoAttachToTicket": "whenTheMessageIsSent",
	"contactFilterConditionMetType": "any",
	"contactFilterLogicalExpresssion": "",
	"contactFilterConditions": [{
		"id":"6538285a-b00f-4063-b37a-cf14170aaa36",
		"campaignId":"f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"FieldName": "AAA",
		"Operator": "is",
		"Value": "BBBB",
		"Order": "0"
	}],
	"scheduledStartTime": "",
	"lastUpdatedTime": "2022-07-26T09:31:36.693Z"
}
```

#### Response
The Response body contains data with the [OutreachCampaign](#outreachcampaign-object) object structure:

```Json 
  HTTP/1.1 200 Ok
  Content-Type: application/json
{
	"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"name": "test-campaign 2",
	"description": "test campaign",
	"channel": "Email",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"message": "Hello, please fill in your application form by the end of this week!",
    "subject": "Test subject",
	"isMessageAutoAttachedToTicket": "true",
	"timeToAutoAttachToTicket": "whenTheMessageIsSent",
	"contactFilterConditionMetType": "any",
	"contactFilterLogicalExpresssion": "",
	"contactFilterConditions": [{
		"id":"6538285a-b00f-4063-b37a-cf14170aaa36",
		"campaignId":"f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"FieldName": "AAA",
		"Operator": "is",
		"Value": "BBBB",
		"Order": "0"
	}],
	"scheduledStartTime": "",
	"lastUpdatedTime": "2022-07-26T09:31:36.693Z"
}
```

### Delete the Outreach Campaign
`DELETE /outreach/campaigns/{id}`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outreach Campaign |  

#### Response
No response

```Json 
  HTTP/1.1 204 Ok
```

### Send the Outreach Campaign
`POST /outreach/campaigns/{id}:send`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outreach Campaign |  
  | `isScheduled` | bool | Yes  | Whether is scheduled | 

#### Response
The Response body contains data with the [OutreachCampaignRecord](#outreachcampaignrecord-object) object structure:

```Json 
  HTTP/1.1 200 Ok
  Content-Type: application/json
{
	"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"sentTime": "2022-05-23 03:00:36.277",
	"campaign":{
		"id": "f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"name": "test-campaign 2",
		"description": "test campaign",
		"channel": "Email",
		"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
		"message": "Hello, please fill in your application form by the end of this week!",
        "subject": "Test subject",
		"isMessageAutoAttachedToTicket": "true",
		"timeToAutoAttachToTicket": "whenTheMessageIsSent",
		"contactFilterConditionMetType": "any",
		"contactFilterLogicalExpresssion": "",
		"contactFilterConditions": [{
			"id":"6538285a-b00f-4063-b37a-cf14170aaa36",
			"campaignId":"f8383a83-48e9-4d0d-a3bd-fb19ce5c12db",
			"FieldName": "AAA",
			"Operator": "is",
			"Value": "BBBB",
			"Order": "0"
		}],
		"scheduledStartTime": ""
	}
}
```

### Get the list of Outreach Message
`GET /outreach/messages/`

#### Parameters
parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
| `channel` | string | no |  App channel of this Outreach Campaign, allowed values: `SMS`,`Email`,`WhatsApp`. |  
| `channelAccountId` | Guid | no |  Channel account id of this Outreach Campaign. |  
| `status` | string | no |  Status of the Outreach Message. Allowed values are `queued`, `sending`, `sent`, `failed`，`delivered`,`undelivered` |  
| `campaignId` |Guid |No| The unique id of the Outreach Campaign | 
| `contactId` |Guid  |No| The unique id of the Contact  |
| `sentTime` | datetime | no |  The time when the Outreach Message is sent. |  
| `keywords` | string | No  | search scope includes: from/to/campaign.name/contact.name |
| `pageIndex` | int | No  | page index | 
| `pageSize` | int | No  | page size, default is 10, if the value is 0, will return all matched contacts. | 
| `sortBy` | string | No  | sort by | 
| `sortOrder` | string | No  | `asc`,`desc`, default `asc` | 

#### Response
The Response body contains data with the [OutreachMessage](#outreachmessage-object) object structure:


```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
	"messages": [{
		"id": "11183a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"sentTime": "2022-05-23 03:00:36.277",
		"channel": "Email",
		"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
		"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
		"message": "Hello, please fill in your application form by the end of this week!",
        "metadata":
        {
            "subject":"Test subject",
            "reference":"outreach-f9928d68-92e6-4487-a2e8-8234fc9d1f48"
        },
		"from": "13312145080",
		"to": "+19085520595",
		"status": "sent",
		"failedReason": "",
		"campaignId": "d3f5b968-ad51-42af-b759-64c0afc40b84",
		"campaignSentTime": "2022-05-22 03:00:36.277",
		"isMessageAutoAttachedToTicket": "true",
		"timeToAutoAttachToTicket": "whenTheMessageIsSent",
		"attachedToTicketId": 1,
		"callbackURL": "https://domainname.com/sms/callback",
		"outboundMessageId": "d426e8b7-c83f-46f9-ad8b-0086813d8345"
	}],
	"previousPage":null,
  	"nextPage":"http://demo.comm100.io/messages?pageIndex=2&pageSize=50&siteId=10100000",
  	"total":255
}
```


### Get the list of Unattached Outreach Message
`GET /outreach/unattachedmessages/`

#### Parameters
parameters

  | Name  | Type | Required  | Description |     
  | - | - | - | - | 
  |`contactId` | Guid |yes| The unique id of the Contact  |
  |`channelId` | string |yes| The channel Id  |

#### Response
The Response body contains data with the [OutreachMessage](#outreachmessage-object) object structure:


```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
	"messages": [{
		"id": "11183a83-48e9-4d0d-a3bd-fb19ce5c12db",
		"sentTime": "2022-05-23 03:00:36.277",
		"channel": "Email",
		"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
		"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
		"message": "Hello, please fill in your application form by the end of this week!",
        "metadata":{
            "subject":"Test subject",
            "reference":"outreach-f9928d68-92e6-4487-a2e8-8234fc9d1f48"
        },
		"from": "13312145080",
		"to": "+19085520595",
		"status": "sent",
		"failedReason": "",
		"campaignId": "d3f5b968-ad51-42af-b759-64c0afc40b84",
		"campaignSentTime": "2022-05-22 03:00:36.277",
		"isMessageAutoAttachedToTicket": "true",
		"timeToAutoAttachToTicket": "whenTheMessageIsSent",
		"attachedToTicketId": 0,
		"callbackURL": "https://domainname.com/sms/callback",
		"outboundMessageId": "d426e8b7-c83f-46f9-ad8b-0086813d8345"
	}]
}
```


### Get a single Outreach Message
`GET /outreach/messages/{id}`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outreach Message |  

#### Response
The Response body contains data with the [OutreachMessage](#outreachmessage-object) object structure:

```Json 
  HTTP/1.1 200 OK
  Content-Type: application/json
{
	"id": "11183a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"sentTime": "2022-05-23 03:00:36.277",
	"channel": "Email",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"message": "Hello, please fill in your application form by the end of this week!",
    "metadata":{
        "subject":"Test subject",
        "reference":"outreach-11183a83-48e9-4d0d-a3bd-fb19ce5c12db"
    },
	"from": "13312145080",
	"to": "+19085520595",
	"status": "sent",
	"failedReason": "",
	"campaignId": "d3f5b968-ad51-42af-b759-64c0afc40b84",
	"campaignSentTime": "2022-05-22 03:00:36.277",
	"isMessageAutoAttachedToTicket": "true",
	"timeToAutoAttachToTicket": "whenTheMessageIsSent",
	"attachedToTicketId": 1,
	"callbackURL": "https://domainname.com/sms/callback",
	"outboundMessageId": "d426e8b7-c83f-46f9-ad8b-0086813d8345"
}
```

### Create a new Outreach Message
`POST /outreach/messages`

#### Parameters
No Parameters

#### Request body

  | Name | Type | Required | Description                                           |     
  | - | - | - | - |  
  | `contactId` | Guid | yes |  Contact id of the Outreach Message. |  
  | `channel` | string | yes |  App channel of this Outreach Campaign, allowed value is `SMS`,`Email`,`WhatsApp` |
  | `channelAccountId` | Guid | yes |  Channel account id of this Outreach Campaign. |  
  | `message` | string | yes |  Message. |  
  | `metadata` | string | yes |  Metadata. |  
  | `isMessageAutoAttachedToTicket` | bool | no |  Whether a message need to auto attached to ticket. |  
  | `timeToAutoAttachToTicket` | string | no |  Allowed values are "whenTheMessageIsSent", "whenContactRepliesTheMessage". |  
  | `callbackURL` | string | no |  The callbackURL of the Outreach Message. |  
  
example:
```Json 
{
	"channel": "Email",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"message": "Hello, please fill in your application form by the end of this week!",
    "metadata":{
        "subject":"Test subject"
    },
	"isMessageAutoAttachedToTicket": "true",
	"timeToAutoAttachToTicket": "whenTheMessageIsSent",
	"callbackURL": "https://domainname.com/sms/callback",
}
```

#### Response
The Response body contains data with the [OutreachMessage](#outreachmessage-object) object structure:

```Json 
  HTTP/1.1 201 Created
  Content-Type: application/json
{
	"id": "11183a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"sentTime": "2022-05-23 03:00:36.277",
	"channel": "Email",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"message": "Hello, please fill in your application form by the end of this week!",
    "metadata":{
        "subject":"Test subject",
        "reference":"outreach-11183a83-48e9-4d0d-a3bd-fb19ce5c12db"
    },
	"from": "13312145080",
	"to": "+19085520595",
	"status": "queued",
	"failedReason": "",
	"campaignId": "d3f5b968-ad51-42af-b759-64c0afc40b84",
	"campaignSentTime": "2022-05-23 03:00:36.277",
	"isMessageAutoAttachedToTicket": "true",
	"timeToAutoAttachToTicket": "whenTheMessageIsSent",
	"attachedToTicketId": 0,
	"callbackURL": "https://domainname.com/sms/callback",
	"outboundMessageId": "d426e8b7-c83f-46f9-ad8b-0086813d8345"
}
```

### Update the Outreach Message
`PUT /outreach/messages/{id}`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | Guid | Yes  |  The unique id of the Outreach Message |  

#### Request body
The request body contains data with the following structure:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - |   
  | `attachedToTicketId` | int | no |  The attached ticked Id of the Outreach Message. |  
  
example:
```Json 
{
	"attachedToTicketId": 1
}
```

#### Response
The Response body contains data with the [OutreachMessage](#outreachmessage-object) object structure:

```Json 
  HTTP/1.1 200 Ok
  Content-Type: application/json
{
	"id": "11183a83-48e9-4d0d-a3bd-fb19ce5c12db",
	"sentTime": "2022-05-23 03:00:36.277",
	"channel": "Email",
	"channelAccountId": "647277e8-06a5-4eec-ba66-1cdd617dc778",
	"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"message": "Hello, please fill in your application form by the end of this week!",
    "metadata":{
        "subject":"Test subject",
        "reference":"outreach-11183a83-48e9-4d0d-a3bd-fb19ce5c12db"
    },
	"from": "Tom Cruise",
	"to": "XXX University",
	"status": "sent",
	"failedReason": "",
	"campaignId": "d3f5b968-ad51-42af-b759-64c0afc40b84",
	"campaignSentTime": "2022-05-22 03:00:36.277",
	"isMessageAutoAttachedToTicket": "true",
	"timeToAutoAttachToTicket": "whenTheMessageIsSent",
	"attachedToTicketId": 1,
	"callbackURL": "https://domainname.com/sms/callback",
	"outboundMessageId": "d426e8b7-c83f-46f9-ad8b-0086813d8345"
}
```

### Outreach Message Callback
`POST /{callbackURL}`

#### Parameters
No Parameters

#### Request body
  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Outreach Message. |  
  | `sentTime` | datetime | yes |  The sent time the Outreach Message. |  
  | `contactId` | Guid | yes |  Contact id of the Outreach Message. |  
  | `from` | string | yes |  Where the Outreach Message from. |  
  | `to` | string | yes |  Where the Outreach Message to. |  
  | `status` | string | yes |  Status of the Outreach Message. Allowed values are `queued`, `sending`, `sent`, `failed`，`delivered`,`undelivered` |  
  | `failedReason` | string | no |  The failed reason of the Outreach Message. |  
  | `campaignId` | Guid | no |  The Outreach Campaign id the Outreach Message. |  
  | `campaignSentTime` | datetime | no |  The sent time of the Outreach Campaign. |  
  | `attachedToTicketId` | int | no |  The attached ticked Id of the Outreach Message. |  

example:
```Json 
{
	"id": "11183a83-48e9-4d0d-a3bd-fb19ce5c12db",		
	"status": "sent",
	"failedReason": "",
}
```
#### Response

```Json 
  HTTP/1.1 200 OK
```


### Create a outbound message
`POST /outboundunichannel/messages/`

#### Parameters
Request body 
Request body the request body contains data with the following structure: 
  | Name | Type | Required | Default | Description |    
  | - | - | :-: | :-: | - | 
  |`channelAccountId` | Guid | yes |  Channel account id. |  
  |`contactId`  | Guid | no |   |   | 
  |`to`  | string | yes |   |   |
  |`message`  | string | yes |   |   |
  |`attachments`  | [Attachment](#attachment-object)[] object | no |  the attachments of message|   
  |`callbackURL` | string | no |  The callbackURL of the Outbound Message. |  
  |`email`  | [Outreach Message MetaData Object](#outreach-message-metadata-object) | no |  extra properties for outbound message|
  #### example:
```Json 
 {
    "channelAccountId":"q3f5b438-xw31-44af-b729-64swaf3d0b56",
    "to": "+19857473632", 
    "contactId":"q3f5b438-xw31-44af-b729-64swaf3d0b56",
    "message":"hello!Leon",
    "callbackURL":"https://domainname.com/callback/",
	"metadata":{
		"reference":"outreach-f9928d68-92e6-4487-a2e8-8234fc9d1f48",
		"subject":"test subject"
	}
  } 
```

#### Response
The Response body contains data with the [OutboundMessage](#outboundmessage-object) object structure:


Response
```Json
HTTP/1.1 200 OK 
  Content-Type:  application/json 
{
	"id": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"originMessageid": "f9928d68-92e6-4487-a2e8-8234fc9d1f48", //This is a string type as different channel will return different type. 
	"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"message": "Hello, please fill in your application form by the end of this week!",
    "metadata":{
		"reference":"outreach-f9928d68-92e6-4487-a2e8-8234fc9d1f48",
		"subject":"test subject"
	},
	"from" : "13312155802",
	"to": "+13453746564",
	"status": "sent",
	"failedReason": "",
	"sentTime": "2022-04-26T10:52:24.336Z"
}
```
### Outbound Message Callback
`POST /{callbackURL}`

#### Parameters
No Parameters

#### Request body
Request body the request body contains data with the following structure: 
  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Outbound Message. |    
  | `originMessageid` | string | yes | The origin channel unique id of the Outbound Message. |    
  | `contactId` | Guid | no |  Contact id of the Outbound Message. |  
  | `from` | string | yes |  Where the Outbound Message from. |   
  | `to` | string | yes |  Where the Outbound Message to. |  
  | `status` | string | yes |  Status of the Outbound Message. Allowed values are `queued`, `sending`, `sent`, `failed`，`delivered`,`undelivered` |   
  | `failedReason` | string | no |  The failed reason of the Outbound Message. |  
The request body contains data with the [OutboundMessage](#outboundmessage-object) object structure:

example:
```Json 
{
	"id": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"contactId": "f9928d68-92e6-4487-a2e8-8234fc9d1f48",
	"message": "Hello, please fill in your application form by the end of this week!",
	"attachments": [],
	"to": "+13453746564",
	"from" : "13312155802",
	"status": "sent",
	"failedReason": "",
	"sentTime": "2022-04-26T10:52:24.336Z",
}
```
#### Response

```Json 
  HTTP/1.1 200 OK
```

### Attach a message to the ticket
`POST /ticketing/tickets/{id}/messages`

#### Parameters
Path parameters

  | Name | Type | Required  | Description |     
  | - | - | - | - | 
  | `id` | int | Yes  |  The unique id of the ticket |  
#### Request body 
Request body the request body contains data with the [Ticket Message](#ticketmessage-object) object structure: 
  | Name | Type | Required |  Description |    
  | - | - | :-: |  - | 
  |`parentId` | guid | no | Id of current message's parent. |
  |`sentById` | guid | yes | Id of the message sender. |
  |`sentByType` | string | yes | Role of the sender. Allowed values are "contact", "visitor", "chatbot", "channelAccount", "system", "agent". For Outreach send value as "outreach." |
  |`time` | datetime | no | Time when the message was created. |
  |`type` | string | yes | Type of message content. Allowed values are "text", "html", "video", "audio", "image", "file", "location", "quickReply", "webView". |
  |`metadata` | object | yes | Message metadata, Json format. |
  |`originalId` | string | no | Id in original channel. |
  |`channelId` | string | yes | Name of the message channel. |
  |`body` | string | no | Content of the message. You can pass both plaintext and base64 encoded text. If the request containing plaintext is blocked by comm100 WAF, 	use base64 format. When using base64, add "data:text/plain;base64," before the content. |
  |`ifDisplayInTicketCorrespondences` | bool | no | Whether a trigger email should be included within a ticket's correspondence thread or not. |
  |`isRead` | string | no | Name of the message channel. |
  |`messageDelivery` | [messageDelivery](#messagedelivery-object) | no | Delivery information of the message. |
  |`channelId` | [attachments[]](#attachment-object) | no | The list of message attachment. |
  #### example:
```Json 
{
	"parentId": "ef50cc68-5b88-4405-88f0-84334581246d",
	"sentById": "31cb8d70-b5a6-4faa-b021-62335d6dcf6c",
	"sentByType": "visitor",
	"time": "2021-04-26T10:52:24.336Z",
	"type": "text",
	"metadata": {...},
	"originalId": "96e7964a-29cb-40d6-8251-4f334b5748bd",
	"channelId": "Email",
	"body": "data:text/plain;base64,PHA+MTExPC9wPg==",
	"ifDisplayInTicketCorrespondences": true,
	"isRead": true,
	"attachments": [],
	"messageDelivery": {}
}
```

#### Response

Response
```Json
HTTP/1.1 200 OK 
```

## Model

### OutreachCampaign Object
Outreach Campaign is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Outreach Campaign. |  
  | `name` | string | yes |  Name of this Outreach Campaign. |  
  | `description` | string | no |  Description of this Outreach Campaign. |  
  | `channel` | string | no |  App channel of this Outreach Campaign, allowed value is "SMS". |  
  | `channelAccountId` | Guid | yes |  Channel account id of this Outreach Campaign. |  
  | `message` | string | yes |  Message of this Outreach Campaign. |  
  | `subject` | string | yes |  Outreach email subject. |  
  | `isMessageAutoAttachedToTicket` | bool | no |  Whether a message need to auto attached to ticket. | 
  | `timeToAutoAttachToTicket` | string | no |  Allowed values are "whenTheMessageIsSent", "whenContactRepliesTheMessage". |  
  | `contactFilterConditionMetType` | string | no |  Allowed values are "all", "any", "logicalExpression". |  
  | `contactFilterLogicalExpresssion` | string | no |  Contact Filter Logical Expresssion of this Condition. |  
  | `contactFilterConditions` | [ContactFilterCondition](#contactfiltercondition-object)[] | no | An array of [ContactFilterCondition](#contactfiltercondition-object)| 
  | `scheduledStartTime` | datetime | no |  Time when the schedule start. |  
  | `lastUpdatedTime` | datetime | no |  Time when the campaign last updated. |  
  
### ContactFilterCondition Object
Outreach Campaign Contact Filter Condition is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `fieldName` | string | yes |  Field name of the contact filter condition. |  
  | `operator` | string | yes |  Allowed values are "Contains", "Does Not Contain", "Is", "Is Not", "Is More Than", "Is Less Than", "Before", "After". |  
  | `value` | string | yes |  Trigger mode of the contact filter condition. |  
  | `order` | integer | yes |  Order of the contact filter condition. | 
  
### OutreachCampaignRecord Object
Outreach Campaign is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Outreach Campaign Record. |  
  | `sentTime` | datetime | yes | The unique id of the Outreach Campaign. | 
  | `campaign` |  [OutreachCampaign](#outreachcampaign-object) | yes | Outreach Campaign. |  
  
  
### OutreachMessage Object
Outreach Message is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Outreach Message. |  
  | `sentTime` | datetime | yes |  The sent time the Outreach Message. |  
  | `contactId` | Guid | yes |  Contact id of the Outreach Message. |  
  | `channel` | string | yes |  App channel of this Outreach Campaign, allowed value is "sms". |
  | `channelAccountId` | Guid | yes |  Channel account id of this Outreach Campaign. |  
  | `message` | string | yes |  Message. |  
  | `from` | string | yes |  Where the Outreach Message from. |  
  | `to` | string | yes |  Where the Outreach Message to. |  
  | `status` | string | yes |  Status of the Outreach Message. Allowed values are `queued`, `sending`, `sent`, `failed`，`delivered`,`undelivered` |  
  | `failedReason` | string | no |  The failed reason of the Outreach Message. |  
  | `campaignId` | Guid | no |  The Outreach Campaign id the Outreach Message. |  
  | `campaignSentTime` | datetime | no |  The sent time of the Outreach Campaign. |  
  | `isMessageAutoAttachedToTicket` | bool | no |  Whether a message need to auto attached to ticket. |    
  | `timeToAutoAttachToTicket` | string | no |  Allowed values are "whenTheMessageIsSent", "whenContactRepliesTheMessage". |  
  | `attachedToTicketId` | int | no |  The attached ticked Id of the Outreach Message. |  
  | `callbackURL` | string | no |  The callbackURL of the Outreach Message. |  
  | `outboundMessageId` | Guid | no |  The outbound message Id of the Outreach Message. |  
  | `metadata` | [Outreach Message MetaData Object](#outreach-message-metadata-object) | no | extra info for message, such as email subject, reference key, etc. |  


### OutboundMessage Object
Outbound Message is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `id` | Guid | yes | The unique id of the Outbound Message. |    
  | `originMessageid` | string | yes | The origin channel unique id of the Outbound Message. |    
  | `channelAccountId` | Guid | yes |  Channel account id of this Outbound Campaign. |  
  | `contactId` | Guid | no |  Contact id of the Outbound Message. |  
  | `message` | string | yes |  Message. |  
  | `metadata` | [Outreach Message Metadata](#outreach-message-metadata-object) | yes | Metadata. |  
  | `attachments` | string | no |  attachment. |  
  | `from` | string | yes |  Where the Outbound Message from. |   
  | `to` | string | yes |  Where the Outbound Message to. |  
  | `status` | string | yes |  Status of the Outbound Message. Allowed values are `queued`, `sending`, `sent`, `failed`，`delivered`,`undelivered` |   
  | `failedReason` | string | no |  The failed reason of the Outbound Message. |  
  | `callbackURL` | string | no |  The callbackURL of the Outreach Message. |  
  
 ### Attachment Object
Attachment Object is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `type` | string | yes | Type of the attachment. Allowed values are "video", "audio", "image", "file". |  
  | `url` | string | yes | Download URL of the attachment. |  
  | `size` | integer | no |  Size of the attachment. |  
  | `name` | string | no |  Name of the attachment. |  
  
 ### MessageDelivery Object
MessageDelivery Object is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `messageId` | guid | yes |  Id of the message which the delivery belongs to. |  
  | `status` | string | no |  Status of the delivery. Allowed values are "waitForSending", "sending", "failed", "success". |  
  | `failReason` | string | no | Reason why the delivery failed. |  
  | `time` | datetime | no | Time when message was delivered. |  
  | `groupId` | guid | no |  The Id of the message group, it is used for bot message, it should be delivered in a group and one by one in order. |  
  | `orderNum` | int | no |  Order of the delivery. |  
  
  ### CallbackRequest Object
MessageDelivery Object is represented as simple flat JSON objects with the following keys:

  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `Id` | guid | yes |  The unique id of the Outreach Message. |  
  | `status` | string | no |  Status of the delivery. Allowed values are "waitForSending", "sending", "failed", "success". |  
  | `failReason` | string | no | Reason why the delivery failed. |    

  ### Outreach Message MetaData Object
  MetaData object is represented as simple flat JSON objects with the following keys:
  
  | Name | Type | Required | Description                                           |     
  | - | - | - | - | 
  | `ishtmlMessage` | bool | no | Whether message html or not. Note: Default value will be true | 
  | `reference` | string | no | Refernce key for outreach service. Note: will used to check any ticket is already exist or not |
  | `subject` | string | no | Subject of the email message. |
