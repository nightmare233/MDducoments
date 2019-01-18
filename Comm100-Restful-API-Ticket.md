# Authentication 
- Comm100 Ticket API provides 2 authentication methods: 
    - API_key Authentication 
    - OAuth Authentication 
- [Reference document](https://www.comm100.com/doc/api/introduction.htm#/) 

# Parameter explanation 
- Incoming parameters:
    - Get API passes parameters through the query string 
    - Put/Post API passes parameters through json data. 
    - DateTime Parameters: 
        - The input time parameter needs to conform to the standard format of is-8601, for reference: <a href="https://en.wikipedia.org/wiki/ISO_8601" target="_blank">wikipedia</a> 
    - The total size of all of a ticket's attachments cannot exceed 20MB.
- All time values are UTC time and the caller converts as their time zone as required. 

# Includes
- Following APIs support `Includes` to get related objects.

    | Endpoints | Support including parameters |
    | - | - |
    | `get api/v3/tickets` | agentAssignee, departmentAssignee, contact, createdBy, lastRepliedBy |
    | `get api/v3/tickets/{id}` | agentAssignee, departmentAssignee, contact, createdBy, lastRepliedBy, messages |
    | `get api/v3/tickets/{id}/messages` | sender |
    | `get api/v3/deletedTickets` | agentAssignee, departmentAssignee, contact, createdBy, lastRepliedBy |
    | `get api/v3/deletedTickets/{id}` | agentAssignee, departmentAssignee, contact, createdBy, lastRepliedBy, messages |
    | `get api/v3/deletedTickets/{id}/messages` | sender |
    | `get api/v3/portalTickets/{id}` | contact, messages |
    | `get api/v3/portalTickets` | contact |
    | `get api/v3/portalTickets/{id}/messages` | sender | 

- Sample:
    - request: `get api/v3/tickets/{id}?include=agentAssignee&include=contact&include=createdBy&include=messages `
    - response:

        ``` javascript
        {
            "ticket": {
                "id": 1,
                "agentAssigneeId": 12,
                "agentAssignee": {  //included the agent object
                    "id": 12,
                    //...
                },
                "contactId":23
                "contact": {  //included the contact object
                    "id": 23,
                    //...
                },
                "createdById":34,
                "createdByType": "agent",
                "createdBy": {  //included the agent or contact object according to the createdByType.
                    "id": 34,
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
|[Ticket](#ticket)|/api/v3/tickets|| 
|[PortalTicket](#portalTicket)|/api/v3/portalTickets|for contact|
|[Filter](#filter)|/api/v3/tickets/filters|| 
|[Field](#field)|/api/v3/tickets/fields|| 
|[Attachment](#attachment)|/api/v3/tickets/attachments|| 
|[BlockedSender](#blockedsender)|/api/v3/tickets/blockedSenders|blocked email or domain| 
|[CannedResponse](#cannedresponse)|/api/v3/tickets/cannedResponses||
|[Config](#config)|/api/v3/tickets/configs|site settings| 
|[Department](#department)|/api/v3/tickets/departments|| 
|[EmailAccount](#emailaccount)|/api/v3/tickets/emailAccounts|| 
|[JunkEmail](#junkemail)|/api/v3/tickets/junkEmails|emails from <br/>blocked senders| 
|[Tag](#tag)|/api/v3/tickets/tags|| 

# Tickets 
## objects 
### ticket 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id of ticket | 
| `subject` | string | ticket subject | 
| `agentAssigneeId` | integer | agent assignee id | 
| `departmentAssigneeId` | integer | department assignee id | 
| `contactId` | integer | the contact id | 
| `receivedFrom` | string | received email address for email channel | 
| `channel` | string | `portal`, `email`| 
| `priority` | string | `urgent`, `high`, `normal`, `low` | 
| `status` | string | `new`, `pendingInternal`, <br/>`pendingExternal`, `onHold`, `closed` | 
| `isRead` | boolean | if the ticket is read | 
| `customFields` | [custom field value](#custom-field-value)[] | custom field value array | 
| `createdById` | integer | contact id or agent id | 
| `createdByType` |  string | agent or contact or system | 
| `createdTime` | datetime | create time of ticket | 
| `lastActivityTime` | datetime | last activity time of ticket | 
| `lastStatusChangeTime` | datetime | last status change time of ticket | 
| `lastRepliedTime` | datetime | last replied time | 
| `lastRepliedById` | integer | contact id or agent id | 
| `lastRepliedByType` | string | `agent` or `contact` or `system`| 
| `hasDraft` | boolean | if has draft | 
| `tagIds` | integer[] | tag id array | 
| `isDeleted` | boolean | if deleted | 
| `slaPolicyId` | integer | SLA id of this ticket matched | 
| `firstRespondBreachAt` | datetime | Timestamp that denotes when the first <br/> response is due | 
| `nextRespondBreachAt` | datetime | Timestamp that denotes when the next <br/> response is due | 
| `resolveBreachAt` | datetime | Timestamp that denotes when the ticket is <br/> due to be resolved | 
| `mentionedAgents`|[mentioned agent](#mentioned-agent)[]| mentioned agents list | 
| `permissions`|[permission](#permission)[]| the permissions that the current agent has | 

### permission
| Name | Type | Description | 
| - | - | - | 
| `canView` | boolean | can view the ticket |
| `canManage` | boolean | can manage the ticket |
| `canPermanentlyDelete` | boolean | can permanent delete ticket |

### custom field value
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | the id of custom field |
| `name` | string | the name of custom field |
| `value` | string | the value of custom field |

### mentioned agent 
| Name | Type | Description | 
| - | - | - | 
| `agentId` | integer | the agent id of mentioned | 
| `isRead`| boolean | if the mentioned ticket is read | 
| `messageId`| integer| message id| 

### message 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id of message | 
| `type` | string | `note`, `email`, `reply` | 
| `source` | string | `agentConsole`, `helpDesk`, `webForm`, `API`, `chat`, `offlineMessage` | 
| `htmlBody` | string | html body of message | 
| `plainBody` | string | plain text body of message | 
| `quote` | string | quoted content of the message, only for email message | 
| `senderId`| integer | id of agent or contact | 
| `senderType`| string | `agent` or `contact` or `system` | 
| `time` | datetime | | 
| `subject` | string | subject | 
| `from` | string | from email address| 
| `to` | string | | 
| `cc` | string | cc email addresses |  
| `attachments` | [attachment](#attachment)[] | attachment array| 
| `mentionedAgentIds` | integer[] | only for Note, @mentioned agents id array |

### ticket draft 
| Name | Type | Description | 
| - | - | - | 
| `draftId` | integer | id of ticket draft | 
| `ticketId` | integer | id of ticket | 
| `subject` | string | draft subject | 
| `htmlBody` | string | html body of ticket draft | 
| `plainBody` | string | plain text body of ticket draft | 
| `quote` | string | quoted body | 
| `from` | string | from email address | 
| `to` | string | to email address | 
| `cc` | string | cc email addresses | 
| `savedTime` | datetime | | 
| `savedById` | integer | the id of the agent who saved the ticket draft | 
| `attachments` | [attachment](#attachment)[] | draft attachments | 

## endpoints 
### List tickets 
`get api/v3/tickets` 
+ Max 50 tickets are responded for each request. 
+ Parameters 
    - filterId: integer, filter id  
    - tagId: integer, tag id
    - keywords: string
    - pageIndex: integer
    - sortBy: string, `nextSLABreach`, `lastReplyTime`, `lastActivityTime`, `priority`, `status` , default value: `lastReplyTime`
    - sortOrder: string, `ascending` or `descending`, default value: `descending`
    - conditions: parameter format: `conditions[0][field]=agent&conditions[0][matchType]=is&conditions[0][value]=hi&conditions[1][field]=agent&conditions[1][matchType]=is&conditions[1][value]=hello`, fields can be ticket system fields and custom fields.
+ Response 
    - tickets: [ticket](#tickets) list, 
    - total: integer, total number of tickets 
    - previousPage: string, next page uri, the first page return null. 
    - nextPage: string, the last page return null. 
    - currentPage: string, current page uri. 
+ Includes

    | Includes | Description |
    | - | - |
    | agentAssignee | `get api/v3/tickets?include=agentAssignee` |
    | departmentAssignee | `get api/v3/tickets?include=departmentAssignee` |
    | contact | `get api/v3/tickets?include=contact` |
    | createdBy | `get api/v3/tickets?include=createdBy` |
    | lastRepliedBy | `get api/v3/tickets?include=lastRepliedBy` | 

### Get a ticket 
`get api/v3/tickets/{id} ` 
+ Parameters 
    - id: integer, ticket  
+ Response 
    - ticket: [ticket](#ticket) 
+ Includes

    | Includes | Description |
    | - | - |
    | agentAssignee | `get api/v3/tickets/{id}?include=agentAssignee` |
    | departmentAssignee | `get api/v3/tickets/{id}?include=departmentAssignee` |
    | contact | `get api/v3/tickets/{id}?include=contact` |
    | createdBy | `get api/v3/tickets/{id}?include=createdBy` |
    | lastRepliedBy | `get api/v3/tickets/{id}?include=lastRepliedBy` |
    | messages | `get api/v3/tickets/{id}?include=messages` |
 
### Submit new ticket 
`post api/v3/tickets` 
- Parameters 
    - subject: string, ticket subject, required
    - channel: string, `portal`, `email`, required 
    - contactId: integer, contact id
    - agentAssigneeId: integer, agent id
    - departmentAssigneeId: integer, department id
    - priority: string, `urgent`, `high`, `normal`, `low`, default value: `normal` 
    - status: string, `new`, `pendingInternal`, `pendingExternal,`, `onHold`, `closed`, default value: `new`  
    - customFields: [custom field value](#custom-field-value)[], custom field value array
    - tagIds: integer[], tag id array
    - message: the first message of the ticket, required
        - type: string, `note`, `email`, `reply`, required
        - source: string, `agentConsole`, `API`, default value: `API`
        - subject: string, for email message, email subject
        - htmlBody: string, html body of message
        - plainBody: string, plain text body of message
        - from: string, for email type message, one of email account address 
        - cc: string, message cc emails 
        - attachments: [attachment](#attachment)[], attachment array
+ Response 
    - ticket: [ticket](#tickets)

### List ticket messages 
`get api/v3/tickets/{id}/messages` 
+ Parameters 
    - id: integer, ticket id 
+ Response 
    - messages: [message](#message) list 
+ Includes

    | Includes | Description |
    | - | - |
    | sender | `get api/v3/tickets/{id}/messages?include=sender` |

### Update ticket 
`put api/v3/tickets/{id}` 
- Parameters 
    - id: integer, ticket id
    - subject: string, ticket subject
    - contactId: integer, the contact id or agent id
    - agentAssigneeId: integer, agent id
    - departmentAssigneeId: integer, department id
    - priority: string, priority: `urgent`, `high`, `normal`, `low`
    - status: string, `new`, `pendingInternal`, `pendingExternal,`, `onHold`, `closed`
    - isRead: boolean
    - customFields: [custom field value](#custom-field-value)[], custom field value array
    - tagIds: integer[], tag id array
- Response 
    - ticket: [ticket](#ticket) 

### Batch update ticket 
`put api/v3/tickets/` 
+ Parameters 
    - ids: integer[], ticket id array
    - status, string
    - priority, string
    - agentAssigneeId, integer
    - departmentAssigneeId, integer
    - isRead, boolean
+ Response 
    - tickets: [ticket](#ticket) list 

### Reply ticket 
`post api/v3/tickets/{id}/messages` 
- Parameters  
    - type: string, `note`, `email`, `reply`, required
    - source: string, `agentConsole`, `API`, default value: `API`
    - subject: string, for email message, email subject
    - htmlBody: string, html body of message, if you want to @mention an agent in a note, you can use the format: `<span data-id=agentId class="athighlight">note body</span>`
    - plainBody: string, plain text body of message
    - from: string, for email type message, one of email account address 
    - cc: string, message cc emails 
    - attachments: [attachment](#attachment)[], attachment array
- Response 
    - message: [message](#message) 

### Mark ticket as read 
`put api/v3/tickets/{id}/read` 
+ Parameters 
    - id: ticketId 
+ Response 
    - ticket: [ticket](#ticket)

### Mark ticket as unread 
`put api/v3/tickets/{id}/unread` 
+ Parameters 
    - id: ticketId 
+ Response 
    - ticket: [ticket](#ticket)

### Delete a ticket 
`delete api/v3/tickets/{id}` 
- Parameters 
    - id: integer 
- Response 
    - http status code 

### Batch delete tickets 
`delete api/v3/tickets` 
+ Parameters 
    - ids: integer[], id array
+ Response 
    - http status code 

### List deleted tickets 
`get api/v3/deletedTickets/` 
- Parameters 
    - keywords: string
    - pageIndex: integer
    - timeFrom: DateTime, default search the last 30 days
    - timeTo: DateTime, defautl value is the current time
- Response 
    - deletedTickets: [ticket](#ticket) list 
    - total: integer, total number of tickets 
    - previousPage: string, next page uri, the first page return null. 
    - nextPage: string, the last page return null. 
    - currentPage: string, current page uri. 
- Includes

    | Includes | Description |
    | - | - |
    | agentAssignee | `get api/v3/deletedTickets?include=agentAssignee` |
    | departmentAssignee | `get api/v3/deletedTickets?include=departmentAssignee` |
    | contact | `get api/v3/deletedTickets?include=contact` |
    | createdBy | `get api/v3/deletedTickets?include=createdBy` |
    | lastRepliedBy | `get api/v3/deletedTickets?include=lastRepliedBy` | 

### Get a deleted ticket 
`get api/v3/deletedTickets/{id}` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - deletedTicket: [ticket](#ticket) 
- Includes

    | Includes | Description |
    | - | - |
    | agentAssignee | `get api/v3/deletedTickets/{id}?include=agentAssignee` |
    | departmentAssignee | `get api/v3/deletedTickets/{id}?include=departmentAssignee` |
    | contact | `get api/v3/deletedTickets/{id}?include=contact` |
    | createdBy | `get api/v3/deletedTickets/{id}?include=createdBy` |
    | lastRepliedBy | `get api/v3/deletedTickets/{id}?include=lastRepliedBy` |
    | messages | `get api/v3/deletedTickets/{id}?include=messages` |

### List deleted ticket messages
`get api/v3/deletedTickets/{id}/messages` 
- Parameters 
    - id: integer 
- Response 
    - messages: [message](#message) 
- Includes

    | Includes | Description |
    | - | - |
    | sender | `get api/v3/deletedTickets/{id}/messages?include=sender` |


### Restore a deleted ticket 
`post api/v3/deletedTickets/{id}/restore ` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - deletedTicket: [ticket](#ticket)  

### Delete a ticket permanently 
`delete api/v3/deletedTickets/{id}` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - http status code 

### Get ticket draft 
`get api/v3/tickets/{id}/draft` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - ticketDraft: [ticket draft](#ticket-draft) 

### Create ticket draft 
`post api/v3/tickets/{id}/draft` 
- Parameters 
    - [ticket draft](#ticket-draft) 
- Response 
    - ticketDraft: [ticket draft](#ticket-draft) 

### Update ticket draft 
`put api/v3/tickets/{id}/draft` 
- Parameters 
    - [ticket draft](#ticket-draft) 
- Response 
    - ticketDraft: [ticket draft](#ticket-draft) 

### Delete ticket draft 
`delete api/v3/tickets/{id}/draft` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - http status code 

### Merge ticket 
`post api/v3/tickets/{id}/merge`
- Parameters 
    - id: integer, target ticket id, 
    - sourceId: integer, source ticket id 
- Response 
    - ticket: [ticket](#ticket) 

### List unread tickets number of filters 
`get api/v3/tickets/unreadCount?filterIds={filterid1}&filterIds={filterid2}&filterIds={filterid3}`
- Parameters 
    - filterIds: filter id array 
- Response 
    - allCount: integer, all unread ticket number. 
    - array including: 
        - filterId: integer, filter id 
        - unreadCount: integer, count unread tickets of a filter 
        - unreadMentionedCount: integer, the number of tickets which is unread and mentioned to me 

# PortalTicket
## objects
### portal ticket
| Name | Type | Description |
| - | - | - |
| `id` | integer | id of ticket |
| `subject` | string | subject |
| `contactId` | integer | id of the contact who submitted the portal ticket |
| `isClosed` | boolean | if the portal ticket is closed |
| `customFields` | [custom field value](#custom-field-value)[] | custom field value array |
| `createdTime` | datetime | create time |
| `closedTime` | datetime | close time |

### portal ticket message 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id of message | 
| `htmlBody` | string | html body | 
| `plainBody` | string | plain text body | 
| `senderId`| integer | id of agent or contact | 
| `senderType`| string | `agent` or `contact` or `system` | 
| `time` | datetime | |   
| `attachments` | [attachment](#attachment)[] | attachment array| 

## endpoints
### Get a portal ticket by id
`get api/v3/portalTickets/{id}`
- Parameters
    - id, integer, id
    - contactId, integer
- Response
    - portalTicket: [portal ticket](#portal-ticket) 
- Includes

    |Includes| Description |
    | - | - |
    | contact | `get api/v3/portalTickets/{id}?include=contact` | 
    | messages | `get api/v3/portalTickets/{id}?include=messages` |
 
### List portal tickets
`get api/v3/portalTickets`
- Parameters:
    - contactId, integer, required
    - startTime, DateTime
    - endTime, DateTime
- Response: 
    - portalTickets: [portal ticket](#portal-ticket) list
- Includes

    |Includes| Description |
    | - | - |
    | contact | `get api/v3/portalTickets?include=contact` |  

### Submit a portal ticket
`post api/v3/portalTickets`
- Parameters: 
    - subject: string, subject, required
    - contactId: integer, id of the contact who submitted the portal ticket
    - customFields: [custom field value](#custom-field-value)[], custom field value array
    - message:  the first portal message
        - htmlBody: string, html body
        - plainBody: string, plain text
        - attachments: [attachment](#attachment)[], attachment array of message
- Response: 
  - portalTicket: [portal ticket](#portal-ticket) 

### Close portalTicket
`put api/v3/portalTickets/{id}/close` 
- Parameters: 
    - id, integer, ticket id,
- Response: 
    - portalTicket: [portal ticket](#portal-ticket) 

### Reopen portalTicket
`put api/v3/portalTickets/{id}/reopen` 
- Parameters: 
    - id, integer, ticket id,
- Response: 
    - portalTicket: [portal ticket](#portal-ticket) 

### List portal ticket messages
`get api/v3/portalTickets/{id}/messages`
- Parameters: 
    - id, integer
    - contactId, integer
- Response: 
    - messages: [portal ticket message](#portal-ticket-message) list
- Includes

    |Includes| Description |
    | - | - |
    | sender| `get api/v3/portalTickets/{id}/messages?include=sender` | 

### Reply portal ticket
 `post api/v3/portalTickets/{id}/messages`
- Parameters:
    - id: integer
    - contactId: integer
    - htmlBody: string, html body
    - plainBody: string, plain text
    - attachments: [attachment](#attachment)[], attachment array
- Response: 
    - message: [portal ticket message](#portal-ticket-message)

# Filters 
## objects 
### filter 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | filter id | 
| `name` | string | filter name | 
| `isPrivate` | boolean | if private filter| 
| `createdById` | integer | agent id | 
| `conditions` | [condition](#condition)[] | array of filter condition | 

### condition 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | condition id | 
| `fieldId` | integer | field id | 
| `matchType` | string | `contains`, `notContains`, `is`, `isNot`, `isMoreThan`, `isLessThan`, `before`, `after` | 
| `value` | string | condition value | 

## endpoints 
### List all public and private filters 
`get /api/v3/tickets/filters`
- Parameters 
    - no parameters 
- Response 
    - filters: [filter](#filter) list, without conditions

### Create a new filter 
`post api/v3/tickets/filters`
- Parameters 
    - name: string, filter name, required 
    - isPrivate: boolean, if private filter, default value: `false` 
    - conditions: [condition](#condition)[], array of filter condition
- Response 
    - filters: [filter](#filter) list 

### Get a filter and its conditions 
`get api/v3/tickets/filters/{id}` 
- Parameters 
    - id: integer, filter id 
- Response 
    - filter: [filter](#filter) 

### Update filter 
`put api/v3/tickets/filters/{id}` 
- Parameters 
    - id: integer, filter id 
    - name: string, filter name, required 
    - isPrivate: boolean, if private filter 
    - conditions: [condition](#condition)[], array of filter condition
- Response 
    - filter: [filter](#filter) 

### Delete filter 
`delete api/v3/tickets/filters/{id}` 
- Parameters 
    - id: integer, filter id 
- Response 
    - http status code 


# Fields 
## objects 
### field 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | field id | 
| `type` | string | `text`, `textarea`, `email`, `url`, `date`, `integer`, `float`, `operator`, <br/>`radio`, `checkbox`, `dropdownList`, `checkboxList`, `link`, `department` | 
| `name` | integer | field name | 
| `isSystemField` | boolean | if is system field | 
| `isRequired` | boolean | value if is required | 
| `defaultValue` | string | field default value | 
| `helpText` | string | field help text | 
| `length` | integer | field value max length | 
| `options` | [field option](#fieldoption)[] | value option | 

### fieldOption 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | option id | 
| `name` | string | option name | 
| `value` | string | field value | 
| `order` | integer | option order | 

## endpoints 
### List all fields and their options 
`get api/v3/tickets/fields` 
- Parameters
    - no parameters
- Response 
    - fields: [field](#field) list 

# Attachments 
## objects 
### attachment 
| Name | Type | Description | 
| - | - | - | 
| `guid` | string | attachment unique id | 
| `fileName` | string | attachment file name| 
| `url` | string | attachment download link | 
| `isAvailable` | boolean | if the attachment is available | 
## endpoints 
### Upload attachment 
`post /api/v3/tickets/attachments` 
- Content-type
    - multipart/form-data
- Parameters 
    - file: file
- Response 
    - attachment: [attachment](#attachment) 

# BlockedSenders 
## objects 
### blocked sender 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `email` | string | email or domain | 
| `blockType` | string | `blockEmailasJunk`, `rejectEmail`, `blockDomainasJunk`, `rejectDomain` | 

## endpoints 
### List blocked senders 
`get /api/v3/tickets/blockedSenders` 
- Parameters 
    - email: string, domain or email address 
- Response 
    - blockedSenders: [block sender](#blocked-sender) list 

### Add/update block sender 
`put api/v3/tickets/blockedSenders` 
- Parameters 
    - `email`, string, domain or email address 
    - `blockType`, string, `blockEmailasJunk`, `rejectEmail`, `blockDomainasJunk`, `rejectDomain`
- Response 
    - blockedSender: [block sender](#blocked-sender) 

### Remove block sender 
`delete api/v3/tickets/blockedSenders` 
- Parameters 
   - email: string, domain or email address 
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
`get api/v3/tickets/cannedResponses` 
- Parameters 
    - no parameters
- Response 
    - cannedResponses: [Canned responses](#canned-response) list 


# Configs 
### Get site configs about ticket 
`get api/v3/tickets/configs` 
- Parameters 
    - no parameters
- Response 
    - configs
        - isEnabledDepartment: boolean 
        - recipientLimitPerEmail: integer 

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
### Get one department 
`get api/v3/tickets/departments/{id} ` 
- Response 
    - department: [department](#department) 

### List all departments 
`get api/v3/tickets/departments` 
- Response 
    - departments: [department](#department) List without department member. 

# EmailAccounts 
## objects 
### email account 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `email` | string | email address |  
| `type` | string | pop3 or exchange | 
| `agentAssigneeId` | integer | agent id | 
| `departmentAssigneeId` | integer | department id | 
| `isDefault` | boolean | if default email account | 


## endpoints 
### List all enabled email accounts 
`get api/v3/tickets/emailAccounts` 
- Response 
    - emailAccounts: [email account](#email-account) list 

# JunkEmails 
## objects 
### junk email 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `subject` | string | email subject | 
| `time` | datetime | received time | 
| `name` | string | sender name | 
| `from` | string | email from email address | 
| `to` | string | to email addresses | 
| `cc` | string | cc email addresses | 
| `htmlBody` | string | html body | 
| `plainBody` | string | plain text body | 
| `emailAccountId` | integer | receive email account id | 
| `isRead` | boolean | if is read | 
| `attachments` | [attachment](#attachment)[] | attachments | 

## endpoints 
### List junk emails
`get api/v3/tickets/junkEmails` 

- Parameters 
    - keywords: string
    - pageIndex: integer
    - timeFrom: DateTime
    - timeTo: DateTime
- Response 
    - junkEmails: [junk email](#junk-email) list 
    - total: integer
    - previousPage: string, next page uri, the first page return null
    - nextPage: string, the last page return null
    - currentPage: string

### Get a junk email 
`get api/v3/tickets/junkEmails/{id}` 
- Parameters 
    - id: integer, email id 
- Response 
    - junkEmail: [junk email](#junk-email) 

### Update junk email 
`put api/v3/tickets/junkEmails/{id}` 
- Parameters 
    - isRead: boolean, 
- Response 
    - junkEmail: [junk email](#junk-email) 

### Restore a junk email to a normal ticket 
`post api/v3/tickets/junkEmails/{id}/notJunk` 
- Parameters 
    - id: integer, email id 
- Response 
    - ticket: [ticket](#ticket) 

### Delete a junk email 
`delete api/v3/tickets/junkEmails/{id}` 
- Parameters 
    - id: integer, junk email id 
- Response 
    - http status code 

# Tags 
## objects 
### tag 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `name` | string | tag name | 
| `ticketCount` | integer | the count of tickets with the tag | 
## endpoints 
### List all tags 
`Get api/v3/tickets/tags` 
- Response 
    - tags: [tag](#tag) list 

### Add a tag 
`Post api/v3/tickets/tags` 
- Parameters 
    - name: string, tag name 
- Response 
    - tag: [tag](#tag) 

### Update One Tag 
`Put api/v3/tickets/tags/{id}` 
- Parameters 
    - id: integer, tag id 
    - name: string, tag name 
- Response 
    - tag: [tag](#tag) 

### Delete a tag 
`Delete api/v3/tickets/tags/{id}` 
- Parameters 
    - id: integer, tag id 
- Response 
    - http status code