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
- Ticket system main enumeration types including: 

# Resource List 
|Name|EndPoint|Note| 
|---|---|---| 
|[Ticket](#ticket)|/api/v1/ticket/tickets|| 
|[TicketForContact](#ticketforcontact)|/api/v1/ticket/ticketsForContact|for contact|
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
| `channel` | string | `internal`, `portal`, `email`| 
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
| `ResolveBreachAt` | datetime | Timestamp that denotes when the ticket is <br/> due to be resolved | 
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
| `source` | string | `Note`, `Agentconsole`, `HelpDesk`, <br/>`API`, `Email`, `Chat`, `Offline Message`| 
| `originalId` | string | the original ID of message in original object |
| `content` | string | html content of message | 
| `contentText` | string | plain text of message | 
| `quote` | string | quoted content of the message, only for email message | 
| `senderId`| integer | id of agent or contact | 
| `senderType`| string | `agent` or `contact` or `system` | 
| `senderName` | string | sender name | 
| `senderAvatar` | string | the avatar url of sender | 
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
| `content` | string | html content of message | 
| `contentText` | string | plain text of message | 
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
    - filterId: int, Filter ID, optional 
    - tagId: int, Tag ID, optional 
    - keywords: string, optional 
    - pageIndex: int, optional 
    - sortBy: string, optional, `Next SLA Breach\ Last Reply Time\	Last Activity Time\	Priority\ Status` 
    - order: string, optional, `ascending` or `descending` 
    - conditions: optional, parameter format: `conditions[0][field]=agent&conditions[0][matchType]=is&conditions[0][value]=hi `

| Name | Type | Description | 
| - | - | - | 
| `fieldId` | integer | field id | 
| `matchType` | string | matach type: `contains`, `notContains`, `is`,<br/> `isNot`, `isMoreThan`, `isLessThan`, `before`, `after` | 
| `value` | string | condition value | 
+ Response 
    - [ticket object](#tickets) list, 
    - total: int, total number of tickets 
    - previousPage: string, next page uri, the first page return null. 
    - nextPage: string, the last page return null. 
    - currentPage: string, current page uri. 

### Get a ticket 
<code>GET api/v1/ticket/tickets/{id} </code> 
+ Parameters 
    - id: int, ticket ID 
+ Response 
    - [ticket object](#ticket) 

### Submit new ticket 
<code>POST api/v1/ticket/tickets</code> 
- Parameters 

| Name | Type | Description | 
| - | - | - | 
| `subject` | string | ticket subject | 
| `content` | string | the first message of the ticket. | 
| `agentAssignee` | integer | agent id | 
| `departmentAssignee` | integer | department id | 
| `contactId` | integer | the contact id or agent id | 
| `createdBy` | integer | contact id or agent id | 
| `createdByType` | string | contact or agent | 
| `channel` | string | `internal`, `portal`, `email`, `facebookMessenger`, `facebookVisitorPost`, `facebookWallPost`, `tweet`, `twitterDirectMessage`, `SMS`, `weChatMessage`| 
| `priority` | string | priority: `urgent`, `high`, `normal`, `low` | 
| `status` | string |  status of ticket: `new`, `pendingInternal`, `pendingExternal,`, `onHold`, `closed` | 
| `attachments` | [attachment](#attachment)[] | attachment array of the first message | 
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

| Name | Type | Description | 
| - | - | - | 
| `source` | string | `Note`, `Agentconsole`, `HelpDesk`, `API`, `Email`, `Chat`, `Offline Message`, `Facebook Messenger`, `Facebook Visitor Post`, `Facebook Wall Post`, `Tweet`, `Twitter Direct Message`, `SMS`, WeChat Message` | 
| `subject` | string | message subject | 
| `content` | string | html text of the message | 
| `contentText` | string | plain text of the message | 
| `originalId` | string | the original ID of the message | 
| `from` | string | message from email | 
| `to` | string | message to email when contact reply,<br/> to and from is nullable and don't send email | 
| `cc` | string | message cc emails | 
| `attachments` | [attachment](#attachment)[] | attachment array | 

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
    - id: int, ticket ID 
- Response 
    - [ticket object](#ticket) 

### Restore a deleted ticket 
<code>POST api/v1/ticket/deletedTickets/{id}/restore </code> 
- Restore ticket from recycle bin 
- Parameters 
    - id: int, ticket ID 
- Response 
    - [ticket object](#ticket)  

### Delete a ticket permanently 
<code>DELETE api/v1/ticket/deletedTickets/{id}</code> 
- delete ticket from recycle bin, permanently delete. including: message, attachment, ticket history. 
- Parameters 
    - id: int, ticket ID 
- Response 
    - http status code 

### Get ticket draft 
<code>GET api/v1/ticket/tickets/{id}/draft</code> 
- Parameters 
    - id: int, ticket ID 
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
    - id: int, ticket ID 
- Response 
    - http status code 

### Merge ticket 
<code>POST api/v1/ticket/tickets/{id}/merge</code> 
- merge source ticket to this ticket 
- Parameters 
    - id: int, target ticket ID, 
    - sourceId: int, source ticket ID 
- Response 
    - [ticket object](#ticket) 

### Get unread tickets number in filters 
<code>GET api/v1/ticket/filters/unreadCount?filterIds={filterid1}&filterIds={filterid2}&filterIds={filterid3}</code> 
- Parameters 
    - filterIds: filter id array 
- Response 
    - allCount: integer, all unread ticket number. 
    - array including: 
        - filterId: int, filter ID 
        - unreadCount: int, count unread tickets of a filter 
        - unreadMentionedCount: int, unread and metioned to me tickets number in filter 

# TicketForContact
## objects
### ticketForContact
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
`get api/v1/ticket/ticketsForContact/{id}`
- Parameters: 
    - id, integer, ticket id
    - contactId, integer
- Response: 
    - [ticket for contact object](#ticketforcontact) 

### Get ticket list
`get api/v1/ticket/ticketsForContact/`
- Parameters:
    - contactId, integer
    - startTime, DateTime, optional
    - endTime, DateTime, optional
- Response: 
    - [ticket for contact object list](#ticketforcontact)

### Submit new ticket
`post api/v1/ticket/ticketsForContact/`
- Parameters: 

| Name | Type | Description |
  | - | - | - | 
  | `subject` | string | ticket subject |
  | `content` | string | the content of the first message |
  | `contactId` | integer | id of the contact who submit the ticket |
  | `attachments` | [attachment](#attachment)[] | attachments of the first message |
  | `customFields` | [custom field value](#customfieldvalue)[] | custom field value array |

- Response: 
    - [ticket for contact object ](#ticketforcontact)

### Close ticket
`put api/v1/ticket/ticketsForContact/{id}/close` 
- Parameters: 
    - id, integer, ticket id,
    - contactId, integer
- Response: 
    - [ticket for contact object](#ticketforcontact)

### Reopen ticket
`put api/v1/ticket/ticketsForContact/{id}/reopen` 
- Parameters: 
    - id, integer, ticket id,
    - contactId, integer
- Response: 
    - [ticket for contact object](#ticketforcontact)

### Get message list of ticket
`get api/v1/ticket/ticketsForContact/{id}/messages`
- Parameters: 
    - id, integer, ticket id
    - contactId, integer
- Response: 
    - [message object list](#message) 

### Reply ticket
 `post api/v1/ticket/ticketsForContact/{id}/messages`
- Parameters:
| Name | Type | Description |
  | - | - | - | 
  | `ticketId` | integer | id of ticket |
  | `content` | string | message content |  
  | `contactId` | integer | contact id |  
  | `attachments` | [attachment](#attachment)[] | attachment array of message | 
- Response: 
    - [message object](#message) 

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
    - id: int, filter ID 
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
    - id: int, filter ID 
- Response 
    - http status code 


# Fields 
## objects 
### field 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | field id | 
| `dataType` | string | field date type: <br>`text`, `textarea`, `email`, <br>`url`, `date`, `integer`, <br>`float`, `operator`, `radio`,<br> `checkbox`, `dropdownList`, <br>`checkboxList`, `link`, `department` | 
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
| `ifAvailable` | boolean | if the attachment is available | 
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
| `type` | [department member type](#departmentmembertype) | agent or group | 

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
| `body` | string | email content | 
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
    - id: int, email ID 
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
    - id: int, email ID 
- Response 
    - [ticket object](#ticket) 

### Delete a junk email 
<code>DELETE api/v1/ticket/junkEmails/{id}</code> 
- Parameters 
    - id: int, junk email ID 
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
    - id: int, tag ID 
    - name: string, tag name 
- Response 
    - [tag object](#tag) 

### Delete a tag 
<code>Delete api/v1/ticket/tags/{id}</code> 
- Parameters 
    - id: int, tag ID 
- Response 
    - http status code


    