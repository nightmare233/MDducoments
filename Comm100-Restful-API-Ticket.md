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
|[Ticket](#ticket)|/api/v1/ticket/tickets|| 
|[PortalTicket](#portalTicket)|/api/v1/ticket/portalTickets|for contact|
|[Filter](#filter)|/api/v1/ticket/filters|| 
|[Field](#field)|/api/v1/ticket/fields|| 
|[Attachment](#attachment)|/api/v1/ticket/attachments|| 
|[BlockedSender](#blockedsender)|/api/v1/ticket/blockedSenders|blocked email or domain| 
|[CannedResponse](#cannedresponse)|/api/v1/ticket/cannedResponses||
|[Config](#config)|/api/v1/ticket/configs|site settings| 
|[Department](#department)|/api/v1/ticket/departments|| 
|[EmailAccount](#emailaccount)|/api/v1/ticket/emailAccounts|| 
|[JunkEmail](#junkemail)|/api/v1/ticket/junkEmails|emails from <br/>blocked senders| 
|[Tag](#tag)|/api/v1/ticket/tags|| 

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
| `status` | string |  status of ticket: `new`, `pendingInternal`, <br/>`pendingExternal`, `onHold`, `closed` | 
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
| `ifRead`| boolean | if read | 
| `messageId`| integer| message id| 

### message 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id of message | 
| `type` | string | `Note`, `Email`, `Reply` | 
| `source` | string | `Agentconsole`, `HelpDesk`, `API`, `Chat`, `Offline Message`| 
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

### new_message
| Name | Type | Description | 
| - | - | - | 
| `type` | string | `Note`, `Email`, `Reply` | 
| `source` | string | `Agentconsole`, `HelpDesk`, `API`, `Email`, `Chat`, `Offline Message` | 
| `subject` | string | message subject | 
| `htmlBody` | string | html body of message | 
| `plainBody` | string | plain text body of message | 
| `from` | string | message from email | 
| `to` | string | message to email when contact reply,<br/> to and from is nullable and don't send email | 
| `cc` | string | message cc emails | 
| `attachments` | [attachment](#attachment)[] | attachment array | 

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
| `attachments` | [attachment](#attachment)[] | draft attachments | 

## endpoints 
### Get or search ticket list 
<code>GET api/v1/ticket/tickets</code> 
+ Max 50 tickets are responded for each request. 
+ Parameters 
    - filterId: int, filter id, optional 
    - tagId: int, tag id, optional 
    - keywords: string, optional 
    - pageIndex: int, optional 
    - sortBy: string, optional, `Next SLA Breach\ Last Reply Time\	Last Activity Time\	Priority\ Status` 
    - order: string, optional, `ascending` or `descending` 
    - conditions: optional, parameter format: `?conditions[0][field]=agent&conditions[0][matchType]=is&conditions[0][value]=hi&conditions[1][field]=agent&conditions[1][matchType]=is&conditions[1][value]=hello` 
+ Response 
    - [ticket object](#tickets) list, 
    - total: int, total number of tickets 
    - previousPage: string, next page uri, the first page return null. 
    - nextPage: string, the last page return null. 
    - currentPage: string, current page uri. 

### Get a ticket 
<code>GET api/v1/ticket/tickets/{id} </code> 
+ Parameters 
    - id: int, ticket  
+ Response 
    - [ticket object](#ticket) 

### Submit new ticket 
<code>POST api/v1/ticket/tickets</code> 
- Parameters 

| Name | Type | Description | 
| - | - | - | 
| `subject` | string | ticket subject | 
| `message` | [new message object](#new_message) | the first message of the ticket. | 
| `agentAssignee` | integer | agent id | 
| `departmentAssignee` | integer | department id | 
| `contactId` | integer | the contact id or agent id | 
| `createdBy` | integer | contact id or agent id | 
| `createdByType` | string | contact or agent | 
| `channel` | string | `portal`, `email` | 
| `priority` | string | priority: `urgent`, `high`, `normal`, `low` | 
| `status` | string |  status of ticket: `new`, `pendingInternal`, `pendingExternal,`, `onHold`, `closed` |  
| `customFields` | [custom field value](#customfieldvalue)[] | custom field value array | 
| `tags` | [tag](#tag)[] | tags of the ticket | 

+ Response 
    - [ticket object](#tickets)

### Get ticket messages 
<code>GET api/v1/ticket/tickets/{id}/messages</code> 
+ Parameters 
    - id: int, ticket id 
+ Response 
    - [message](#message) list 


### Reply ticket 
<code>POST api/v1/ticket/tickets/{id}/messages</code> 
+ Parameters 
    - [new message object](#new_message)  
+ Response 
    - [message](#message) list 

### Update ticket 
<code>PUT api/v1/ticket/tickets/{id}</code> 
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
<code>PUT api/v1/ticket/tickets/</code> 
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
<code>PUT api/v1/ticket/tickets/{id}/read</code> 
+ Parameters 
    - id: ticketId 
+ Response 
    - http status code 

### Mark ticket unread 
<code>PUT api/v1/ticket/tickets/{id}/unread</code> 
+ Parameters 
    - id: ticketId 
+ Response 
    - http status code 

### Delete a ticket 
<code>Delete api/v1/ticket/tickets/{id} </code> 
- delete ticket, display ticket in recycle bin. 
- Parameters 
    - id: int 
- Response 
    - http status code 

### Batch delete tickets 
<code>Delete api/v1/ticket/tickets/ </code> 
+ Parameters 
    - ticket id array 
+ Response 
    - http status code 

### Get deleted tickets 
<code>GET api/v1/ticket/deletedTickets/</code> 
- get deleted tickets for recycle bin 
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
<code>GET api/v1/ticket/deletedTickets/{id}</code> 
- Parameters 
    - id: int, ticket id 
- Response 
    - [ticket object](#ticket) 

### Restore a deleted ticket 
<code>POST api/v1/ticket/deletedTickets/{id}/restore </code> 
- Restore ticket from recycle bin 
- Parameters 
    - id: int, ticket id 
- Response 
    - [ticket object](#ticket)  

### Delete a ticket permanently 
<code>DELETE api/v1/ticket/deletedTickets/{id}</code> 
- delete ticket from recycle bin, permanently delete. including: message, attachment, ticket history. 
- Parameters 
    - id: int, ticket id 
- Response 
    - http status code 

### Get ticket draft 
<code>GET api/v1/ticket/tickets/{id}/draft</code> 
- Parameters 
    - id: int, ticket id 
- Response 
    - [ticket draft object](#ticketdraft) 

### Create ticket draft 
<code>POST api/v1/ticket/tickets/{id}/draft</code> 
- Parameters 
    - [ticket draft object](#ticketdraft) 
- Response 
    - [ticket draft object](#ticketdraft) 

### Update ticket draft 
<code>PUT api/v1/ticket/tickets/{id}/draft</code> 
- Parameters 
    - [ticket draft object](#ticketdraft) 
- Response 
    - [ticket draft object](#ticketdraft) 

### Delete ticket draft 
<code>DELETE api/v1/ticket/tickets/{id}/draft</code> 
- Parameters 
    - id: int, ticket id 
- Response 
    - http status code 

### Merge ticket 
<code>POST api/v1/ticket/tickets/{id}/merge</code> 
- merge source ticket to this ticket 
- Parameters 
    - id: int, target ticket id, 
    - sourceId: int, source ticket id 
- Response 
    - [ticket object](#ticket) 

### Get unread tickets number in filters 
<code>GET api/v1/ticket/filters/unreadCount?filterIds={filterid1}&filterIds={filterid2}&filterIds={filterid3}</code> 
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

### portalTicketNewMessage
| Name | Type | Description |
| - | - | - |
| `htmlBody` | string | html body of the message |   
| `plainBody` | string | plain text of the message |  
| `attachments` | [attachment](#attachment)[] | attachment array of message | 

## endpoints
### Get a ticket by ticket id
`get api/v1/ticket/portalTickets/{id}`
- Parameters: 
    - id, integer, ticket id
    - contactId, integer
- Response: 
    - [portal ticket object](#portalticket) 

### Get ticket list
`get api/v1/ticket/portalTickets`
- Parameters:
    - contactId, integer
    - startTime, DateTime, optional
    - endTime, DateTime, optional
- Response: 
    - [portal ticket object ](#portalticket)list

### Submit new ticket
`post api/v1/ticket/portalTickets`
- Parameters: 

| Name | Type | Description |
| - | - | - | 
| `subject` | string | ticket subject |
| `message` | [new message object](#portalticketnewmessage) | the first message |
| `contactId` | integer | id of the contact who submit the ticket | 
| `customFields` | [custom field value](#customfieldvalue)[] | custom field value array |

- Response: 
  - [portal ticket object](#portalticket) 

### Close ticket
`put api/v1/ticket/portalTickets/{id}/close` 
- Parameters: 
    - id, integer, ticket id,
    - contactId, integer
- Response: 
    - [portal ticket object](#portalticket) 

### Reopen ticket
`put api/v1/ticket/portalTickets/{id}/reopen` 
- Parameters: 
    - id, integer, ticket id,
    - contactId, integer
- Response: 
    - [portal ticket object](#portalticket) 

### Get message list of ticket
`get api/v1/ticket/portalTickets/{id}/messages`
- Parameters: 
    - id, integer, ticket id
    - contactId, integer
- Response: 
    - [message object](#message) list

### Reply ticket
 `post api/v1/ticket/portalTickets/{id}/messages`
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
<code>Get /api/v1/ticket/filters</code> 
- Parameters 
    - no parameters 
- Response 
    - [filter object](#filter) list, without conditions. 

### Create a new filter 
<code>POST api/v1/ticket/filters</code> 
- Parameters 
    - [filter object](#filter) 
- Response 
    - [filter object](#filter) list 

### Get a filter and its conditions 
<code>Get api/v1/ticket/filters/{id}</code> 
- Parameters 
    - id: int, filter id 
- Response 
    - [filter object](#filter) 

### Update filter 
<code>Put api/v1/ticket/filters/{id}</code> 
- Parameters 
    - [filter object](#filter) 
- Response 
    - [filter object](#filter) 

### Delete filter 
<code>Delete api/vi/ticket/filters/{id}</code> 
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
| `dataType` | string | `text`, `textarea`, `email`, <br>`url`, `date`, `integer`, <br>`float`, `operator`, `radio`,<br> `checkbox`, `dropdownList`, <br>`checkboxList`, `link`, `department` | 
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
<code>Get api/v1/ticket/fields</code> 
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
<code>Post /api/v1/ticket/attachments</code> 
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
| `blockType` | string | block type：`blockEmailasJunk`,<br> `rejectEmail`, `blockDomainasJunk`, `rejectDomain` | 

## endpoints 
### Get block sender list 
<code>Get /api/v1/ticket/blockedSenders?domain={domain}</code> 
- Parameters 
    - domain: string, the domain of email address 
- Response 
    - [block sender object](#blockedsender) list 

### Add block sender 
<code>POST api/v1/ticket/blockedSenders</code> 
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
<code>Get api/v1/ticket/cannedResponses</code> 
- Response 
    - [Canned responses object](#cannedresponse) list 


# Configs 
### Get ticket configs of this site 
<code>GET api/v1/ticket/configs</code> 
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
<code>GET api/v1/ticket/departments/{id} </code> 
- Response 
    - [department object](#department) 

### Get all departments 
<code>GET api/v1/ticket/departments</code> 
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
<code>GET api/v1/ticket/emailAccounts</code> 
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
| `htmlBody` | string | email body | 
| `plainBody` | string | email body | 
| `emailAccountId` | integer | receive email account id | 
| `isRead` | boolean | if is read | 
| `attachments` | [attachment](#attachment)[] | attachments | 

## endpoints 
### Get junk email list 
<code>GET api/v1/ticket/junkEmails</code> 

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
<code>GET api/v1/ticket/junkEmails/{id}</code> 
- Parameters 
    - id: int, email id 
- Response 
    - [junk email object](#junkemail) 

### Update junk email 
<code>PUT api/v1/ticket/junkEmails/{id}</code> 
- Parameters 
    - isRead: bool, 
- Response 
    - [junk email object](#junkemail) 

### Restore a junk email to a normal ticket 
<code>POST api/v1/ticket/junkEmails/{id}/notJunk</code> 
- Parameters 
    - id: int, email id 
- Response 
    - [ticket object](#ticket) 

### Delete a junk email 
<code>DELETE api/v1/ticket/junkEmails/{id}</code> 
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
<code>Get api/v1/ticket/tags</code> 
- Response 
    - [tag object](#tag) list 

### Add a tag 
<code>Post api/v1/ticket/tags</code> 
- Parameters 
    - name: string tag name 
- Response 
    - [tag object](#tag) 

### Update One Tag 
<code>Put api/v1/ticket/tags/{id}</code> 
- Parameters 
    - id: int, tag id 
    - name: string, tag name 
- Response 
    - [tag object](#tag) 

### Delete a tag 
<code>Delete api/v1/ticket/tags/{id}</code> 
- Parameters 
    - id: int, tag id 
- Response 
    - http status code


    