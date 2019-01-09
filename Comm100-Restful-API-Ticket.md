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
    | `get api/v2/ticket/tickets` | agentAssignee, departmentAssignee, contact, createdBy, lastRepliedBy, messages |
    | `get api/v2/ticket/tickets/{id}` | agentAssignee, departmentAssignee, contact, createdBy, lastRepliedBy, messages |
    | `get api/v2/ticket/tickets/{id}/messages` | sender |
    | `get api/v2/ticket/deletedTickets` | agentAssignee, departmentAssignee, contact, createdBy, lastRepliedBy, messages |
    | `get api/v2/ticket/deletedTickets/{id}` | agentAssignee, departmentAssignee, contact, createdBy, lastRepliedBy, messages |
    | `get api/v2/ticket/deletedTickets/{id}/messages` | sender |
    | `get api/v2/ticket/portalTickets/{id}` | contact, portalMessages |
    | `get api/v2/ticket/portalTickets` | contact, portalMessages |
    | `get api/v2/ticket/portalTickets/{id}/portalMessages` | sender |
    | `get /api/v2/ticket/filters` | createdBy |

- Sample:
    - request: `get api/v2/ticket/tickets/{id}?include=agentAssignee&contact `
    - response:

        ``` javascript
        {
            "ticket": {
                "id": 1
                //...
            },
            "agentAssignee": {
                "id": 12,
                //...
            },
            "contact": {
                "id": 23,
                //...
        }
        ```

