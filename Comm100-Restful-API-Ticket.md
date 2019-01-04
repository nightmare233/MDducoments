# Catalogue 
# Generic 
- Authorization: Comm100 Ticket API provides 2 authentication methods: 
    - API_key Authentication 
    - OAuth Authentication 
- [Reference document](https://www.comm100.com/doc/api/introduction.htm#/) 

# Paramter explanation 
- Incoming parameters： 
    - Get API passes parameters through the query string 
    - Put/Post API passes parameters through json data. 
    - DateTime Parametes： 
        - The input time parameter needs to conform to the standard format of is-8601, for reference：<a href="https://en.wikipedia.org/wiki/ISO_8601" target="_blank">wikipedia</a> 
- All response time values are UTC time, and the caller converts as their time zone as required. 

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
| `agentAssignee` | integer | agent assignee | 
| `departmentAssignee` | integer | department assignee | 
| `contactId` | integer | the contact id | 
| `receivingAccount` | string | `receiving email` | 
| `channel` | string | `portal`, `email`| 
| `priority` | string | priority: `urgent`, `high`, `normal`, `low` | 
| `status` | string | `new`, `pendingInternal`, <br/>`pendingExternal`, `onHold`, `closed` | 
| `isRead` | bool | if read of ticket | 
| `customFields` | [custom field value](#customfieldvalue)[] | custom field value array | 
| `createdBy` | integer | creator id of ticket: contact id or agent id | 
| `createdByType` |  string | creator type: agent or contact | 
| `createdTime` | datetime | create time of ticket | 
| `lastActivityTime` | datetime | last activity time of ticket | 
| `lastReplyTime` | datetime | last reply time of ticket | 
| `lastStatusChangeTime` | datetime | last status change time of ticket | 
| `lastReplyBy` | integer | last replier: contact id or agent id | 
| `isHaveDraft` | boolean | if has ticket draft | 
| `tags` | [tag](#tag)[] | tags of the ticket | 
| `closedTime` | datetime | closed time | 
| `closedByType` | string | closed by type: agent or contact | 
| `closedBy` | integer | id of agent or contact | 
| `isDeleted` | boolean | if deleted | 
| `slaPolicy` | integer | SLA id of this ticket matched | 
| `firstRespondBreachAt` | datetime | Timestamp that denotes when the first <br/> response is due | 
| `nextRespondBreachAt` | datetime | Timestamp that denotes when the next <br/> response is due | 
| `resolveBreachAt` | datetime | Timestamp that denotes when the ticket is <br/> due to be resolved | 
| `mentionedAgents`|[mentioned Agent](#mentionedagent)[]| mentioned agents list | 

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
| `isRead`| boolean | if read | 
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
| `time` | datetime | message received time | 
| `subject` | string | message subject | 
| `from` | string | message from email | 
| `to` | string | | 
| `cc` | string | message cc emails |  
| `attachments` | [attachment](#attachment)[] | attachment array| 

### ticketDraft 
| Name | Type | Description | 
| - | - | - | 
| `draftId` | integer | id of ticket draft | 
| `ticketId` | integer | id of ticket | 
| `subject` | string | draft subject | 
| `htmlBody` | string | html body of ticket draft | 
| `plainBody` | string | plain text body of ticket draft | 
| `from` | string | from email address | 
| `to` | string | to email address | 
| `cc` | string | cc email addresses | 
| `savedTime` | datetime | draft save time | 
| `savedBy` | integer | the agent id who saved the ticket draft | 
| `attachments` | [attachment](#attachment)[] | draft attachments | 

## endpoints 
### Get or search ticket list 
`get api/v2/ticket/tickets` 
+ Max 50 tickets are responded for each request. 
+ Parameters 
    - filterId: int, filter id, optional 
    - tagId: int, tag id, optional 
    - keywords: string, optional 
    - pageIndex: int, optional 
    - sortBy: string, optional, `Next SLA Breach\ Last Reply Time\	Last Activity Time\	Priority\ Status` 
    - order: string, optional, `ascending` or `descending` 
    - conditions: optional, parameter format: `conditions[0][field]=agent&conditions[0][matchType]=is&conditions[0][value]=hi&conditions[1][field]=agent&conditions[1][matchType]=is&conditions[1][value]=hello` 
+ Response 
    - [ticket object](#tickets) list, 
    - total: int, total number of tickets 
    - previousPage: string, next page uri, the first page return null. 
    - nextPage: string, the last page return null. 
    - currentPage: string, current page uri. 

### Get a ticket 
`get api/v2/ticket/tickets/{id} ` 
+ Parameters 
    - id: int, ticket  
+ Response 
    - [ticket object](#ticket) 

### Submit new ticket 
`post api/v2/ticket/tickets` 
- Parameters 

| Name | Type | Description | 
| - | - | - | 
| `subject` | string | ticket subject | 
| `message` | [new message object](#newmessage) | the first message of the ticket. | 
| `agentAssignee` | integer | agent id | 
| `departmentAssignee` | integer | department id | 
| `contactId` | integer | the contact id or agent id | 
| `createdBy` | integer | contact id or agent id | 
| `createdByType` | string | contact or agent | 
| `channel` | string | `portal`, `email` | 
| `priority` | string | priority: `urgent`, `high`, `normal`, `low` | 
| `status` | string | `new`, `pendingInternal`, `pendingExternal,`, `onHold`, `closed` |  
| `customFields` | [custom field value](#customfieldvalue)[] | custom field value array | 
| `tags` | [tag](#tag)[] | tags of the ticket | 

+ Response 
    - [ticket object](#tickets)

#### newMessage
| Name | Type | Description | 
| - | - | - | 
| `type` | string | `note`, `email`, `reply` | 
| `source` | string | `agentConsole`, `API` | 
| `subject` | string | message subject | 
| `htmlBody` | string | html body of message | 
| `plainBody` | string | plain text body of message | 
| `from` | string | message from email | 
| `to` | string | message to email when contact reply,<br/> to and from is nullable and don't send email | 
| `cc` | string | message cc emails | 
| `attachments` | [attachment](#attachment)[] | attachment array | 

### Get ticket messages 
`get api/v2/ticket/tickets/{id}/messages` 
+ Parameters 
    - id: int, ticket id 
+ Response 
    - [message](#message) list 


### Reply ticket 
`post api/v2/ticket/tickets/{id}/messages` 
+ Parameters 
    - [new message object](#newmessage)  
+ Response 
    - [message](#message) list 

### Update ticket 
`put api/v2/ticket/tickets/{id}` 
+ Parameters 

| Name | Type | Description | 
| - | - | - | 
| `subject` | string | ticket subject | 
| `agentAssignee` | integer | agent assignee | 
| `departmentAssignee` | integer | department assignee | 
| `contactId` | integer | the contact id | 
| `priority` | string | priority: `urgent`, `high`, `normal`, `low` | 
| `status` | string |  status of ticket: `new`, `pendingInternal`, `pendingExternal`, `onHold`, `closed` | 
| `isRead` | bool | if read of ticket | 
| `customFields` | [custom field value](#customfieldvalue)[] | custom field value array | 
| `tags` | [tag](#tag)[] | tags of the ticket | 
+ Response 
    - [ticket](#ticket) object 

### Batch update ticket 
`put api/v2/ticket/tickets/` 
+ Parameters 
    - ids: ticket id array, 
    - status, string, optional 
    - priority, string, optional 
    - agentassigneeId, integer, optional 
    - departmentAssigneeId, integer, optional 
    - isRead, boolean, optional 
+ Response 
    - [ticket](#ticket) object list 

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
`delete api/v2/ticket/tickets/{id} ` 
- Parameters 
    - id: int 
- Response 
    - http status code 

### Batch delete tickets 
`delete api/v2/ticket/tickets/ ` 
+ Parameters 
    - ticket id array 
+ Response 
    - http status code 

### Get deleted tickets 
`get api/v2/ticket/deletedTickets/` 
- Parameters 
    - keywords: string, optional 
    - pageIndex: int, optional 
    - timeFrom: DateTime, optional, default search the last 30 days. 
    - timeTo: DateTime, optional, defautl value is the current time. 
- Response 
    - [ticket object](#ticket) list 
    - total: int, total number of tickets 
    - previousPage: string, next page uri, the first page return null. 
    - nextPage: string, the last page return null. 
    - currentPage: string, current page uri. 

### Get a deleted ticket 
`get api/v2/ticket/deletedTickets/{id}` 
- Parameters 
    - id: int, ticket id 
- Response 
    - [ticket object](#ticket) 

### Restore a deleted ticket 
`post api/v2/ticket/deletedTickets/{id}/restore ` 
- Parameters 
    - id: int, ticket id 
- Response 
    - [ticket object](#ticket)  

### Delete a ticket permanently 
`delete api/v2/ticket/deletedTickets/{id}` 
- Parameters 
    - id: int, ticket id 
- Response 
    - http status code 

### Get ticket draft 
`get api/v2/ticket/tickets/{id}/draft` 
- Parameters 
    - id: int, ticket id 
- Response 
    - [ticket draft object](#ticketdraft) 

### Create ticket draft 
`post api/v2/ticket/tickets/{id}/draft` 
- Parameters 
    - [ticket draft object](#ticketdraft) 
- Response 
    - [ticket draft object](#ticketdraft) 

### Update ticket draft 
`put api/v2/ticket/tickets/{id}/draft` 
- Parameters 
    - [ticket draft object](#ticketdraft) 
- Response 
    - [ticket draft object](#ticketdraft) 

### Delete ticket draft 
`delete api/v2/ticket/tickets/{id}/draft` 
- Parameters 
    - id: int, ticket id 
- Response 
    - http status code 

### Merge ticket 
`post api/v2/ticket/tickets/{id}/merge`
- Parameters 
    - id: int, target ticket id, 
    - sourceId: int, source ticket id 
- Response 
    - [ticket object](#ticket) 

### Get unread tickets number in filters 
`get api/v2/ticket/filters/unreadCount?filterIds={filterid1}&filterIds={filterid2}&filterIds={filterid3}`
- Parameters 
    - filterIds: filter id array 
- Response 
    - allCount: integer, all unread ticket number. 
    - array including: 
        - filterId: int, filter id 
        - unreadCount: int, count unread tickets of a filter 
        - unreadMentionedCount: int, unread and metioned to me tickets number in filter 

# PortalTicket
## objects
### portalTicket
| Name | Type | Description |
| - | - | - |
| `id` | integer | id of ticket |
| `subject` | string | ticket subject |
| `contactId` | integer | id of the contact who submit the ticket |
| `isClosed` | boolean | if the ticket is closed |
| `customFields` | [custom field value](#customfieldvalue)[] | custom field value array |
| `createdTime` | datetime | create time of ticket |
| `closedTime` | datetime | close time of ticket |


## endpoints
### Get a ticket by ticket id
`get api/v2/ticket/portalTickets/{id}`
- Parameters: 
    - id, integer, ticket id
    - contactId, integer
- Response: 
    - [portal ticket object](#portalticket) 

### Get ticket list
`get api/v2/ticket/portalTickets`
- Parameters:
    - contactId, integer
    - startTime, DateTime, optional
    - endTime, DateTime, optional
- Response: 
    - [portal ticket object ](#portalticket)list

### Submit new ticket
`post api/v2/ticket/portalTickets`
- Parameters: 

| Name | Type | Description |
| - | - | - | 
| `subject` | string | ticket subject |
| `message` | [new message object](#portalticketnewmessage) | the first message |
| `contactId` | integer | id of the contact who submit the ticket | 
| `customFields` | [custom field value](#customfieldvalue)[] | custom field value array |

- Response: 
  - [portal ticket object](#portalticket) 

##### portalTicketNewMessage
| Name | Type | Description |
| - | - | - |
| `htmlBody` | string | html body of the message |   
| `plainBody` | string | plain text of the message |  
| `attachments` | [attachment](#attachment)[] | attachment array of message | 

### Close ticket
`put api/v2/ticket/portalTickets/{id}/close` 
- Parameters: 
    - id, integer, ticket id,
    - contactId, integer
- Response: 
    - [portal ticket object](#portalticket) 

### Reopen ticket
`put api/v2/ticket/portalTickets/{id}/reopen` 
- Parameters: 
    - id, integer, ticket id,
    - contactId, integer
- Response: 
    - [portal ticket object](#portalticket) 

### Get message list of ticket
`get api/v2/ticket/portalTickets/{id}/messages`
- Parameters: 
    - id, integer, ticket id
    - contactId, integer
- Response: 
    - [message object](#message) list

### Reply ticket
 `post api/v2/ticket/portalTickets/{id}/messages`
- Parameters:
    - id: integer, ticket id
    - contactId: integer, contact id
    - [new message object](#portalticketnewmessage)
- Response: 
    - [message object](#message) list

# Filters 
## objects 
### filter 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | filter id | 
| `name` | string | filter name | 
| `isPrivate` | boolean | if private filter| 
| `createdBy` | integer | agent id | 
| `conditions` | [condition](#condition)[] | array of filter condition | 

### condition 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | condition id | 
| `fieldId` | integer | field id | 
| `matchType` | string | matach type: `contains`, `notContains`, <br>`is`, `isNot`, `isMoreThan`, `isLessThan`, `before`, `after` | 
| `value` | string | condition value | 

## endpoints 
### Get all public and private filters 
`get /api/v2/ticket/filters`
- Parameters 
    - no parameters 
- Response 
    - [filter object](#filter) list, without conditions. 

### Create a new filter 
`post api/v2/ticket/filters`
- Parameters 
    - [filter object](#filter) 
- Response 
    - [filter object](#filter) list 

### Get a filter and its conditions 
`get api/v2/ticket/filters/{id}` 
- Parameters 
    - id: int, filter id 
- Response 
    - [filter object](#filter) 

### Update filter 
`put api/v2/ticket/filters/{id}` 
- Parameters 
    - [filter object](#filter) 
- Response 
    - [filter object](#filter) 

### Delete filter 
`delete api/vi/ticket/filters/{id}` 
- Parameters 
    - id: int, filter id 
- Response 
    - http status code 


# Fields 
## objects 
### field 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | field id | 
| `dataType` | string | `text`, `textarea`, `email`, `url`, `date`, `integer`, `float`,<br/> `operator`, `radio`, `checkbox`, `dropdownList`, `checkboxList`, `link`, `department` | 
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
    - [field object](#field) list 

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
- Parameters 
    - multipart/form-data 
- Response 
    - [attachment object](#attachment) 

# BlockedSenders 
## objects 
### blockedSender 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `email` | string | email or domain | 
| `blockType` | string | `blockEmailasJunk`,<br> `rejectEmail`, `blockDomainasJunk`, `rejectDomain` | 

## endpoints 
### Get block sender list 
`get /api/v2/ticket/blockedSenders?domain={domain}` 
- Parameters 
    - domain: string, the domain of email address 
- Response 
    - [block sender object](#blockedsender) list 

### Add block sender 
`post api/v2/ticket/blockedSenders` 
- Parameters 
    - [block sender object](#blockedsender) 
- Response 
    - [block sender object](#blockedsender) 

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
    - [Canned responses object](#cannedresponse) list 


# Configs 
### Get ticket configs of this site 
`get api/v2/ticket/configs` 
- Response 
    - isEnabledDepartment: bool 
    - recipientLimitPerEmail: int 

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
    - [department object](#department) 

### Get all departments 
`get api/v2/ticket/departments` 
- Response 
    - [department object](#department) List without department member. 

# EmailAccounts 
## objects 
### emailAccount 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `email` | string | email address |  
| `type` | string | pop3 or exchange | 
| `agentAssigneeId` | integer | agent assignee id | 
| `departmentAssigneeId` | integer | agent assignee id | 
| `isDefault` | boolean | if default email account | 


## endpoints 
### Get all enabled email accounts 
`get api/v2/ticket/emailAccounts` 
- Response 
    - [email account object](#emailaccount) list 

# JunkEmails 
## objects 
### junkEmail 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id | 
| `subject` | string | email subject | 
| `time` | datetime | id | 
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
    - keywords: string, optional 
    - pageIndex: int, optional 
    - timeFrom: DateTime, optional 
    - timeTo: DateTime, optional 
- Response 
    - [junk email object](#junkemail) list 
    - total: int, 
    - previousPage: string, next page uri, the first page return null. 
    - nextPage: string, the last page return null. 
    - currentPage: string, 

### Get a junk email 
`get api/v2/ticket/junkEmails/{id}` 
- Parameters 
    - id: int, email id 
- Response 
    - [junk email object](#junkemail) 

### Update junk email 
`put api/v2/ticket/junkEmails/{id}` 
- Parameters 
    - isRead: bool, 
- Response 
    - [junk email object](#junkemail) 

### Restore a junk email to a normal ticket 
`post api/v2/ticket/junkEmails/{id}/notJunk` 
- Parameters 
    - id: int, email id 
- Response 
    - [ticket object](#ticket) 

### Delete a junk email 
`delete api/v2/ticket/junkEmails/{id}` 
- Parameters 
    - id: int, junk email id 
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
    - [tag object](#tag) list 

### Add a tag 
`Post api/v2/ticket/tags` 
- Parameters 
    - name: string, tag name 
- Response 
    - [tag object](#tag) 

### Update One Tag 
`Put api/v2/ticket/tags/{id}` 
- Parameters 
    - id: int, tag id 
    - name: string, tag name 
- Response 
    - [tag object](#tag) 

### Delete a tag 
`Delete api/v2/ticket/tags/{id}` 
- Parameters 
    - id: int, tag id 
- Response 
    - http status code


    