  | Change Version | API Version | Change nots | Change Date | Author |
  | - | - | - | - | - |
  | 4.0 | v3 | Ticketing Restful API | 2020-7-16 | Frank |  
 

# Authentication 
 

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
    | `get api/v3/ticketing/tickets` | assignedAgent, assignedDepartment, contactOrVisitor, createdBy, lastRepliedBy, lastMessage |
    | `get api/v3/ticketing/tickets/{id}` | assignedAgent, assignedDepartment, contactOrVisitor, createdBy, lastRepliedBy, messages, eventLogs |
    | `get api/v3/ticketing/tickets/{id}/messages` | sender, messageContact |
    | `get api/v3/ticketing/deletedTickets` | assignedAgent, assignedDepartment, contactOrVisitor, createdBy, lastRepliedBy |
    | `get api/v3/ticketing/deletedTickets/{id}` | assignedAgent, assignedDepartment, contactOrVisitor, createdBy, lastRepliedBy, messages |
    | `get api/v3/ticketing/deletedTickets/{id}/messages` | sender |
    | `get api/v3/ticketing/portalTickets/{id}` | contact, messages |
    | `get api/v3/ticketing/portalTickets` | contact |
    | `get api/v3/ticketing/portalTickets/{id}/messages` | sender | 
    | `get api/v3/ticketing/junks/` | sender | 

- Sample:
    - request: `get api/v3/ticketing/tickets/{id}?include=assignedAgent,createdBy,messages`
    - response: 