# Resource List 
|Name|EndPoint|Note| 
|---|---|---| 
|[Ticket](#ticket)|/api/v2/ticket/tickets|| 
|[PortalTicket](#portalTicket)|/api/v2/ticket/portalTickets|for contact|
|[Filter](#filter)|/api/v2/ticket/filters|| 
|[Field](#field)|/api/v2/ticket/fields|| 
|[Attachment](#attachment)|/api/v2/ticket/attachments|| 
|[BlockedSender](#blockedsender)|/api/v2/ticket/blockedSenders|blocked email or domain| 
|[CannedResponse](#cannedresponse)|/api/v2/ticket/cannedResponses||
|[Config](#config)|/api/v2/ticket/configs|site settings| 
|[Department](#department)|/api/v2/ticket/departments|| 
|[EmailAccount](#emailaccount)|/api/v2/ticket/emailAccounts|| 
|[JunkEmail](#junkemail)|/api/v2/ticket/junkEmails|emails from <br/>blocked senders| 
|[Tag](#tag)|/api/v2/ticket/tags|| 

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
| `customFields` | [custom field value](#customfieldvalue)[] | custom field value array | 
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
| `mentionedAgents`|[mentionedAgent](#mentionedagent)[]| mentioned agents list | 

### customFieldValue
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | the id of custom field |
| `name` | string | the name of custom field |
| `value` | string | the value of custom field |

### mentionedAgent 
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

### ticketDraft 
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
| `savedById` | integer | the agent id who saved the ticket draft | 
| `attachments` | [attachment](#attachment)[] | draft attachments | 

## endpoints 
### Get or search ticket list 
`get api/v2/ticket/tickets` 
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
    - tickets: [ticket object](#tickets) list, 
    - total: integer, total number of tickets 
    - previousPage: string, next page uri, the first page return null. 
    - nextPage: string, the last page return null. 
    - currentPage: string, current page uri. 
+ Includes

    | Includes | Description |
    | - | - |
    | agentAssignee | `get api/v2/ticket/tickets?include=agentAssignee` |
    | departmentAssignee | `get api/v2/ticket/tickets?include=departmentAssignee` |
    | contact | `get api/v2/ticket/tickets?include=contact` |
    | createdBy | `get api/v2/ticket/tickets?include=createdBy` |
    | lastRepliedBy | `get api/v2/ticket/tickets?include=lastRepliedBy` |
    | messages | `get api/v2/ticket/tickets?include=messages` |

### Get a ticket 
`get api/v2/ticket/tickets/{id} ` 
+ Parameters 
    - id: integer, ticket  
+ Response 
    - ticket: [ticket object](#ticket) 
+ Includes

    | Includes | Description |
    | - | - |
    | agentAssignee | `get api/v2/ticket/tickets/{id}?include=agentAssignee` |
    | departmentAssignee | `get api/v2/ticket/tickets/{id}?include=departmentAssignee` |
    | contact | `get api/v2/ticket/tickets/{id}?include=contact` |
    | createdBy | `get api/v2/ticket/tickets/{id}?include=createdBy` |
    | lastRepliedBy | `get api/v2/ticket/tickets/{id}?include=lastRepliedBy` |
    | messages | `get api/v2/ticket/tickets/{id}?include=messages` |
 
### Submit new ticket 
`post api/v2/ticket/tickets` 
- Parameters 
    - subject: string, ticket subject, required
    - channel: string, `portal`, `email`, required 
    - contactId: integer, contact id
    - agentAssigneeId: integer, agent id
    - departmentAssigneeId: integer, department id
    - priority: string, `urgent`, `high`, `normal`, `low`, default value: `normal` 
    - status: string, `new`, `pendingInternal`, `pendingExternal,`, `onHold`, `closed`, default value: `new`  
    - customFields: [custom field value](#customfieldvalue)[], custom field value array
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
    - ticket: [ticket object](#tickets)

### Get ticket messages 
`get api/v2/ticket/tickets/{id}/messages` 
+ Parameters 
    - id: integer, ticket id 
+ Response 
    - messages: [message](#message) list 
+ Includes

    | Includes | Description |
    | - | - |
    | sender | `get api/v2/ticket/tickets/{id}/messages?include=sender` |

### Update ticket 
`put api/v2/ticket/tickets/{id}` 
- Parameters 
    - id: integer, ticket id
    - subject: string, ticket subject
    - contactId: integer, the contact id or agent id
    - agentAssigneeId: integer, agent id
    - departmentAssigneeId: integer, department id
    - priority: string, priority: `urgent`, `high`, `normal`, `low`
    - status: string, `new`, `pendingInternal`, `pendingExternal,`, `onHold`, `closed`
    - isRead: boolean
    - customFields: [custom field value](#customfieldvalue)[], custom field value array
    - tagIds: integer[], tag id array
- Response 
    - ticket: [ticket](#ticket) object 

### Batch update ticket 
`put api/v2/ticket/tickets/` 
+ Parameters 
    - ids: integer[], ticket id array
    - status, string
    - priority, string
    - agentassigneeId, integer
    - departmentAssigneeId, integer
    - isRead, boolean
+ Response 
    - tickets: [ticket](#ticket) object list 

### Reply ticket 
`post api/v2/ticket/tickets/{id}/messages` 
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

### Mark ticket read 
`put api/v2/ticket/tickets/{id}/read` 
+ Parameters 
    - id: ticketId 
+ Response 
    - http status code 

### Mark ticket unread 
`put api/v2/ticket/tickets/{id}/unread` 
+ Parameters 
    - id: ticketId 
+ Response 
    - http status code 

### Delete a ticket 
`delete api/v2/ticket/tickets/{id}` 
- Parameters 
    - id: integer 
- Response 
    - http status code 

### Batch delete tickets 
`delete api/v2/ticket/tickets` 
+ Parameters 
    - ids: integer[], id array
+ Response 
    - http status code 

### Get deleted tickets 
`get api/v2/ticket/deletedTickets/` 
- Parameters 
    - keywords: string
    - pageIndex: integer
    - timeFrom: DateTime, default search the last 30 days
    - timeTo: DateTime, defautl value is the current time
- Response 
    - deletedTickets: [ticket object](#ticket) list 
    - total: integer, total number of tickets 
    - previousPage: string, next page uri, the first page return null. 
    - nextPage: string, the last page return null. 
    - currentPage: string, current page uri. 
- Includes

    | Includes | Description |
    | - | - |
    | agentAssignee | `get api/v2/ticket/deletedTickets?include=agentAssignee` |
    | departmentAssignee | `get api/v2/ticket/deletedTickets?include=departmentAssignee` |
    | contact | `get api/v2/ticket/deletedTickets?include=contact` |
    | createdBy | `get api/v2/ticket/deletedTickets?include=createdBy` |
    | lastRepliedBy | `get api/v2/ticket/deletedTickets?include=lastRepliedBy` |
    | messages | `get api/v2/ticket/deletedTickets?include=messages` |

### Get a deleted ticket 
`get api/v2/ticket/deletedTickets/{id}` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - deletedTicket: [ticket object](#ticket) 
- Includes

    | Includes | Description |
    | - | - |
    | agentAssignee | `get api/v2/ticket/deletedTickets/{id}?include=agentAssignee` |
    | departmentAssignee | `get api/v2/ticket/deletedTickets/{id}?include=departmentAssignee` |
    | contact | `get api/v2/ticket/deletedTickets/{id}?include=contact` |
    | createdBy | `get api/v2/ticket/deletedTickets/{id}?include=createdBy` |
    | lastRepliedBy | `get api/v2/ticket/deletedTickets/{id}?include=lastRepliedBy` |
    | messages | `get api/v2/ticket/deletedTickets/{id}?include=messages` |

### Get messages of a deleted ticket 
`get api/v2/ticket/deletedTickets/{id}/messages` 
- Parameters 
    - id: integer 
- Response 
    - messages: [message object](#message) 
- Includes

    | Includes | Description |
    | - | - |
    | sender | `get api/v2/ticket/deletedTickets/{id}/messages?include=sender` |


### Restore a deleted ticket 
`post api/v2/ticket/deletedTickets/{id}/restore ` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - deletedTicket: [ticket object](#ticket)  

### Delete a ticket permanently 
`delete api/v2/ticket/deletedTickets/{id}` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - http status code 

### Get ticket draft 
`get api/v2/ticket/tickets/{id}/draft` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - ticketDraft: [ticket draft object](#ticketdraft) 

### Create ticket draft 
`post api/v2/ticket/tickets/{id}/draft` 
- Parameters 
    - [ticket draft object](#ticketdraft) 
- Response 
    - ticketDraft: [ticket draft object](#ticketdraft) 

### Update ticket draft 
`put api/v2/ticket/tickets/{id}/draft` 
- Parameters 
    - [ticket draft object](#ticketdraft) 
- Response 
    - ticketDraft: [ticket draft object](#ticketdraft) 

### Delete ticket draft 
`delete api/v2/ticket/tickets/{id}/draft` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - http status code 

### Merge ticket 
`post api/v2/ticket/tickets/{id}/merge`
- Parameters 
    - id: integer, target ticket id, 
    - sourceId: integer, source ticket id 
- Response 
    - ticket: [ticket object](#ticket) 

### Get unread tickets number in filters 
`get api/v2/ticket/filters/unreadCount?filterIds={filterid1}&filterIds={filterid2}&filterIds={filterid3}`
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
### portalTicket
| Name | Type | Description |
| - | - | - |
| `id` | integer | id of ticket |
| `subject` | string | subject |
| `contactId` | integer | id of the contact who submitted the portal ticket |
| `isClosed` | boolean | if the portal ticket is closed |
| `customFields` | [custom field value](#customfieldvalue)[] | custom field value array |
| `createdTime` | datetime | create time |
| `closedTime` | datetime | close time |

### portalMessage 
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
### Get a portalTicket by id
`get api/v2/ticket/portalTickets/{id}`
- Parameters
    - id, integer, id
    - contactId, integer
- Response
    - portalTicket: [portalTicket object](#portalticket) 
- Includes

    |Includes| Description |
    | - | - |
    | contact | `get api/v2/ticket/portalTickets/{id}?include=contact` | 
    | portalMessages | `get api/v2/ticket/portalTickets/{id}?include=portalMessages` |
 
### Get portalTicket list
`get api/v2/ticket/portalTickets`
- Parameters:
    - contactId, integer, required
    - startTime, DateTime
    - endTime, DateTime
- Response: 
    - portalTickets: [portalTicket object ](#portalticket) list
- Includes

    |Includes| Description |
    | - | - |
    | contact | `get api/v2/ticket/portalTickets?include=contact` | 
    | portalMessages | `get api/v2/ticket/portalTickets?include=portalMessages` |

### Submit a portalTicket
`post api/v2/ticket/portalTickets`
- Parameters: 
    - subject: string, subject, required
    - contactId: integer, id of the contact who submitted the portal ticket
    - customFields: [custom field value](#customfieldvalue)[], custom field value array
    - portalMessage:  the first portal message
        - htmlBody: string, html body
        - plainBody: string, plain text
        - attachments: [attachment](#attachment)[], attachment array of message
- Response: 
  - portalTicket: [portalTicket object](#portalticket) 

### Close portalTicket
`put api/v2/ticket/portalTickets/{id}/close` 
- Parameters: 
    - id, integer, ticket id,
- Response: 
    - portalTicket: [portalTicket object](#portalticket) 

### Reopen portalTicket
`put api/v2/ticket/portalTickets/{id}/reopen` 
- Parameters: 
    - id, integer, ticket id,
- Response: 
    - portalTicket: [portalTicket object](#portalticket) 

### Get portalMessages of a portalTicket
`get api/v2/ticket/portalTickets/{id}/portalMessages`
- Parameters: 
    - id, integer
    - contactId, integer
- Response: 
    - portalMessages: [portalMessage object](#portalMessage) list
- Includes

    |Includes| Description |
    | - | - |
    | sender| `get api/v2/ticket/portalTickets/{id}/portalMessages?include=sender` | 

### Reply portalTicket
 `post api/v2/ticket/portalTickets/{id}/portalMessages`
- Parameters:
    - id: integer
    - contactId: integer
    - htmlBody: string, html body
    - plainBody: string, plain text
    - attachments: [attachment](#attachment)[], attachment array
- Response: 
    - portalMessage: [portalMessage object](#portalMessage) list

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
| `matchType` | string | `contains`, `notContains`, <br>`is`, `isNot`, `isMoreThan`, `isLessThan`, `before`, `after` | 
| `value` | string | condition value | 

## endpoints 
### Get all public and private filters 
`get /api/v2/ticket/filters`
- Parameters 
    - no parameters 
- Response 
    - filters: [filter object](#filter) list, without conditions
- Includes

    |Includes| Description |
    | - | - |
    | createdBy | `get /api/v2/ticket/filters?include=createdBy` | 

### Create a new filter 
`post api/v2/ticket/filters`
- Parameters 
    - name: string, filter name, required 
    - isPrivate: boolean, if private filter, default value: `false` 
    - conditions: [condition](#condition)[], array of filter condition
- Response 
    - filters: [filter object](#filter) list 

### Get a filter and its conditions 
`get api/v2/ticket/filters/{id}` 
- Parameters 
    - id: integer, filter id 
- Response 
    - filter: [filter object](#filter) 

### Update filter 
`put api/v2/ticket/filters/{id}` 
- Parameters 
    - id: integer, filter id 
    - name: string, filter name, required 
    - isPrivate: boolean, if private filter 
    - conditions: [condition](#condition)[], array of filter condition
- Response 
    - filter: [filter object](#filter) 

### Delete filter 
`delete api/vi/ticket/filters/{id}` 
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
### Get fields and their options 
`get api/v2/ticket/fields` 
- Response 
    - fields: [field object](#field) list 

# Attachments 
## objects 
### attachment 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | attachment id | 
| `fileName` | string | attachment file name| 
| `guid` | string | attachment unique id | 
| `url` | string | attachment download link | 
| `isAvailable` | boolean | if the attachment is available | 
## endpoints 
### Upload attachment 
`post /api/v2/ticket/attachments` 
- Content-type
    - multipart/form-data
- Parameters 
    - file: file
- Response 
    - attachment: [attachment object](#attachment) 

# BlockedSenders 
## objects 
### blockedSender 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `email` | string | email or domain | 
| `blockType` | string | `blockEmailasJunk`, `rejectEmail`, `blockDomainasJunk`, `rejectDomain` | 

## endpoints 
### Get block sender list 
`get /api/v2/ticket/blockedSenders?domain={domain}` 
- Parameters 
    - domain: string, the domain of email address 
- Response 
    - blockedSenders: [block sender object](#blockedsender) list 

### Add block sender 
`post api/v2/ticket/blockedSenders` 
- Parameters 
    - [block sender object](#blockedsender) 
- Response 
    - blockedSender: [block sender object](#blockedsender) 

# CannedResponses 
## objects 
### cannedResponse 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `name` | string | canned response name | 
| `htmlContent` | string | html format content | 
| `textContent` | string | text format content | 


## endpoints 
### Get canned responses 
`get api/v2/ticket/cannedResponses` 
- Response 
    - cannedResponses: [Canned responses object](#cannedresponse) list 


# Configs 
### Get ticket configs of this site 
`get api/v2/ticket/configs` 
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
| `members` | [department member](#departmentmember)[] | department member array | 

### departmentMember 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `name` | string | member name | 
| `type` | string | agent or group | 

## endpoints 
### Get one department 
`get api/v2/ticket/departments/{id} ` 
- Response 
    - department: [department object](#department) 

### Get all departments 
`get api/v2/ticket/departments` 
- Response 
    - departments: [department object](#department) List without department member. 

# EmailAccounts 
## objects 
### emailAccount 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `email` | string | email address |  
| `type` | string | pop3 or exchange | 
| `agentAssigneeId` | integer | agent id | 
| `departmentAssigneeId` | integer | depatment id | 
| `isDefault` | boolean | if default email account | 


## endpoints 
### Get all enabled email accounts 
`get api/v2/ticket/emailAccounts` 
- Response 
    - emailAccounts: [email account object](#emailaccount) list 

# JunkEmails 
## objects 
### junkEmail 
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
### Get junk email list 
`get api/v2/ticket/junkEmails` 

- Parameters 
    - keywords: string
    - pageIndex: integer
    - timeFrom: DateTime
    - timeTo: DateTime
- Response 
    - junkEmails: [junk email object](#junkemail) list 
    - total: integer
    - previousPage: string, next page uri, the first page return null
    - nextPage: string, the last page return null
    - currentPage: string

### Get a junk email 
`get api/v2/ticket/junkEmails/{id}` 
- Parameters 
    - id: integer, email id 
- Response 
    - junkEmail: [junk email object](#junkemail) 

### Update junk email 
`put api/v2/ticket/junkEmails/{id}` 
- Parameters 
    - isRead: boolean, 
- Response 
    - junkEmail: [junk email object](#junkemail) 

### Restore a junk email to a normal ticket 
`post api/v2/ticket/junkEmails/{id}/notJunk` 
- Parameters 
    - id: integer, email id 
- Response 
    - ticket: [ticket object](#ticket) 

### Delete a junk email 
`delete api/v2/ticket/junkEmails/{id}` 
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
| `ticketCount` | integer | tickets number with this tag | 
## endpoints 
### Get all tags 
`Get api/v2/ticket/tags` 
- Response 
    - tags: [tag object](#tag) list 

### Add a tag 
`Post api/v2/ticket/tags` 
- Parameters 
    - name: string, tag name 
- Response 
    - tag: [tag object](#tag) 

### Update One Tag 
`Put api/v2/ticket/tags/{id}` 
- Parameters 
    - id: integer, tag id 
    - name: string, tag name 
- Response 
    - tag: [tag object](#tag) 

### Delete a tag 
`Delete api/v2/ticket/tags/{id}` 
- Parameters 
    - id: integer, tag id 
- Response 
    - http status code