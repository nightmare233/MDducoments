# Authentication 
- Comm100 anytime Conversation API provides 2 authentication methods: 
    - API_key Authentication 
    - OAuth Authentication 
- [document](https://www.comm100.com/doc/api/introduction.htm#/) 

# Parameter introduction 
- Incoming parameters:
    - Get API passes parameters through the query string 
    - Put/Post API passes parameters through json data. 
    - DateTime Parameters: 
        - All time parameters need to be entered in the standard format of <a href="https://en.wikipedia.org/wiki/ISO_8601" target="_blank">ISO-8601</a>
- All time values are UTC time. The caller can convert to different time zones where needed. 

# Includes
- Following APIs can use `Includes` as parameters to get related objects.

    | Endpoints | Support including parameters |
    | - | - |
    | `get api/v3/anytime/conversations` | assignedAgent, assignedDepartment, contact, createdBy, lastRepliedBy |
    | `get api/v3/anytime/conversations/{id}` | assignedAgent, assignedDepartment, contact, createdBy, lastRepliedBy, messages |
    | `get api/v3/anytime/conversations/{id}/messages` | sender |
    | `get api/v3/anytime/deletedConversations` | assignedAgent, assignedDepartment, contact, createdBy, lastRepliedBy |
    | `get api/v3/anytime/deletedConversations/{id}` | assignedAgent, assignedDepartment, contact, createdBy, lastRepliedBy, messages |
    | `get api/v3/anytime/deletedConversations/{id}/messages` | sender |
    | `get api/v3/anytime/portalConversations/{id}` | contact, messages |
    | `get api/v3/anytime/portalConversations` | contact |
    | `get api/v3/anytime/portalConversations/{id}/messages` | sender | 

- Sample:
    - request: `get api/v3/anytime/conversations/{id}?include=assignedAgent,contact,createdBy,messages`
    - response:

        ``` javascript
        {
            "conversation": {
                "id": 1,
                "assignedAgentId": 1,
                "assignedAgent": {  //included the agent object
                    "id": 1,
                    //...
                },
                "contactId":2
                "contact": {  //included the contact object
                    "id": 2,
                    //...
                },
                "createdById": 3,
                "createdByType": "agent",
                "createdBy": {  //included the agent or contact object according to the createdByType.
                    "id": 3,
                    //...
                },
                "messages":[    //included the messages.
                    {
                        "id": 56, 
                        //...
                    },
                    {
                          "id": 57, 
                        //...
                    }
                ]
            //...
            }
        }, 
        ``` 

# Resource List 
|Name|EndPoint|Note| 
|---|---|---| 
|[Conversation](#conversations)|/api/v3/anytime/conversations| Points for agent console| 
|[PortalConversation](#portalConversations)|/api/v3/anytime/portalConversations| Points for portal and contacts |
|[Attachment](#attachments)|/api/v3/anytime/attachments| Upload attachment for conversations | 
|[Filter](#filters)|/api/v3/anytime/filters| Agent console filters| 
|[Field](#fields)|/api/v3/anytime/fields| System fields and custom fields | 
|[BlockedSender](#blockedsenders)|/api/v3/anytime/blockedSenders|Blocked email or domain| 
|[Config](#config)|/api/v3/anytime/configs| Get site settings| 
|[Junk](#junks)|/api/v3/anytime/junks| Emails from blocked senders| 
|[IntegrationAccount](#integration-accounts)|/api/v3/anytime/integrationAccounts| integration accounts| 
|[Report](#reports)|/api/v3/anytime/reports| Anytime conversation reports| 

# Conversations 
## objects 
### conversation 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id of conversation | 
| `guid` | string | guid of conversation | 
| `relatedType` | string | `contact`, `visitor`, `agent`| 
| `relatedId` | string | contact id, visitor id, agent id | 
| `subject` | string | conversation subject | 
| `assignedAgentId` | string | agent assignee id | 
| `assignedDepartmentId` | string | department assignee id | 
| `channel` | string | `portal`, `email`, `chat`, `offlinemessage`, `facebookMessenger`, `facebookWallPost`, `Tweet`, etc.| 
| `receivedBy` | string | receiving intergrated account id | 
| `originalId` | string | original id on social platform | 
| `originalLink` | string | original link on social platform | 
| `priority` | string | `urgent`, `high`, `normal`, `low` | 
| `status` | string | `new`, `pendingInternal`, `pendingExternal`, `onHold`, `closed` | 
| `hasDraft` | boolean | if has draft | 
| `isDeleted` | boolean | if deleted | 
| `isRead` | boolean | if the conversation is read | 
| `isReadByContact` | boolean | if the portal conversation is read by contact |
| `isEditable`| boolean | if the current agent can update\reply the conversation | 
| `isActive`| boolean | if open in my active work area by agent | 
| `lastMessage`| string | plain text of last message | 
| `tagIds` | string[] | tag id array | 
| `mentionedAgents`|[mentioned agent](#mentioned-agent)[]| mentioned agents list | 
| `customFields` | [custom field value](#custom-field-value)[] | custom field value array | 
| `createdById` | string | contact id or agent id or visitor id| 
| `createdByType` | string | agent or contact or system or visitor | 
| `createdTime` | datetime | create time of conversation | 
| `lastActivityTime` | datetime | last activity time of conversation | 
| `lastStatusChangeTime` | datetime | last status change time of conversation | 
| `lastRepliedTime` | datetime | last replied time | 
| `lastRepliedById` | integer | contact id or agent id | 
| `lastRepliedByType` | string | `agent` or `contact` or `system`| 
| `slaPolicyId` | string | SLA id of this conversation matched | 
| `firstRespondBreachAt` | datetime | Timestamp that denotes when the first response is due | 
| `nextRespondBreachAt` | datetime | Timestamp that denotes when the next response is due | 
| `resolveBreachAt` | datetime | Timestamp that denotes when the conversation is due to be resolved | 

### custom field value
| Name | Type | Description | 
| - | - | - | 
| `id` | string | the id of custom field |
| `name` | string | the name of custom field |
| `value` | string | the value of custom field |

### mentioned agent 
| Name | Type | Description | 
| - | - | - | 
| `agentId` | string | the agent id of mentioned | 
| `isRead`| boolean | if the mentioned conversation is read | 
| `messageId`| string | message id| 

### message 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | id of message | 
| `conversationId` | integer | id of conversation | 
| `type` | string | `note`, `email`, `reply`, `socialMessage`, `chat`, `offlineMessage` |
| `directType` | string | `receive`, `send` |
| `accountId`| string | integrated account id | 
| `contactIdentityId`| string | id of contact identity |
| `source` | string | `agentConsole`, `helpDesk`, `webForm`, `API`, `chat`, `offlineMessage` | 
| `originalMessageId` | string | original message id|
| `originalMessageLink` | string | origial message link |
| `parentId` | string | parent id |
| `quoteTweetId` | string | quote tweet id |  
| `texts` | [text](#text)[] | text of message |  
| `quote` | string | quoted content of the message, only for email message |  
| `subject` | string | subject | 
| `cc` | string | cc email addresses |  
| `attachments` | [attachment](#attachment)[] | attachment array| 
| `mentionedAgentIds` | integer[] | only for Note, @mentioned agents id array |
| `isRead`| boolean | | 
| `sendStatus` | string | `sucess`, `sending`, `fail` |
| `sendertId`| string | id of agent or contact | 
| `senderType`| string | `agent` or `contact` or `system` | 
| `time` | datetime | the sent time of the message | 
 
### conversation draft 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | id of message draft | 
| `conversationId` | integer | id of conversation | 
| `type` | string | `note`, `email`, `reply`, `socialMessage` |
| `accountId`| string | integrated account id | 
| `contactIdentityId`| string | id of contact identity |
| `parentId` | string | parent id |
| `quoteTweetId` | string | quote tweet id |  
| `texts` | [text](#text)[] | text of message |  
| `quote` | string | quoted content of the message, only for email message |  
| `subject` | string | subject | 
| `cc` | string | cc email addresses |  
| `attachments` | [attachment](#attachment)[] | attachment array| 
| `mentionedAgentIds` | integer[] | only for Note, @mentioned agents id array |
| `sendertId`| string | id of agent| 
| `time` | datetime | the sent time of the message | 

### text
| Name | Type | Description | 
| - | - | - |
| `id` | string | id |
| `format` | string | `plaintext`, `html` |
| `content` | string | plain text or html body |


### attachment 
| Name | Type | Description | 
| - | - | - |
| `id` | string | attachment unique id |
| `messageId` | string | message id |
| `type` | string | attachment type |
| `mimetype` | string | attachment mime type |
| `originalId` | string | original id |
| `originalLink` | string | original link |
| `text` | string | attachment text or description |
| `fileName` | string | attachment file name| 
| `url` | string | attachment download link | 
| `previewUrl` | string | preview url | 
| `size` | int | attachment size |
| `scale` | string | scale for location |
| `isAvailable` | boolean | if the attachment is available | 

## endpoints 
### List conversations 
`get api/v3/anytime/conversations` 
+ Each request returns a maximum of 50 conversations. 
+ Parameters 
    - filterId: string, filter id  
    - tagId: string, tag id
    - keywords: string
    - timeFrom: DateTime, last reply time, default search the last 30 days
    - timeTo: DateTime, last reply time, default value is the current time
    - timeZoneOffset, float, time zone of your time parameters
    - pageIndex: integer
    - sortBy: string, `nextSLABreach`, `lastReplyTime`, `lastActivityTime`, `priority`, `status` , default value: `lastReplyTime`
    - sortOrder: string, `ascending` or `descending`, default value: `descending`
    - conditions: parameter format: `conditions[0][field]=subject&conditions[0][matchType]=is&conditions[0][value]=hi&conditions[1][field]=status&conditions[1][matchType]=is&conditions[1][value]=new`, fields can be conversation system fields and custom fields.
        - field: string, field name
        - matchType: string 
        - value: string
+ Response 
    - conversations: [conversation](#conversations) list, 
    - total: integer, total number of conversations 
    - previousPage: string, next page uri, the first page return null. 
    - nextPage: string, the last page return null. 
    - currentPage: string, current page uri. 
+ Includes

    | Includes | Description |
    | - | - |
    | assignedAgent | `get api/v3/anytime/conversations?include=assignedAgent` |
    | assignedDepartment | `get api/v3/anytime/conversations?include=assignedDepartment` |
    | contact | `get api/v3/anytime/conversations?include=contact` |
    | createdBy | `get api/v3/anytime/conversations?include=createdBy` |
    | lastRepliedBy | `get api/v3/anytime/conversations?include=lastRepliedBy` | 

### Get a conversation 
`get api/v3/anytime/conversations/{id} ` 
+ Parameters 
    - id: integer, conversation  
+ Response 
    - conversation: [conversation](#conversation) 
+ Includes

    | Includes | Description |
    | - | - |
    | assignedAgent | `get api/v3/anytime/conversations/{id}?include=assignedAgent` |
    | assignedDepartment | `get api/v3/anytime/conversations/{id}?include=assignedDepartment` |
    | contact | `get api/v3/anytime/conversations/{id}?include=contact` |
    | createdBy | `get api/v3/anytime/conversations/{id}?include=createdBy` |
    | lastRepliedBy | `get api/v3/anytime/conversations/{id}?include=lastRepliedBy` |
    | messages | `get api/v3/anytime/conversations/{id}?include=messages` |
 
### Submit a new conversation 
`post api/v3/anytime/conversations` 
- Parameters 
    - subject: string, conversation subject, required
    - channel: string, `portal`, `email`, `chat`, `facebookMessenger`, etc.  required 
    - relatedType: string, `contact`, `visitor`
    - relatedId: string, contact id or visitor id
    - assignedAgentId: string, agent id
    - assignedDepartmentId: string, department id
    - priority: string, `urgent`, `high`, `normal`, `low`, default value: `normal` 
    - status: string, `new`, `pendingInternal`, `pendingExternal`, `onHold`, `closed`, default value: `new` 
    - receivedBy: string,
    - customFields: [custom field value](#custom-field-value)[], custom field value array
    - tagIds: string[], tag id array
    - message: the first message of the conversation, required
        - type: string, `note`, `email`, `reply`, `socialMessage`, required
        - subject: string, for email message, email subject
        - text: [text](#text), 
        - from: string, for email type message, one of email account address 
        - cc: string, message cc emails 
        - attachments: [attachment](#attachment)[], attachment array
+ Response 
    - conversation: [conversation](#conversations)

### List messages of a conversation 
`get api/v3/anytime/conversations/{id}/messages` 
+ Parameters 
    - id: integer, conversation id 
+ Response 
    - messages: [message](#message) list 
+ Includes

    | Includes | Description |
    | - | - |
    | sender | `get api/v3/anytime/conversations/{id}/messages?include=sender` |

### Update a conversation 
`put api/v3/anytime/conversations/{id}` 
- Parameters 
    - id: integer, conversation id
    - subject: string, conversation subject
    - relatedType: string, `contact`, `visitor`
    - relatedId: string, contact id or visitor id
    - assignedAgentId: string, agent id
    - assignedDepartmentId: string, department id
    - priority: string, priority: `urgent`, `high`, `normal`, `low`
    - status: string, `new`, `pendingInternal`, `pendingExternal,`, `onHold`, `closed`
    - isRead: boolean
    - isActive: boolean
    - customFields: [custom field value](#custom-field-value)[], custom field value array
    - tagIds: integer[], tag id array
- Response 
    - conversation: [conversation](#conversation) 

### Batch update conversations
`put api/v3/anytime/conversations/` 
+ Parameters 
    - ids: integer[], conversation id array
    - status, string
    - priority, string
    - assignedAgentId, string
    - assignedDepartmentId, string
    - isRead, boolean
+ Response 
    - conversations: [conversation](#conversation) list 

### Reply a conversation 
`post api/v3/anytime/conversations/{id}/messages` 
- Parameters  
    - type: string, `note`, `email`, `reply`, `socialMessage`, required
    - accountId: string, channel account id,
    - contactIdentityId: string, contact identity id,
    - subject: string, for email message, email subject
    - text: [text](#text) 
    - quote: string, quote content, only for email message
    - from: string, for email type message, one of email account address 
    - cc: string, message cc emails 
    - parentId: string,
    - quoteTweetId: string,
    - sendByType: string, `agent`, 
    - sendById: string, agent id
    - attachments: [attachment](#attachment)[], attachment array
- Response 
    - message: [message](#message) 

### Mark a conversation as read 
`put api/v3/anytime/conversations/{id}/read` 
+ Parameters 
    - id: integer, conversation id 
+ Response 
    - http status code

### Mark a conversation as unread 
`put api/v3/anytime/conversations/{id}/unread` 
+ Parameters 
    - id: integer, conversation id 
+ Response 
    - http status code

### Mark a message as read 
`put api/v3/anytime/conversations/messages/{id}/read` 
+ Parameters 
    - id: string, message id 
+ Response 
    - http status code

### Mark a message as unread 
`put api/v3/anytime/conversations/messages/{id}/unread` 
+ Parameters 
    - id: string, message id 
+ Response 
    - http status code

### Delete a conversation 
`delete api/v3/anytime/conversations/{id}` 
- Parameters 
    - id: integer, conversation id
- Response 
    - http status code 

### Batch delete conversations 
`delete api/v3/anytime/conversations` 
+ Parameters 
    - ids: integer[], id array
+ Response 
    - http status code 

### List deleted conversations 
`get api/v3/anytime/deletedConversations/` 
- Parameters 
    - keywords: string
    - pageIndex: integer
    - timeFrom: DateTime, last reply time, default search the last 30 days
    - timeTo: DateTime, last reply time, defautl value is the current time
- Response 
    - deletedConversations: [conversation](#conversation) list 
    - total: integer, total number of conversations 
    - previousPage: string, next page uri, the first page return null. 
    - nextPage: string, the last page return null. 
    - currentPage: string, current page uri. 
- Includes

    | Includes | Description |
    | - | - |
    | assignedAgent | `get api/v3/anytime/deletedConversations?include=assignedAgent` |
    | assignedDepartment | `get api/v3/anytime/deletedConversations?include=assignedDepartment` |
    | contact | `get api/v3/anytime/deletedConversations?include=contact` |
    | createdBy | `get api/v3/anytime/deletedConversations?include=createdBy` |
    | lastRepliedBy | `get api/v3/anytime/deletedConversations?include=lastRepliedBy` | 

### Get a deleted conversation 
`get api/v3/anytime/deletedConversations/{id}` 
- Parameters 
    - id: integer, conversation id 
- Response 
    - deletedConversation: [conversation](#conversation) 
- Includes

    | Includes | Description |
    | - | - |
    | assignedAgent | `get api/v3/anytime/deletedConversations/{id}?include=assignedAgent` |
    | assignedDepartment | `get api/v3/anytime/deletedConversations/{id}?include=assignedDepartment` |
    | contact | `get api/v3/anytime/deletedConversations/{id}?include=contact` |
    | createdBy | `get api/v3/anytime/deletedConversations/{id}?include=createdBy` |
    | lastRepliedBy | `get api/v3/anytime/deletedConversations/{id}?include=lastRepliedBy` |
    | messages | `get api/v3/anytime/deletedConversations/{id}?include=messages` |

### List messages of a deleted conversation
`get api/v3/anytime/deletedConversations/{id}/messages` 
- Parameters 
    - id: integer, conversation id
- Response 
    - messages: [message](#message) 
- Includes

    | Includes | Description |
    | - | - |
    | sender | `get api/v3/anytime/deletedConversations/{id}/messages?include=sender` |


### Restore a deleted conversation 
`post api/v3/anytime/deletedConversations/{id}/restore ` 
- Parameters 
    - id: integer, conversation id 
- Response 
    - deletedConversation: [conversation](#conversation)  

### Delete a conversation permanently 
`delete api/v3/anytime/deletedConversations/{id}` 
- Parameters 
    - id: integer, conversation id 
- Response 
    - http status code 

### Get a conversation draft 
`get api/v3/anytime/conversations/{id}/draft` 
- Parameters 
    - id: integer, conversation id 
- Response 
    - conversationDraft: [conversation draft](#conversation-draft) 

### Create a conversation draft 
`post api/v3/anytime/conversations/{id}/draft` 
- Parameters 
    - [conversation draft](#conversation-draft) 
- Response 
    - conversationDraft: [conversation draft](#conversation-draft) 

### Update a conversation draft 
`put api/v3/anytime/conversations/{id}/draft` 
- Parameters 
    - [conversation draft](#conversation-draft) 
- Response 
    - conversationDraft: [conversation draft](#conversation-draft) 

### Delete a conversation draft 
`delete api/v3/anytime/conversations/{id}/draft` 
- Parameters 
    - id: integer, conversation id 
- Response 
    - http status code 

### Merge a conversation 
`post api/v3/anytime/conversations/{id}/merge`
- Parameters 
    - id: integer, target conversation id, 
    - sourceId: integer, source conversation id 
- Response 
    - conversation: [conversation](#conversation) 

### List unread conversations number for filters 
`get api/v3/anytime/conversations/unreadCount?filterIds={filterid1}&filterIds={filterid2}&filterIds={filterid3}`
- Parameters 
    - filterIds: filter id array 
- Response 
    - allCount: integer, all unread conversation number. 
    - array including: 
        - filterId: string, filter id 
        - unreadCount: integer, count unread conversations of a filter 
        - unreadMentionedCount: integer, the number of conversations which is unread and mentioned to me 

# PortalConversation
## objects
### portal conversation
| Name | Type | Description |
| - | - | - |
| `id` | integer | id of conversation |
| `subject` | string | subject |
| `contactId` | string | id of the contact who submitted the portal conversation |
| `isClosed` | boolean | if the portal conversation is closed |
| `isReadByContact` | boolean | if the portal conversation is read by contact |
| `customFields` | [custom field value](#custom-field-value)[] | custom field value array |
| `createdTime` | datetime | create time |
| `closedTime` | datetime | close time |

### portal conversation message 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | id of message | 
| `text` | [text](#text) | text |  
| `senderId`| string | id of agent or contact | 
| `senderType`| string | `agent` or `contact` or `system` | 
| `time` | datetime | |   
| `attachments` | [attachment](#attachment)[] | attachment array| 

## endpoints
### Get a portal conversation by id
`get api/v3/anytime/portalConversations/{id}`
- Parameters
    - id, integer, portal conversation id
    - contactId, string
- Response
    - portalConversation: [portal conversation](#portal-conversation) 
- Includes

    |Includes| Description |
    | - | - |
    | contact | `get api/v3/anytime/portalConversations/{id}?include=contact` | 
    | messages | `get api/v3/anytime/portalConversations/{id}?include=messages` |
 
### List portal conversations
`get api/v3/anytime/portalConversations`
- Parameters:
    - contactIds, string array, required
- Response: 
    - portalConversations: [portal conversation](#portal-conversation) list
- Includes

    |Includes| Description |
    | - | - |
    | contact | `get api/v3/anytime/portalConversations?include=contact` |  
- Sample
    - `get api/v3/anytime/portalConversations?contactIds=1&contactIds=2&contactIds=3`

### Submit a portal conversation
`post api/v3/anytime/portalConversations`
- Parameters: 
    - subject: string, subject, required
    - contactId: string, id of the contact who submitted the portal conversation
    - customFields: [custom field value](#custom-field-value)[], custom field value array
    - message:  the first portal message
        - text: [text](#text), 
        - attachments: [attachment](#attachment)[], attachment array of message
- Response: 
  - portalConversation: [portal conversation](#portal-conversation) 

### Close a portalConversation
`put api/v3/anytime/portalConversations/{id}/close` 
- Parameters: 
    - id, integer, portal conversation id,
    - contactId, string, required
- Response: 
    - portalConversation: [portal conversation](#portal-conversation) 

### Reopen a portalConversation
`put api/v3/anytime/portalConversations/{id}/reopen` 
- Parameters: 
    - id, integer, portal conversation id,
    - contactId, string, required
- Response: 
    - portalConversation: [portal conversation](#portal-conversation) 

### List messages of a portal conversation 
`get api/v3/anytime/portalConversations/{id}/messages`
- Parameters: 
    - id, integer, conversation id
    - contactId, string, contact id
- Response: 
    - messages: [portal conversation message](#portal-conversation-message) list
- Includes

    |Includes| Description |
    | - | - |
    | sender| `get api/v3/anytime/portalConversations/{id}/messages?include=sender` | 

### Reply a portal conversation
 `post api/v3/anytime/portalConversations/{id}/messages`
- Parameters:
    - id: integer
    - contactId: string required
    - text: [text](#text), 
    - attachments: [attachment](#attachment)[], attachment array
- Response: 
    - message: [portal conversation message](#portal-conversation-message)

### Contact mark a portal conversation as read
`put api/v3/anytime/portalConversations/{id}/read`
- Parameters 
    - id: integer, conversation id 
- Response 
    - http status code

### Contact mark a portal conversation as unread
`put api/v3/anytime/portalConversations/{id}/unread`
- Parameters 
    - id: integer, conversation id 
- Response 
    - http status code

# Attachments  
## endpoints 
### Upload attachment 
`post /api/v3/anytime/attachments` 
- Content-type
    - multipart/form-data
- Parameters 
    - file: file
- Response 
    - attachment: [attachment](#attachment) 
    
### Update status of attachment
`Put /api/v2/livechat/attachments/{guid}`
#### Parameters:
    - isAvailable - boolean `true` or  `false`
#### Response
 - attachment: [attachment](#attachment) 

### Delete attachment 
`delete /api/v3/anytime/attachments/{guid}` 
- Parameters 
    - guid: string, the guid of the attachment
- Response 
    - httpStatusCode


# Filters 
## objects 
### filter 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | filter id | 
| `name` | string | filter name | 
| `isPrivate` | boolean | if private filter| 
| `createdById` | string | agent id | 
| `conditions` | [condition](#condition)[] | array of filter condition | 

### condition 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | condition id | 
| `fieldId` | string | field id | 
| `matchType` | string | `contains`, `notContains`, `is`, `isNot`, `isMoreThan`, `isLessThan`, `before`, `after` | 
| `value` | string | condition value | 

## endpoints 
### List all public and private filters 
`get /api/v3/anytime/filters`
- Parameters 
    - no parameters 
- Response 
    - filters: [filter](#filter) list, without conditions

### Create a new filter 
`post api/v3/anytime/filters`
- Parameters 
    - name: string, filter name, required 
    - isPrivate: boolean, if private filter, default value: `false` 
    - conditions: [condition](#condition)[], array of filter condition
- Response 
    - filters: [filter](#filter) list 

### Get a filter and its conditions 
`get api/v3/anytime/filters/{id}` 
- Parameters 
    - id: string, filter id 
- Response 
    - filter: [filter](#filter) 

### Update a filter 
`put api/v3/anytime/filters/{id}` 
- Parameters 
    - id: string, filter id 
    - name: string, filter name, required 
    - isPrivate: boolean, if private filter 
    - conditions: [condition](#condition)[], array of filter condition
- Response 
    - filter: [filter](#filter) 

### Delete a filter 
`delete api/v3/anytime/filters/{id}` 
- Parameters 
    - id: string, filter id 
- Response 
    - http status code 


# Fields 
## objects 
### field 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | field id | 
| `type` | string | `text`, `textarea`, `email`, `url`, `date`, `integer`, `float`, `operator`, <br/>`radio`, `checkbox`, `dropdownList`, `checkboxList`, `link`, `department` | 
| `name` | string | field name | 
| `isSystemField` | boolean | if is system field | 
| `isRequired` | boolean | value if is required | 
| `defaultValue` | string | field default value | 
| `helpText` | string | field help text | 
| `length` | integer | field value max length | 
| `options` | [field option](#fieldoption)[] | value option | 

### fieldOption 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | option id | 
| `name` | string | option name | 
| `value` | string | field value | 
| `order` | integer | option order | 

## endpoints 
### List all fields and their options 
`get api/v3/anytime/fields` 
- Parameters
    - no parameters
- Response 
    - fields: [field](#field) list 

# BlockedSenders 
## objects 
### blocked sender 
| Name | Type | Description | 
| - | - | - | 
| `email` | string | email or domain | 
| `blockType` | string | `blockEmailasJunk`, `rejectEmail`, `blockDomainasJunk`, `rejectDomain` | 

## endpoints 
### List blocked senders 
`get /api/v3/anytime/blockedSenders` 
- Parameters 
    - email: string, domain or email address 
- Response 
    - blockedSenders: [block sender](#blocked-sender) list 

### Add/update a block sender 
`put api/v3/anytime/blockedSenders` 
- Parameters 
    - `email`, string, domain or email address 
    - `blockType`, string, `blockEmailasJunk`, `rejectEmail`, `blockDomainasJunk`, `rejectDomain`
- Response 
    - blockedSender: [block sender](#blocked-sender) 

### Remove a block sender 
`delete api/v3/anytime/blockedSenders` 
- Parameters 
   - email: string, domain or email address 
- Response 
    - http status code


# Configs 
### Get site configs about conversation 
`get api/v3/anytime/configs` 
- Parameters 
    - no parameters
- Response 
    - configs
        - isEnabledDepartment: boolean 
        - recipientLimitPerEmail: integer 

# Junks 
## objects 
### junk 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | id of junk | 
| `type` | string | `note`, `email`, `reply`, `socialMessage` |
| `accountId`| string | integrated account id | 
| `contactIdentityId`| string | id of contact identity |
| `source` | string | `agentConsole`, `helpDesk`, `webForm`, `API`, `chat`, `offlineMessage` | 
| `originalMessageId` | string | original message id|
| `originalMessageLink` | string | origial message link |
| `parentId` | string | parent id |
| `quoteTweetId` | string | quote tweet id |  
| `texts` | [text](#text)[] | text |   
| `subject` | string | subject | 
| `cc` | string | cc email addresses |  
| `attachments` | [attachment](#attachment)[] | attachment array| 
| `isRead`| boolean | | 
| `sendertId`| string | id of agent or contact | 
| `senderType`| string | `agent` or `contact` or `system` | 
| `time` | datetime | the sent time of the message | 

## endpoints 
### List junk emails
`get api/v3/anytime/junks` 

- Parameters 
    - keywords: string
    - pageIndex: integer
    - timeFrom: DateTime
    - timeTo: DateTime
- Response 
    - junks: [junk](#junk) list 
    - total: integer
    - previousPage: string, next page uri, the first page return null
    - nextPage: string, the last page return null
    - currentPage: string

### Get a junk email 
`get api/v3/anytime/junks/{id}` 
- Parameters 
    - id: integer, email id 
- Response 
    - junk: [junk](#junk) 

### Update a junk 
`put api/v3/anytime/junks/{id}` 
- Parameters 
    - isRead: boolean, 
- Response 
    - junk: [junk](#junk) 

### Restore a junk email to a normal conversation 
`post api/v3/anytime/junks/{id}/notJunk` 
- Parameters 
    - id: integer, email id 
- Response 
    - conversation: [conversation](#conversation) 

### Delete a junk email 
`delete api/v3/anytime/junks/{id}` 
- Parameters 
    - id: integer, junk email id 
- Response 
    - http status code 

# Integration Accounts 

## objects 

### integrationAccount 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | id | 
| `name` | string | account name |   
| `channelId` | string | channel id |   

## endpoints 
### List integration accounts 
`get api/v3/anytime/integrationAccounts` 
- Parameters
    - channelId: string, optional
- Response 
    - integrationAccounts: [integrationAccount](#integrationAccount)[] 

### Add an integration account 
`post api/v3/anytime/integrationAccounts` 
- Parameters
    - integrationAccount: [integrationAccount](#integrationAccount) 
- Response 
    - integrationAccount: [integrationAccount](#integrationAccount)

### Update an integration account 
`put api/v3/anytime/integrationAccounts/{id}` 
- Parameters
    - name: string 
- Response 
    - integrationAccount: [integrationAccount](#integrationAccount)

### Delete an integration account 
`delete api/v3/anytime/integrationAccounts/{id}` 
- Response 
    - http status code


***

# Reports

## EndPoint List 
| endpoint | type | note |
| --- | --- | --- |
| [/api/v3/anytime/reports/realtime/now](#right-now) | GET | real-time report of now |
| [/api/v3/anytime/reports/realtime/today](#realtime-today) | GET | real-time report of today |
| [/api/v3/anytime/reports/realtime/departments](#realtime-department) | GET | real-time report of departments |
| [/api/v3/anytime/reports/realtime/agents](#realtime-agent) | GET | real-time report of agents |
| [/api/v3/anytime/reports/volume/export](#export-volume) | GET | export volume report data |
| [/api/v3/anytime/reports/volume](#report-volume) | GET | report of volume |
| [/api/v3/anytime/reports/channel/export](#export-channel) | GET | export channel report data |
| [/api/v3/anytime/reports/channel](#report-channel) | GET | channel report |
| [/api/v3/anytime/reports/efficiency/export](#export-efficiency) | GET | export efficiency report |
| [/api/v3/anytime/reports/efficiency](#report-efficiency) | GET | export efficiency report data |

| [/api/v3/anytime/reports/sla](#report-sla) | GET | export SLA report data |

## EndPoints

### right now
`GET /api/v3/anytime/reports/realtime/now`
- Parameters：
	- siteId: number
- Return：
 	- unassignedConversations: number
	- openConversations: number
	- newConversations: number
	- pendingInternalConversations: number
	- pendingExternalConversations: number
	- onHoldConversations: number
	- urgentConversations: number
	- highConversations: number

### realtime today
- `GET /api/v3/anytime/reports/realtime/today`
- Parameters:
	- siteId: number
- Return：
 	- createdConversations: number
	- closedConversations: number
	- repliedConversations: number
	- reopenedConverstations: number

### realtime department
`GET /api/v3/anytime/reports/realtime/departments`
- Parameters：
	- siteId: number
- Return：
	- dataList:
		- departmentId: number,
		- openConversations: number,
		- todayRepliedConversations: number,
		- todayClosedConversations: number,
		- newConversations: number,
		- pendingInternalConversations: number,
		- pendingExternalConversations: number,
		- onHoldConversations: number,
		- urgentConversations: number,
		- highConversations: number,

### realtime agent
`GET/api/v3/anytime/reports/realtime/agents`
- Parameters：
	- siteId: number,
	- filterType: string, `site`, `agent`, `department`, `account`, `channel`
	- filterValue: number,
- Return：
	- dataList: 
		- agentId: number,
		- openConversations: number,
		- todayRepliedConversations: number,
		- todayClosedConversations: number,
		- newConversations: number,
		- pendingInternalConversations: number,
		- pendingExternalConversations: number,
		- onHoldConversations: number,
		- urgentConversations: number,
		- highConversations: number,
	
### export volume
`GET /api/v3/anytime/reports/volume/export`
- Parameters：
	- siteId: number,
    - startTime: Datetime,
    - endTime: Datetime,
    - filterType: string, `site`, `agent`, `department`, `account`, `channel`
	- filterValue: number,
    - timeUnit: string,
    - dimensionType: string,
    - timeOffset： number,
    - dateFormat: string,
- Return：csv file


### report volume
`GET /api/v3/anytime/reports/volume`
- Parameters：
    - siteId: number,
    - startTime: Datetime,
    - endTime: Datetime,
    - filterType: string, `site`, `agent`, `department`, `account`, `channel`
	- filterValue: number,
    - timeUnit: string,
    - dimensionType: string,
- Return：
	- dataList:
	    - id: number,
    	- createdConversations: number,
    	- createdConversationsPercentage:  float,	
    	- closedConversations: number,
    	- closedConversationsPercentage: float,
    	- repliedConversations: number,
    	- repliedConversationsPercentage: float,
    	- reopenedConversations: number,
    	- reopenedConversationsPercentage: float,
    	- openConversations: number,
    	- openConversationsPercentage: float,
    	- startTime: string,
    	- endTime: string,
	- totalCreatedConversations: number,
	- totalClosedConversations: number,
	- totalRepliedConversations: number,
	- totalReopenedConversations: number,
	- totalOpenConversations: number

### export channel
`GET /api/v3/anytime/reports/channel/export`
- Parameters：
	- siteId: number,
    - startTime: Datetime,
    - endTime: Datetime,
    - filterType: string, `site`, `agent`, `department`, `account`, `channel`
	- filterValue: number,
    - timeUnit: string,
    - dimensionType: string,
    - timeOffset： number,
    - dateFormat: string,
- Return：csv file

### report-channel
`GET /api/v3/anytime/reports/channel`
- Parameters：
	- siteId: number,
    - startTime: Datetime,
    - endTime: Datetime,
    - filterType: string, `site`, `agent`, `department`, `account`, `channel`
	- filterValue: number,
    - timeUnit: string,
    - dimensionType: string,
- Return：
	- dataList: 
	    - id: number,
		- name: string,
		- channelEfficiencies, [channelEfficiencie](#channel-efficiency)[]
		- startTime: string,
		- endTime: string,
	- channel total number, [channel total number](#Channel-totoal-messages-number)[]

### Channel Efficiency
| Name | Type | Description | 
| - | - | - | 
| `channelId` | string | channel id |
| `channelName` | number | channel name |
| `channelPercentage` | float |  |

### Channel totoal messages number
| Name | Type | Description | 
| - | - | - | 
| `channelId` | string | channel id |
| `channelName` | number | channel name |
| `totalNumber` | number |  |

### export efficiency
`GET /api/v3/anytime/reports/efficiency/export`
- Parameters：
	- siteId: number,
    - startTime: Datetime,
    - endTime: Datetime,
    - filterType: string, `site`, `agent`, `department`, `account`, `channel`
	- filterValue: number,
    - timeUnit: string,
    - dimensionType: string,
    - timeOffset： number,
    - dateFormat: string,
- Return：csv file

### report efficiency
`GET /api/v3/anytime/reports/efficiency`
- Parameters：
	- siteId: number,
    - startTime: Datetime,
    - endTime: Datetime,
    - filterType: string, `site`, `agent`, `department`, `account`, `channel`
	- filterValue: number,
    - timeUnit: string,
    - dimensionType: string, `byTime`, `byAgent`, `byDepartment`, `byChannel`
- Return：
	- dataList:
	    - id: number,
		- avgAgentResponseTime: string,
		- avgFirstResponseTime: string,
		- avgConversationTime: string,
		- avgContactMessages: float,
		- avgAgentMessages: float, 
		- startTime: string,
		- endTime: string,
	- totalAvgAgentResponseTime: string,
	- totalAvgFirstResponseTime: string,
	- totalAvgConversationTime: string,
	- totalAvgContactMessages: float,
	- totalAvgAgentMessages: float

### export SLA report
`GET /api/v3/anytime/reports/sla/export`
- Parameters：
	- siteId: number,
    - startTime: Datetime,
    - endTime: Datetime,
    - timeUnit: string, `day`,`week`, `month`
    - dimensionType: string, `slaPolicies`, `agent`, `department`
- Return：
	- dataList:
	    - id: number,
		- firstRespondTimeAchievedPercentage: float,
		- avgFirstRespondTime: float,
		- nextRespondTimeAchievedPercentage: float,
		- avgNextRespondTime: float,
		- ResolveTimeAchievedPercentage: float, 
        - avgResolveTime: fload,
        - breachedTickets: integer, 

### report SLA
`GET /api/v3/anytime/reports/sla`
- Parameters：
	- siteId: number,
    - startTime: Datetime,
    - endTime: Datetime,
    - timeUnit: string, `day`,`week`, `month`
    - dimensionType: string, `slaPolicies`, `agent`, `department`
- Return：
	- dataList:
	   	    - id: number,
		- firstRespondTimeAchievedPercentage: float,
		- avgFirstRespondTime: float,
		- nextRespondTimeAchievedPercentage: float,
		- avgNextRespondTime: float,
		- ResolveTimeAchievedPercentage: float, 
        - avgResolveTime: fload,
        - breachedTickets: integer

* * *

# Abandon API

# Tags 
## objects 
### tag 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `name` | string | tag name | 
| `conversationCount` | integer | the number of conversations with the tag | 
## endpoints 
### List all tags 
`Get api/v3/anytime/tags` 
- Response 
    - tags: [tag](#tag) list 

### Add a tag 
`Post api/v3/anytime/tags` 
- Parameters 
    - name: string, tag name 
- Response 
    - tag: [tag](#tag) 

### Update One Tag 
`Put api/v3/anytime/tags/{id}` 
- Parameters 
    - id: integer, tag id 
    - name: string, tag name 
- Response 
    - tag: [tag](#tag) 

### Delete a tag 
`Delete api/v3/anytime/tags/{id}` 
- Parameters 
    - id: integer, tag id 
- Response 
    - http status code

# CannedResponses 
## objects 
### canned response 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `name` | string | canned response name | 
| `htmlContent` | string | html format content | 
| `textContent` | string | text format content | 


## endpoints 
### List all canned responses 
`get api/v3/anytime/cannedResponses` 
- Parameters 
    - no parameters
- Response 
    - cannedResponses: [Canned responses](#canned-response) list 


# EmailAccounts 
## objects 
### email account 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `email` | string | email address |  
| `type` | string | pop3 or exchange | 
| `assignedAgentId` | integer | agent id | 
| `assignedDepartmentId` | integer | department id | 
| `isDefault` | boolean | if default email account | 


## endpoints 
### List all enabled email accounts 
`get api/v3/anytime/emailAccounts` 
- Response 
    - emailAccounts: [email account](#email-account) list 

# Department 
## objects 
### department 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `name` | string | department name | 
| `description` | string | department description | 
| `members` | [department member](#department-member)[] | department member array | 

### department member 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `name` | string | member name | 
| `type` | string | agent or group | 

## endpoints 
### Get a department 
`get api/v3/anytime/departments/{id} ` 
- Response 
    - department: [department](#department) 

### List all departments 
`get api/v3/anytime/departments` 
- Response 
    - departments: [department](#department) List without department member. 