# Resource List 
|Name|EndPoint|Note| 
|---|---|---| 
|[Ticket](#tickets)|/api/v3/ticketing/tickets| Tickets | 
|[PortalTicket](#portalTickets)|/api/v3/ticketing/portalTickets| Portal tickets |
|[DeletedTicket](#DeletedTicket)|/api/v3/ticketing/deletedTickets| Deleted tickets |
|[Attachment](#attachments)|/api/v3/ticketing/attachments| Upload attachment for tickets | 
|[View](#views)|/api/v3/ticketing/views| Agent console views| 
|[Routing](#Routing)|/api/v3/ticketing/routing| Routing | 
|[AutoAllocation](#AutoAllocations)|/api/v3/ticketing/autoAllocation| Auto allocations | 
|[Trigger](#Triggers)|/api/v3/ticketing/triggers| Triggers| 
|[SLAPolicy](#SLAPolicies)|/api/v3/ticketing/SLAPolicies| SLA policies | 
|[WorkingTime&Holiday](#WorkingTime&Holiday)|/api/v3/ticketing/workingTime| Work time and holiday | 
|[Fields&Mapping](#fields&mapping)|/api/v3/ticketing/fields| System fields and custom fields | 
|[BlockedSender](#blockedsenders)|/api/v3/ticketing/blockedSenders| Blocked email or domain | 
|[Junk](#junks)|/api/v3/ticketing/junks| Emails from blocked senders | 
|[ChannelAccount](#channel-accounts)|/api/v3/ticketing/channelAccounts| Channel accounts | 
|[Channel](#channels)|/api/v3/ticketing/channels| integrated channels | 
|[Report](#reports)|/api/v3/ticketing/reports| Ticketing ticket reports | 

# Tickets 
## objects 
### ticket 
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | id of ticket | 
| `subject` | string | ticket subject | 
| `relatedType` | string | `contact`, `visitor`, `agent` | 
| `relatedId` | string | contact id, visitor id, agent id | 
| `assigneeType` | string | `agent`, `bot` | 
| `assigneeId` | string | assignee id | 
| `departmentAssigneeId` | string | department assignee id | 
| `priority` | string | `urgent`, `high`, `normal`, `low` | 
| `status` | string | `new`, `pendingInternal`, `pendingExternal`, `onHold`, `resolved` | 
| `hasDraft` | boolean | if has draft | 
| `mergedToTargetId` | int | merged ticket id | 
| `isReadByAgent` | boolean | if the ticket is read by agent | 
| `isReadByContact` | boolean | if the portal ticket is read by contact | 
| `createdById` | string | contact id or agent id or visitor id| 
| `createdByType` | string | `agent` or `contact` or `visitor` | 
| `createdTime` | datetime | create time of ticket | 
| `lastUpdatedTime` | datetime | last updated time of ticket | 
| `lastStatusChangedTime` | datetime | last status change time of ticket | 
| `lastRepliedTime` | datetime | last replied time | 
| `lastRepliedById` | string | contact id or agent id | 
| `lastRepliedByType` | string | `agent` or `contact` or `channelAccount` or `bot`|
| `resolvedById` | string | contact id or agent id| 
| `resolvedByType` | string | `agent` or `contact` or `system` | 
| `resolvedTime` | datetime | resolved time of ticket | 
| `firstMessageId` | string | the id of the first message | 
| `firstMessageChannelId` | string | the channel id of the first message | 
| `firstMessageChannelAccountId` | string | the channel account id of the first message | 
| `lastMessageId` | string | the id of the last message | 
| `lastMessageChannelId` | string | the channel id of the last message | 
| `lastMessageChannelAccountId` | string | the channel account id of the last message | 
| `totalReplies`| int | total replies number | 
| `firstRespondBreachAt` | datetime | Timestamp that denotes when the first response is due | 
| `nextRespondBreachAt` | datetime | Timestamp that denotes when the next response is due | 
| `resolveBreachAt` | datetime | Timestamp that denotes when the ticket is due to be resolved | 
| `nextSLABreachAt` | datetime | Timestamp that the next sla breach time | 
| `isEditable`| boolean | if the current agent can update\reply the ticket | 
| `originalConversationId` | string | original id on social platform | 
| `isDeleted` | boolean | if deleted | 
| `customFields` | [custom field value](#custom-field-value)[] | custom field value array | 
| `tagIds` | string[] | tag id array | 
| `mentionedAgents`|[mentioned agent](#mentioned-agent)[]| mentioned agents list | 
### custom field value
| Name | Type | Description | 
| - | - | - | 
| `id` | string | the id of custom field |
| `name` | string | the name of custom field |
| `value` | string | the value of custom field |

### custom field id and value
| Name | Type | Description | 
| - | - | - | 
| `id` | string | the id of custom field |
| `value` | string | the value of custom field |

### mentioned agent 
| Name | Type | Description | 
| - | - | - | 
| `agentId` | string | the agent id of mentioned | 
| `isRead`| boolean | if the mentioned ticket is read | 
| `messageId`| string | message id| 

### message 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | id of message | 
| `ticketId` | integer | id of ticket | 
| `body` | string | message text | 
| `type` | string | `text`, `html`,`audio`,`video`,`image`,`file`,`location`|
| `metadata` | string | json fomat, content of message |
| `parentId` | string | parent id |
| `time` | datetime | received time for incoming message, sent time for outgoing message | 
| `sentById`| string | id of agent or contact or visitor |
| `sentByType`| string | `agent` or `contact` or `system` or `bot` or `channelAccount` or `visitor` |  
| `attachments`| [attachment](#attachment)[] | attachment array |  
| `messageDelivery` | [messageDelivery](#messageDelivery) | message delivery object |  

### messageDelivery
| Name | Type | Description | 
| - | - | - | 
| `messageId` | string | id of message | 
| `status` | string | send status：`waitForSending`,`sending`,`failed` | 
| `failReason` | string | fail reason | 

### ticket draft 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | id of message draft | 
| `ticketId` | integer | id of ticket | 
| `channelId` | string | channel id | 
| `channelAccountId`| string | channel account id | 
| `contactIdentityId`| string | id of contact identity |
| `parentId` | string | parent id |
| `subject` | string | subject | 
| `quote` | string | quote for email | 
| `cc` | string | cc email addresses |  
| `quote` | string | email quote | 
| `contents` | [content](#content)[] | content array | 
| `sentById`| string | id of agent| 
| `sentTime` | datetime | the sent time of the message | 
  
 ### event log
| Name | Type | Description | 
| - | - | - | 
| `id` | string | event log id | 
| `ticketId` | integer | id of ticket | 
| `text` | string | event log text | 
| `time` | datetime | event time | 

## endpoints 
|EndPoint|Note| 
|---|---|
| `get api/v3/ticketing/tickets`  |  [List tickets](#List-tickets)  |
| `get api/v3/ticketing/tickets/{id}` |  [Get a ticket](#Get-a-ticket)  |
| `post api/v3/ticketing/tickets` | [Submit a new ticket](#Submit-a-new-ticket) |
| `put api/v3/ticketing/tickets/{id}` | [Update a ticket ](#Update-a-ticket ) |
| `put api/v3/ticketing/tickets/`  | [Batch update tickets](#Batch-update-tickets) |
| `put api/v3/ticketing/tickets/{id}/read`  | [Mark a ticket as read](#Mark-a-ticket-as-read) |
| `put api/v3/ticketing/tickets/{id}/unread`  | [Mark-a-ticket-as-unread ](#) |
| `post api/v3/ticketing/tickets/{id}/merge` | [ Merge a ticket ](#Merge-a-ticket) |
| `get api/v3/ticketing/tickets/{id}/agents`  | [Get agents of openning ticket](#Get-agents-who-are-openning-the-ticket) |
| `get api/v3/ticketing/tickets/unreadCount` | [List unread tickets number for views](#List-unread-tickets-number-for-views) |
| `get api/v3/ticketing/tickets/{id}/eventLogs` | [ List ticket event logs ](#List-ticket-event-logs) |
| `delete api/v3/ticketing/tickets/{id}` | [Delete a ticket ](#Delete-a-ticket ) |
| `delete api/v3/ticketing/tickets`  | [Batch delete tickets ](#Batch-delete-tickets ) |
| `get api/v3/ticketing/tickets/{id}/messages` | [List messages of a ticket](#List-messages-of-a-ticket) |
| `get api/v3/ticketing/tickets{id}/messages/{messageId}`  | [Get a message](#Get-a-message) |
| `post api/v3/ticketing/tickets/{id}/messages` | [Reply a message](#Reply-a-message) |
| `put api/v3/ticketing/tickets/{id}/messages/{messageId}/resend`  | [Resend a message](#Resend-a-message) |
| `put api/v3/ticketing/tickets/{id}/messages/{messageId}/read` | [Mark a message as read](#Mark-a-message-as-read) |
| `put api/v3/ticketing/tickets/{id}/messages/{messageId}/unread`   | [Mark a message as unread ](#Mark-a-message-as-unread) | 
| `get api/v3/ticketing/tickets/{id}/draft`  | [Get a ticket draft ](#Get-a-ticket-draft) |
| `post api/v3/ticketing/tickets/{id}/draft`  | [Create a ticket draft ](#Create-a-ticket-draft) |
| `put api/v3/ticketing/tickets/{id}/draft`  | [Update a ticket draft ](#Update-a-ticket-draft) |
| `delete api/v3/ticketing/tickets/{id}/draft`  | [ Delete a ticket draft ](#Delete-a-ticket-draft) |


### List tickets 
`get api/v3/ticketing/tickets` 
+ Each request returns a maximum of 50 tickets. 
+ Parameters 
    - viewId: string, view id
    - tagId: string, tag id
    - keywords: string
    - timeFrom: DateTime, last reply time, default search the last 30 days
    - timeTo: DateTime, last reply time, default value is the current time
    - timeZoneOffset, float, time zone of your time parameters
    - pageIndex: integer, default 1
    - pageSize: integer, default 20, max value 50
    - sortBy: string, `nextSLABreach`, `lastReplyTime`, `lastActivityTime`, `priority`, `status` , default value: `lastReplyTime`
    - sortOrder: string, `ascending` or `descending`, default value: `descending`
    - conditions: parameter format: `conditions[0][field]=subject&conditions[0][matchType]=is&conditions[0][value]=hi&conditions[1][field]=status&conditions[1][matchType]=is&conditions[1][value]=1`, fields can be ticket system fields and custom fields.  
        - field: string, field name
        - matchType: string 
        - value: string

    Here is the list of match types and values supported by ticket system field.    
    
    | Field | Match Type | Values |
    | - | - | - |
    | Ticket Id | Is, IsNot  | number |
    | Subject | Contains, NotContains  | string |
    | Assigned Department | Is, IsNot  | Department Id |
    | Assigned Agent | Is, IsNot  | Agent Id |
    | Status | Is, IsNot  | `new`, `pendingExternal`, `pendingInternal`, `onHold`, `resolved` |
    | Priority | Is, IsNot  | `urgent`, `high`, `normal`, `low` |
    | Created Time | Is, IsNot, Before, After | time format: `2019-01-03` |
    | Last Updated Time | Is, IsNot, Before, After | time format: `2019-01-03` |
    | Last Status Changed Time | Is, IsNot, Before, After | time format: `2019-01-03` |
    | Closed Time | Is, IsNot, Before, After | time format: `2019-01-03` |
    | Total Replies | Is, IsNot, IsMoreThan, IsLessThan | number |
    | @Mentioned Agent | Is, IsNot | number, agent Id |
    | First Message Channel | Is, IsNot | guid, channel Id |
    | First Message Channel Account | Is, IsNot | guid, channel account Id |
    | last Message Channel | Is, IsNot | guid, channel Id |
    | last Message Channel Account | Is, IsNot | guid, channel account Id |
    | is Multi Channel | Is, IsNot | bool, if the ticket is multi channel ticket |

    Here is the list of match types and values supported by ticket custom field.    

    | Field DataType | Match Type | Values |
    | - | - | - |
    | Date | Is, IsNot，After, Before | time format: `2019-01-03` |
    | Drop-down list | Is, IsNot | option text |
    | Check-box list | Is, IsNot | option text |
    | Radio button | Is, IsNot | option text |
    | Check-box | Is, IsNot | `true` or 1, `false` or 0 |
    | Single-line text box | Contains, NotContains | string |
    | Multi-line text box | Contains, NotContains | string |
    | Agent | Is, IsNot | agent id |
    | Department | Is, IsNot | department id |
    | Link | Contains, NotContains | string |
    | Url | Contains, NotContains | string |


+ Response 
    - tickets: [ticket](#tickets) list, 
    - total: integer, total number of tickets 
    - previousPage: string, next page uri, the first page return null. 
    - nextPage: string, the last page return null. 
    - currentPage: string, current page uri. 
+ Includes

    | Includes | Description |
    | - | - |
    | assignedAgent | `get api/v3/ticketing/tickets?include=assignedAgent` |
    | assignedDepartment | `get api/v3/ticketing/tickets?include=assignedDepartment` |
    | contactOrVisitor | `get api/v3/ticketing/tickets?include=contactOrVisitor` |
    | createdBy | `get api/v3/ticketing/tickets?include=createdBy` |
    | lastRepliedBy | `get api/v3/ticketing/tickets?include=lastRepliedBy` | 
    | lastMessage | `get api/v3/ticketing/tickets?include=lastMessage` |

### Get a ticket 
`get api/v3/ticketing/tickets/{id}` 
+ Parameters 
    - id: integer, ticket  
+ Response 
    - [ticket](#ticket) 
+ Includes

    | Includes | Description |
    | - | - |
    | assignedAgent | `get api/v3/ticketing/tickets/{id}?include=assignedAgent` |
    | assignedDepartment | `get api/v3/ticketing/tickets/{id}?include=assignedDepartment` |
    | contactOrVisitor | `get api/v3/ticketing/tickets/{id}?include=contactOrVisitor` |
    | createdBy | `get api/v3/ticketing/tickets/{id}?include=createdBy` |
    | lastRepliedBy | `get api/v3/ticketing/tickets/{id}?include=lastRepliedBy` |
    | messages | `get api/v3/ticketing/tickets/{id}?include=messages` |
    | eventLogs | `get api/v3/ticketing/tickets/{id}?include=eventLogs` |
    | lastMessage | `get api/v3/ticketing/tickets/{id}?include=lastMessage` |
 
### Submit a new ticket
`post api/v3/ticketing/tickets` 
- Parameters 
    - subject: string, ticket subject, required 
    - assignedAgentId: string, agent id
    - assignedDepartmentId: string, department id
    - assignedBotId: string, bot id
    - assignedType: string, `agent`, `bot`
    - priority: string, `urgent`, `high`, `normal`, `low`, default value: `normal` 
    - status: string, `new`, `pendingInternal`, `pendingExternal`, `onHold`, `resolved`, default value: `new` 
    - customFields: [custom field id and value](#custom-field-id-and-value)[], custom field value array
    - tagIds: string[], tag id array
    - message: the first message of the ticket, required
        - channelId: string, channel Id, required
        - channelAccountId: string, channel account id,
        - contactIdentityId: int, contact identity id,
        - subject: string, for email message, email subject 
        - cc: string, message cc emails
        - contents: [content](#content)[],
+ Response 
    - [ticket](#tickets)

### Update a ticket 
`put api/v3/ticketing/tickets/{id}` 
- Parameters 
    - id: integer, ticket id
    - subject: string, ticket subject
    - relatedType: string, `contact`, `visitor`
    - relatedId: integer, contact id or visitor id
    - assignedType: string, `agent`, `bot`
    - assignedBotId: string, bot id
    - assignedAgentId: integer, agent id
    - assignedDepartmentId: string, department id
    - priority: string, priority: `urgent`, `high`, `normal`, `low`
    - status: string, `new`, `pendingInternal`, `pendingExternal,`, `onHold`, `resolved`
    - isRead: boolean
    - isActive: boolean
    - customFields: [custom field id and value](#custom-field-id-and-value)[], custom field value array
    - tagIds: string[], tag id array
- Response 
    - [ticket](#ticket) 

### Batch update tickets
`put api/v3/ticketing/tickets/` 
+ Parameters 
    - ids: integer[], ticket id array
    - status, string
    - priority, string
    - assignedType: string, `agent`, `bot`
    - assignedBotId: string, bot id
    - assignedAgentId: string, agent id
    - assignedDepartmentId, string
    - isRead, boolean
+ Response 
    - [ticket](#ticket) list 

### List ticket event logs
`get api/v3/ticketing/tickets/{id}/eventLogs`
- Parameters 
    - id: integer, ticket id 
- Response 
    - [event log](#event-log)

### List messages of a ticket 
`get api/v3/ticketing/tickets/{id}/messages` 
+ Parameters 
    - id: integer, ticket id 
+ Response 
    - [message](#message) list 
+ Includes

    | Includes | Description |
    | - | - |
    | sender | `get api/v3/ticketing/tickets/{id}/messages?include=sender` |
    | messagecontact | `get api/v3/ticketing/tickets/{id}/messages?include=messagecontact` |

### Get a message 
`get api/v3/ticketing/tickets{id}/messages/{messageId}` 
+ Parameters 
    - id: integer, ticket id 
    - messageId: string, message id
+ Response 
    - [message](#message)
+ Includes

    | Includes | Description |
    | - | - |
    | sender | `get api/v3/ticketing/tickets/{id}/messages/{messageId}?include=sender` |

### Reply a message 
`post api/v3/ticketing/tickets/{id}/messages` 
- Parameters  
    - type: string, `note`, `message`, required
    - channelId: string, channel Id, required
    - channelAccountId: string, channel account id, 
    - subject: string, for email message, email subject
    - cc: string, message cc emails 
    - parentId: string,
    - contents: [content](#content)[]
    - sentTime: datetime, send time
- Response 
    - [message](#message) 

### Resend a message 
`put api/v3/ticketing/tickets/{id}/messages/{messageId}/resend` 
- Parameters  
    - id: integer, ticket id,
    - messageId: string, message id,
- Response 
    - [message](#message) 

### Reply bot response with an intent
`post api/v3/ticketing/tickets/{id}/messages/botIntent`
- Parameters  
    - id: number, ticket id,
    - channelId: string, channel id, required
    - channelAccountId: string, required
    - botId: string, bot id, required
    - intentId: string, bot intent id, required
- Response 
    - http status code

### Mark a ticket as read 
`put api/v3/ticketing/tickets/{id}/read` 
+ Parameters 
    - id: integer, ticket id 
+ Response 
    - http status code

### Mark a ticket as unread 
`put api/v3/ticketing/tickets/{id}/unread` 
+ Parameters 
    - id: integer, ticket id 
+ Response 
    - http status code

### Mark a message as read 
`put api/v3/ticketing/tickets/{id}/messages/{messageId}/read` 
+ Parameters 
    - id: number, ticket id,
    - messageId: string, message id,
+ Response 
    - http status code

### Mark a message as unread 
`put api/v3/ticketing/tickets/{id}/messages/{messageId}/unread` 
+ Parameters 
    - id: number, ticket id,
    - messageId: string, message id,
+ Response 
    - http status code

### Get agents who are openning the ticket 
`get api/v3/ticketing/tickets/{id}/activedAgents` 
+ Parameters 
    - id: number, ticket id,
+ Response 
    - agent list

### Delete a ticket 
`delete api/v3/ticketing/tickets/{id}` 
- Parameters 
    - id: integer, ticket id
- Response 
    - http status code 

### Batch delete tickets 
`delete api/v3/ticketing/tickets` 
+ Parameters 
    - ids: integer[], id array
+ Response 
    - http status code 

### Get a ticket draft 
`get api/v3/ticketing/tickets/{id}/draft` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - [ticket draft](#ticket-draft) 

### Create a ticket draft 
`post api/v3/ticketing/tickets/{id}/draft` 
- Parameters 
    - [ticket draft](#ticket-draft) 
- Response 
    - [ticket draft](#ticket-draft) 

### Update a ticket draft 
`put api/v3/ticketing/tickets/{id}/draft` 
- Parameters 
    - [ticket draft](#ticket-draft) 
- Response 
    - [ticket draft](#ticket-draft) 

### Delete a ticket draft 
`delete api/v3/ticketing/tickets/{id}/draft` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - http status code 

### Merge a ticket 
`post api/v3/ticketing/tickets/{id}/merge`
- Parameters 
    - id: integer, target ticket id, 
    - sourceId: integer, source ticket id 
- Response 
    - [ticket](#ticket) 

### List unread tickets number for views 
`get api/v3/ticketing/tickets/unreadCount?viewIds={viewid1}&viewIds={viewid2}&viewIds={viewid3}`
- Parameters 
    - viewIds: view id array 
- Response 
    - allCount: integer, all unread ticket number. 
    - array including: 
        - viewId: string, view id 
        - unreadCount: integer, count unread tickets of a view 
        - unreadMentionedCount: integer, the number of tickets which is unread and mentioned to me 

# DeletedTicket

## endpoints 

|EndPoint|Note| 
|---|---|
| `get api/v3/ticketing/deletedTickets/`  | [List deleted tickets ](#List-deleted-tickets ) |
| `get api/v3/ticketing/deletedTickets/{id}`  | [Get a deleted ticket](#Get-a-deleted-ticket) |
| `get api/v3/ticketing/deletedTickets/{id}/messages`  | [List messages of a deleted ticket](#List-messages-of-a-deleted-ticket) |
| `delete api/v3/ticketing/deletedTickets/{id}`  | [Delete a ticket permanently ](#Delete-a-ticket-permanently) |
| `post api/v3/ticketing/deletedTickets/{id}/restore `  | [Restore a deleted ticket ](#Restore-a-deleted-ticket) |

### List deleted tickets 
`get api/v3/ticketing/deletedTickets/` 
- Parameters 
    - keywords: string
    - pageIndex: integer
    - pageSize: integer, default 20, max value 50
    - timeFrom: DateTime, last reply time, default search the last 30 days
    - timeTo: DateTime, last reply time, default value is the current time
- Response 
    - deletedTickets: [ticket](#ticket) list 
    - total: integer, total number of tickets 
    - previousPage: string, next page uri, the first page return null. 
    - nextPage: string, the last page return null. 
    - currentPage: string, current page uri. 
- Includes

    | Includes | Description |
    | - | - |
    | assignedAgent | `get api/v3/ticketing/deletedTickets?include=assignedAgent` |
    | assignedDepartment | `get api/v3/ticketing/deletedTickets?include=assignedDepartment` |
    | contactOrVisitor | `get api/v3/ticketing/deletedTickets?include=contactOrVisitor` |
    | createdBy | `get api/v3/ticketing/deletedTickets?include=createdBy` |
    | lastRepliedBy | `get api/v3/ticketing/deletedTickets?include=lastRepliedBy` | 
    | lastMessage | `get api/v3/ticketing/deletedTickets?include=lastMessage` | 

### Get a deleted ticket 
`get api/v3/ticketing/deletedTickets/{id}` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - [ticket](#ticket) 
- Includes

    | Includes | Description |
    | - | - |
    | assignedAgent | `get api/v3/ticketing/deletedTickets/{id}?include=assignedAgent` |
    | assignedDepartment | `get api/v3/ticketing/deletedTickets/{id}?include=assignedDepartment` |
    | contactOrVisitor | `get api/v3/ticketing/deletedTickets/{id}?include=contactOrVisitor` |
    | createdBy | `get api/v3/ticketing/deletedTickets/{id}?include=createdBy` |
    | lastRepliedBy | `get api/v3/ticketing/deletedTickets/{id}?include=lastRepliedBy` |
    | messages | `get api/v3/ticketing/deletedTickets/{id}?include=messages` |
    | eventLogs | `get api/v3/ticketing/deletedTickets/{id}?include=eventLogs` |

### List messages of a deleted ticket
`get api/v3/ticketing/deletedTickets/{id}/messages` 
- Parameters 
    - id: integer, ticket id
- Response 
    - [message](#message) 
- Includes

    | Includes | Description |
    | - | - |
    | sender | `get api/v3/ticketing/deletedTickets/{id}/messages?include=sender` |

### Delete a ticket permanently 
`delete api/v3/ticketing/deletedTickets/{id}` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - http status code 

### Restore a deleted ticket 
`post api/v3/ticketing/deletedTickets/{id}/restore ` 
- Parameters 
    - id: integer, ticket id 
- Response 
    - [ticket](#ticket)  
 
# Attachments  
## objects
### attachment 
| Name | Type | Description | 
| - | - | - |
| `id` | string | attachment unique id |  
| `fileKey` | string | file key |  
| `name` | string | attachment file name| 
| `size` | int | size file name| 
| `url` | string | attachment download link |   

## endpoints 
### Update status of attachment
`Put /api/v3/ticketing/attachments/{guid}`
#### Parameters:
- isAvailable - boolean `true` or  `false`
#### Response
- [attachment](#attachment) 

### Delete attachment 
`delete /api/v3/ticketing/attachments/{guid}` 
- Parameters 
    - guid: string, the guid of the attachment
- Response 
    - httpStatusCode


# Views 
## objects 
### view 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | view id | 
| `name` | string | view name | 
| `isPrivate` | boolean | if private view| 
| `createdById` | string | agent id | 
| `conditions` | [conditions](#conditions) | view conditions | 

### Conditions 
  | Name | Type |Description |
  | - | - | - | 
  | `when` | string | when the rule is triggered, including `all`, `any` and `logicalExpression` |
  | `logicExpression` | string | the logical expression of the conditions |
  | `list` | [condition](#condition)[] | an array of condition |

### condition
| Name | Type | Description | 
| - | - | - | 
| `id` | string | id of the condition |
| `type` | string | `view`, `trigger`, `sla`, `routingRule` |
| `fieldId` | string | field id | 
| `matchType` | string | `contains`, `notContains`, `is`, `isNot`, `isMoreThan`, `isLessThan`, `before`, `after` | 
| `value` | string | condition value | 
| `orderNum` | integer | order number | 

## endpoints 
### List views 
`get /api/v3/ticketing/views`
- Parameters 
    - no parameters 
- Response 
    - [view](#view) list

### Create a new view 
`post api/v3/ticketing/views`
- Parameters 
    - name: string, view name, required 
    - isPrivate: boolean, if private view, default value: `false` 
    - conditions: [conditions](#conditions), view conditions
- Response 
    - [view](#view) list 

### Get a view and its conditions 
`get api/v3/ticketing/views/{id}` 
- Parameters 
    - id: string, view id 
- Response 
    - [view](#view) 

### Update a view 
`put api/v3/ticketing/views/{id}` 
- Parameters 
    - id: string, view id 
    - name: string, view name, required 
    - isPrivate: boolean, if private view 
    - conditions: [conditions](#conditions), view conditions
- Response 
    - [view](#view) 

### Delete a view 
`delete api/v3/ticketing/views/{id}` 
- Parameters 
    - id: string, view id 
- Response 
    - http status code 


# Routing
## objects
### routing
| Name | Type | Description |
| - | - | - |
| `isEnabled` | boolean | whether the routing is enabled or not.
| `type` |string | the type of routing, including `simple`and `customRules`. |
| `simpleRouteType` | string | the rule of route ,including `department` and `agent` |
| `simpleRouteToId` | string | route to agent id |
| `simpleRouteToDepartmentId` | string | route to department id |
| `simpleRouteWithPriority` | string | `urgent`, `high`, `normal`, `low` |
| `percentageToBotWhenAgentsOnline` | integer | Percentage of new ticket to bot when agents are online when simple routing |
| `percentageToBotWhenAgentsOffline` | integer | Percentage of new ticket to bot when agents are offline when simple routing |
| `customRules` | [customRule](#customRule)[] | an array of [customRule](#customRule) json object. |
| `matchFailedType` | string | the rule of failed route  including `department` and `agent` |
| `matchFailedrouteToId` | string | match failed route to agent id |
| `matchFailedrouteToDepartmentId` | string | match failed route to department id |
| `matchFailedWithPriority` | string | `urgent`, `high`, `normal`, `low` |
| `matchFailedPercentageToBotWhenAgentsOnline` | integer | Percentage of new ticket to bot when agents are online when match failed|
| `matchFailedPercentageToBotWhenAgentsOffline` | integer | Percentage of new ticket to bot when agents are offline when match failed |

### customRule
| Name | Type | Description |
| - | - | - |
| `id` | string | id of the custom rule |
| `orderNum` | integer | order of the custom rule |
| `isEnabled` | boolean | whether the custom rule is enabled or not. |
| `name` | string | name of the custom rule |
| `conditions` | [conditions](#conditions)  | an trigger condition json object. |
| `routeType` | string | type of the route, including `agent` and `department`, value `department` is available when config of department is open. 
| `routeToId` | string | route to agent id  |
| `routeToDepartmentId` | string | route to department id |
| `routeWithPriority` | string | ticket priority enum number|
| `percentageToBotWhenAgentsOnline` | integer | Percentage of new ticket to bot when agents are online |
| `percentageToBotWhenAgentsOffline` | integer | Percentage of new ticket to bot when agents are offline |

## endpoints
### get routing
`get api/v3/ticketing/routing`
+ Parameters
    - no parameters
+ Response
    - [routingRule](#routingRule)

### Enable/Disable routing
`put api/v3/ticketing/routing/enable`
+ Parameters
    - isEnabled: boolean,
+ Response
    - http status code

### Update routing
`put api/v3/ticketing/routing/`
+ Parameters
    - isEnabled: boolean
    - type: string, `simple` or `customRules`
    - simpleRouteType: string, department and agent
    - simpleRouteToId: string, simple route to agent id
    - simpleRouteToDepartmentId, string, simple route to department id
    - simpleRoutePercentageOfNewTicketToBotWhenAgentsOnline: integer
    - simpleRoutePercentageOfNewTicketToBotWhenAgentsOffline: integer
    - simpleRouteWithPriority: string,
    - matchFailedType: string, department and agent
    - matchFailedToId: string
    - matchFailedToDepartmentId: string
    - matchFailedWithPriority: string
    - matchFailedPercentageOfNewTicketToBotWhenAgentsOnline: integer
    - matchFailedPercentageOfNewTicketToBotWhenAgentsOffline: integer 
+ Response
    - [routing](#routing)

### Enable/Disable a custom rule
`put api/v3/ticketing/routing/customRules/{id}/enable`
+ Parameters
    - id: string
    - isEnabled: boolean
+ Response
    - [customRule](#customRule) 

### Create a custom rule
`post api/v3/ticketing/routing/customRules`
+ Parameters
    - customRule: [customRule](#customRule)
+ Response
    - [customRule](#customRule)

### Get a custom rule
`get api/v3/ticketing/routing/customRules/{id}`
+ Parameters
    - id: string
+ Response
    - [customRule](#customRule)

### Update a custom rule
`put api/v3/ticketing/routing/customRules/{id}`
+ Parameters
    - customRule: [customRule](#customRule)
+ Response
    - [customRule](#customRule)


### Delete a custom rule
`put api/v3/ticketing/routing/customRules/{id}`
+ Parameters
    - id: string
+ Response
    - http status code

### update the order number of a custom rule
`put api/v3/ticketing/routing/customRules/{id}/sort`
+ Parameters
    - id: string
    - type: string, `up`, `down`
+ Response
    - http status code

# AutoAllocations
## objects
### autoAllocationSetting
| Name | Type | Description | 
| - | - | - | 
| `isEnabled` | boolean | if enabled Auto Allocation |
| `departmentAllocationRule`| [allocationRule](#allocationRule)[] | array of department allocation rules |
| `defaultRule`| string | `loadBalancing`, `roundRobin` |
| `defaultPreferLastAssignee`| boolean | prefer to allocate to the last assignee |
| `ifEnableTicketLimitForEachAgent` | boolean | if set maximum number of tickets an agent can be allocated to |
| `ticketLimitForAllAgents` | integer | maximum number of tickets all agents can be allocated to |
| `agentPreferences` | [agentPreference](#agentPreference)[] | agent preference for allocation |
| `excludePendingExternal` | boolean | if exclude `Pending External` status while validating if an agent has reached the max number |
| `excludeOnHold` | boolean | if exclude `On Hold` status while validating if an agent has reached the max number |

### allocationRule
| Name | Type | Description | 
| - | - | - | 
| `departmentId` | string | department id |
| `departmentName` | string | department name |
| `allocationRule`| string | `loadBalancing`, `roundRobin` |
| `preferLastAssignee` | boolean | prefer to allocate to the last assignee |

### agentPreference
| Name | Type | Description | 
| - | - | - | 
| `agentId` | string | agent id |
| `maxConcurrentCount` | integer | the maximum number of the tickets a agent can accept at the same time |
| `isAcceptAllocation` | boolean | if the agent accept tickets |

## endpoints
### Get auto allocation settings
`get api/v3/ticketing/autoAllocation`
+ Parameters
    - no parameters
+ Response
    - [autoAllocationSetting](#autoAllocationSetting)

### Enable/Disable auto allocation
`put api/v3/ticketing/autoAllocation/enable`
+ Parameters
    - isEnabled: boolean, if enabled auto allocation
+ Response
    - http status code

### Update auto allocation settings
`put api/v3/ticketing/autoAllocation`
+ Parameters
    - isEnabled: boolean, if enabled auto allocation
    - allocationRuleSettings: [allocationRuleSetting](#allocationRuleSetting)[]
    - ifEnableTicketLimitForEachAgent: boolean, if set maximum number of tickets an agent can be allocated to
    - ticketLimitForAllAgents: integer, maximum number of tickets all agents can be allocated to
    - allocationAgentPreferences: [allocationAgentPreference](#allocationAgentPreference)[]
    - ifExcludePendingExternal: boolean, if exclude `Pending External` status while validating if an agent has reached the max number
    - ifExcludeOnHold: boolean, if exclude `On Hold` status while validating if an agent has reached the max number
+ Response
    - [autoAllocationSetting](#autoAllocationSetting)

# Triggers
## object
### trigger
| Name | Type | Description | 
| - | - | - | 
| `id` | string | id of the trigger |
| `description` | string | description of the trigger |
| `isEnabled` | boolean | if enabled the trigger |
| `event` | string |  `ticketCreated`, `ticketReplyReceived`, `agentReplied`, `ticketAssigneeChanged`, `ticketStatusChanged`, ` ticketStatusLastForCertainTime` |
| `conditions` | [conditions](#conditions) | trigger conditions | 
| `ifSetValue` | boolean | if set value |
| `autoUpdate` | [autoUpdate](#autoUpdate)[] | auto update field value |
| `ifSendEmail` | boolean | if send email |
| `ifSendToContacts` | boolean | if send email to contacts|
| `ifSendToAgents` | boolean | if send email to agents |
| `recipientAgentIds` | string[] | agent id array of recipient |
| `subject` | string | subject of the email content |
| `htmlText` | string | html body |
| `plainText` | string | plain text |
| `attachments` | [attachment](#attachment)[] | attachments |
| `ifShowInTicketCorrespondences` | boolean | if show trigger email in Ticket Correspondence |
| `orderNum` | integer | trigger execute and display order |

### autoUpdate
| Name | Type | Description | 
| - | - | - | 
| `id` | string | id of the autoUpdate |
| `triggerId` | string | trigger id |
| `fieldId` | string | field id | 
| `value` | string | condition value | 

## endpoints
### List all triggers
`get api/v3/ticketing/triggers`
+ Parameters 
    - no parameters
+ Response
    - [trigger](#trigger) list

### Get a trigger
`get api/v3/ticketing/triggers/{id}`
+ Parameters
    - id: string, trigger id
+ Response
    - [trigger](#trigger)

### Submit a trigger
`post api/v3/ticketing/triggers`
+ Parameters
    - description, string, description of the trigger
    - isEnabled, boolean, if enabled the trigger
    - event, string, `ticketCreated`, `ticketReplyReceived`, `agentReplied`, `ticketAssigneeChanged`, `ticketStatusChanged`, ` ticketStatusLastForCertainTime`
    - conditions, [conditions](#conditions), conditions 
    - ifSetValue, boolean, if set value
    - autoUpdate, [autoUpdate](#autoUpdate)[], auto update field value
    - ifSendEmail, boolean, if send email
    - ifSendToContacts, boolean, if send email to contacts
    - ifSendToAgents, boolean, if send email to agents
    - recipientAgentIds, string[], agent id array of recipient
    - subject, string, subject of the email content
    - htmlText, string, html body
    - plainText, string, plain text
    - attachments, [attachment](#attachment)[], attachment array
    - ifShowInTicketCorrespondences, boolean, if show trigger email in Ticket Correspondence
+ Response
    - [trigger](#trigger)

 ### Update a trigger
`put api/v3/ticketing/triggers/{id}`
+ Parameters
    - id, string, id of the trigger
    - description, string, description of the trigger
    - isEnabled, boolean, if enabled the trigger
    - event, string,  `ticketCreated`, `ticketReplyReceived`, `agentReplied`, `ticketAssigneeChanged`, `ticketStatusChanged`, ` ticketStatusLastForCertainTime` 
    - conditions, [conditions](#conditions), conditions 
    - ifSetValue, boolean, if set value
    - autoUpdate, [autoUpdate](#autoUpdate)[], auto update field value
    - ifSendEmail, boolean, if send email
    - ifSendToContacts, boolean, if send email to contacts
    - ifSendToAgents, boolean, if send email to agents
    - recipientAgentIds, integer[], agent id array of recipient
    - subject, string, subject of the email content
    - htmlText, string, html body
    - plainText, string, plain text
    - attachments, [attachment](#attachment)[], attachment array
    - ifShowInTicketCorrespondences, boolean, if show trigger email in Ticket Correspondence
+ Response
    - [trigger](#trigger)

### Upgrade/Downgrade a triggers
`put api/v3/ticketing/triggers/{id}/sort`
+ Parameters
    - id: string, trigger id
    - type: string, `up`, `down`
+ Response
    - http status code

### Enable/Disable a triggers
`put api/v3/ticketing/triggers/{id}`
+ Parameters
    - id: string, trigger id
    - isEnabled: boolean, if enabled the trigger
+ Response
    - http status code

 ### Delete a trigger
`delete api/v3/ticketing/triggers/{id}`
+ Parameters
    - id: string, trigger id
+ Response
    - http status code

# SLAPolicies
## objects
### SLAPolicy
| Name | Type | Description | 
| - | - | - | 
| `id` | string | SLA policy id |
| `isEnabled` | boolean | if enabled this SLA policy |
| `firstRespondWithin` | integer | the hours first reply within |
| `nextRespondWithin` | integer | the hours next reply within |
| `resolveRespondWithin` | integer | the hours a ticket should be resolved within |
| `isBusinessHours`| boolean | if `businessHours` or `calenderHours` |
| `conditions` | [conditions](#conditions) | conditions | 
| `orderNum` | integer | SLA execute and display order |

## endpoints
### List all SLA policies
`get api/v3/ticketing/SLAPolicies`
+ Parameters
    - no parameters
+ Response
    - [SLAPolicy](#SLApolicy) list

### Get a SLA policy
`get api/v3/ticketing/SLAPolicies/{id}`
+ Parameters
    - id: string, SLA policy id
+ Response
    - [SLAPolicy](#SLApolicy)

### Create a SLA policy
`post api/v3/ticketing/SLAPolicies`
+ Parameters
    - isEnabled: boolean, if enabled this SLA policy 
    - firstRespondWithin: integer, 
    - nextRespondWithin: integer, 
    - resolveRespondWithin: integer, 
    - isBusinessHours: boolean, `businessHours` or `calenderHours` 
    - conditions: [conditions](#conditions)
+ Response
    - [SLAPolicy](#SLApolicy)

### Update a SLA policy
`put api/v3/ticketing/SLAPolicies/{id}`
+ Parameters
    - id: string, SLA policy id 
    - isEnabled: boolean, if enabled this SLA policy 
    - firstRespondWithin: integer,  
    - nextRespondWithin: integer, 
    - resolveRespondWithin: integer,  
    - businessHours: string, `businessHours` or `calenderHours` 
    - conditions: [conditions](#conditions)
+ Response
    - [SLAPolicy](#SLApolicy)

### Delete a SLA policy
`delete api/v3/ticketing/SLAPolicies/{id}`
+ Parameters
    - id: string, SLA policy id
+ Response
    - http status code

### Upgrade/Downgrade a SLA policy
`put api/v3/ticketing/triggers/{id}/sort`
+ Parameters
    - id: string, SLA policy id
    - type: string, `up`, `down`
+ Response
    - http status code

# WorkingTime&Holiday
## objects
### workingTime
| Name | Type | Description | 
| - | - | - | 
| `dayOfWeek` | string | day of week |
| `isBusinessDay` | boolean |  |
| `startHour` | integer | business start hour |
| `startMin` | integer | business start min |
| `endHour` | integer | business close hour |
| `endMin` | integer | business close min |

### holiday
| Name | Type | Description |
| - | - | - | 
| `id` | string | the id of the holiday |
| `title` | string | the title of the holiday  |
| `date` | datetime | the date of the holiday |

## endpoints
### Get working time setting
`get api/v3/ticketing/workingTime`
+ Parameters
    - no parameters
+ Response
    - [workingTime](#workingTime) list

### Update working time settings
`put api/v3/ticketing/workingTime`
+ Parameters
    - workingTimes: [workingTime](#workingTime)[]
+ Response
    - http status code

### List all holidays
`get api/v3/ticketing/holidays`
+ Parameters
    - no parameters
+ Response
    - [holiday](#holiday) list

### Get a holiday
`get api/v3/ticketing/holidays/{id}`
+ Parameters
    - id: string
+ Response
    - [holiday](#holiday)

### Create a holiday
`post api/v3/ticketing/holidays`
+ Parameters
    - id: string
    - title: string, title of the holiday
    - date: datetime, date of the holiday
+ Response
    - [holiday](#holiday)

### Update a holiday
`put api/v3/ticketing/holidays/{id}`
+ Parameters
    - id: string
    - title: string, title of the holiday
    - date: datetime, date of the holiday
+ Response
    - [holiday](#holiday)

### Delete a holiday
`delete api/v3/ticketing/holidays`
+ Parameters
    - id: string
+ Response
    - http status code

# Fields&Mapping 
## objects 
### field 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | field id | 
| `type` | string | `text`, `textarea`, `email`, `url`, `date`, `integer`, `float`, `operator`, `radio`, `checkbox`, `dropdownList`, `checkboxList`, `link`, `department` |
| `name` | string | field name | 
| `isSystemField` | boolean | if is system field | 
| `isRequired` | boolean | value if is required | 
| `defaultValue` | string | field default value | 
| `helpText` | string | field help text | 
| `length` | integer | field value max length | 
| `options` | [field option](#fieldoption)[] | value option | 
| `chatFieldMapping` | string | chat field name |
| `offlineMessageFieldMapping` | string | offline message field name |

### fieldOption 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | option id | 
| `name` | string | option name | 
| `value` | string | field value | 
| `orderNum` | integer | option order | 

### fieldMapping
| Name | Type | Description | 
| - | - | - | 
| `fieldId` | string | field id | 
| `chatFieldMapping` | string | chat field name |
| `offlineMessageFieldMapping` | string | offline message field name |

## endpoints 
### List fields and their options 
`get api/v3/ticketing/fields` 
+ Parameters
    - isSystemField: boolean, if is system field, optional
    - usageType: string, `all`, `views`, `routingRules`, `SLA`, `triggers`， optional, default `all`
+ Response 
    - [field](#field) list 

### Get a field
`get api/v3/ticketing/fields/{id}`
+ Parameters
    - id: string, field id
+ Response
    - [field](#field) 

### Create a field
`post api/v3/ticketing/fields`
+ Parameters
    - type, string, `text`, `textarea`, `email`, `url`, `date`, `integer`, `float`, `operator`, `radio`, `checkbox`, `dropdownList`, `checkboxList`, `link`, `department`
    - name, string, field name 
    - isRequired, boolean, value if is required 
    - defaultValue, string, field default value 
    - helpText, string, field help text 
    - length, integer, field value max length 
    - options, [field option](#fieldoption)[], value option 
+ Response
    - [field](#field) 

### Update a field
`put api/v3/ticketing/fields/{id}`
+ Parameters
    - id, string, field id 
    - name, string, field name 
    - isRequired, boolean, value if is required 
    - defaultValue, string, field default value 
    - helpText, string, field help text 
    - length, integer, field value max length 
    - options, [field option](#fieldoption)[], value option 
+ Response
    - [field](#field) 

### Delete a field
`delete api/v3/ticketing/fields/{id}`
+ Parameters
    - id, string, field id 
+ Response
    - http status code

### Mapping fields
`put api/v3/ticketing/fields/mapping`
+ Parameters
    - fieldMappings: [fieldMapping](#fieldMapping) list
+ Response
    - http status code


# BlockedSenders 
## objects 
### blocked sender 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | the id of blocked sender |
| `value` | string | email or domain | 
| `blockType` | string | `blockEmailasJunk`, `rejectEmail`, `blockDomainasJunk`, `rejectDomain` | 

## endpoints 
### List blocked senders 
`get /api/v3/ticketing/blockedSenders` 
+ Parameters 
    - value: string, domain or email address 
+ Response 
    - [block sender](#blocked-sender) list 

### Get a blocked sender
`get /api/v3/ticketing/blockedSenders/{id}`
+ Parameters
    - id: string
+ Response
    - [block sender](#blocked-sender) 

### Add/update a blocked sender 
`put api/v3/ticketing/blockedSenders` 
+ Parameters 
    - `value`, string, domain or email address 
    - `blockType`, string, `blockEmailasJunk`, `rejectEmail`, `blockDomainasJunk`, `rejectDomain`
+ Response 
    - [block sender](#blocked-sender) 

### Remove a blocked sender 
`delete api/v3/ticketing/blockedSenders` 
+ Parameters 
   - value: string, domain or email address 
+ Response 
    - http status code

# Junks 
## objects 
### junk 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | id of junk | 
| `channelId` | string | channel id | 
| `channelAccountId`| string | channel account id | 
| `contactIdentityId`| string | id of contact identity |
| `originalMessageId` | string | original message id|
| `originalMessageUrl` | string | origial message link |
| `parentId` | string | parent id |
| `contents` | [content](#content)[] | contents |   
| `subject` | string | subject | 
| `cc` | string | cc email addresses |  
| `isRead`| boolean | | 
| `sentById`| string | id of contact or visitor | 
| `sentByType`| string | `contact` or `visitor` or `system` | 
| `sentTime` | datetime | the sent time of the junk message | 

## endpoints 
### List junk emails
`get api/v3/ticketing/junks` 

- Parameters 
    - keywords: string
    - pageIndex: integer
    - pageSize: integer, default 20, max value 50
    - timeFrom: DateTime
    - timeTo: DateTime
- Response 
    - junks: [junk](#junk) list 
    - total: integer
    - previousPage: string, next page uri, the first page return null
    - nextPage: string, the last page return null
    - currentPage: string

+ Includes

    | Includes | Description |
    | - | - |
    | sender | `get api/v3/ticketing/junks?include=sender` | 

### Get a junk email 
`get api/v3/ticketing/junks/{id}` 
- Parameters 
    - id: string, email id 
- Response 
    - [junk](#junk) 

### Update a junk 
`put api/v3/ticketing/junks/{id}` 
- Parameters 
    - id: string, email id 
    - isRead: boolean, 
- Response 
    - [junk](#junk) 

### Restore a junk email to a normal ticket 
`post api/v3/ticketing/junks/{id}/notJunk` 
- Parameters 
    - id: string, email id 
- Response 
    - [ticket](#ticket) 

### Delete a junk email 
`delete api/v3/ticketing/junks/{id}` 
- Parameters 
    - id: string, junk email id 
- Response 
    - http status code 

# Channel Accounts 

## objects 

### channelAccount 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | id | 
| `accountName` | string | account name |   
| `appId` | string | app id |
| `accountOriginalId` | string | channel account original id |
| `isEnabled` | bool | if is enabled |
| `screenName` | bool | screen name |
| `channelIds` | string[] | channel id array |
| `isEnabledBot` | bool | if is enabled bot |
| `selectedBotId` | string | selected bot id |
| `percentageOfNewTicketToBotWhenAgentsOnline` | integer | Percentage of new ticket to bot when agents are online |
| `percentageOfNewTicketToBotWhenAgentsOffline` | integer | Percentage of new ticket to bot when agents are offline |

## endpoints 
### List channel accounts 
`get api/v3/ticketing/channelAccounts` 
- Parameters
- Response 
    - [channelAccount](#channelAccount)[] 

### Get channel account by id
`get api/v3/ticketing/channelAccounts/{id}` 
- Parameters
    - id: string
- Response 
    - [channelAccount](#channelAccount) 

### Add an channel account 
`post api/v3/ticketing/channelAccounts` 
- Parameters
    - channelAccount: [channelAccount](#channelAccount) 
- Response 
    - [channelAccount](#channelAccount)

### Update an channel account 
`put api/v3/ticketing/channelAccounts/{id}` 
- Parameters
    - accountName: string 
- Response 
    - [channelAccount](#channelAccount)

### Enable/Disable bot for channel account
`put api/v3/ticketing/channelAccounts/{id}/bot/enable`
- Parameters
    - isEnabled: boolean 
- Response 
    - http status code

### Delete an channel account 
`delete api/v3/ticketing/channelAccounts/{id}` 
- Parameters
    - id: string,
- Response 
    - http status code


# Channels 
## objects 
### channel 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | id | 
| `appId` | string | channel app id | 
| `channelAccountIds` | string[] | channel account id array |
| `name` | string | channel name | 
| `contactIdentityTypeId` | string | contact identity type id |
| `contactIdentityType` | int | contact identity type name |
| `icon` | string | identity type icon url |     
| `messageDisplayType` | string | `treeView`, `flatView`, `emailView` |
| `outgoingMessageMaxLength` | int | outgoing message max length |
| `outgoingMessageCapability` | int | outgoing message support message type |
| `ifSupportDiffAccountReply` | bool | If support reply with different channel account |  
| `ifAllowActiveCreation` | bool | If allow active create message by agent |  
| `ifDisplaySubject` | bool | If display subject in agent console |  
| `ifDisplayContact` | bool | If display contact in agent console |  
| `ifDisplayCc` | bool | If display cc in agent console |  
| `ifDisplayToolbarOfEditor` | bool | If display toolbar of editor in agent console |  
| `ifDisplayAttachment` | bool | If display attachment |  
| `ifDisplayChannelAccount` | bool | If display channel account |  
| `ifEnableFullScreenReplay` | bool | If enable full screen when reply message |  
| `ifHasNote` | bool | If has note |  
| `ifEnableSaveAsDraft` | bool | If enable save draft |  
| `ifAllowAtFeature` | bool | If allow @ |  
| `ifAllowActiveReply` | bool | If allow active reply |  
| `ifSupportBot` | bool | If support bot |  

## endpoints 
### List all integrated channels 
`get api/v3/ticketing/channels` 
- Parameters
    - no parameter
- Response 
    - [channel](#channel)[] 
