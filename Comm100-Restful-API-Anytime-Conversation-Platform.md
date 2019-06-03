## Note
- 这个API是共享平台提供给APP中心调用的，用于转发Message和回调消息发送的结果。

## Objects

### message 
| Name | Type | Description | 
| - | - | - | 
| `id` | string | id of message | 
| `conversationId` | integer | id of conversation | 
| `integrationAccountId`| string | integration account id | 
| `contactIdentityId`| string | id of contact identity |
| `source` | string | `agentConsole`, `helpDesk`, `webForm`, `API`, `chat`, `offlineMessage` | 
| `originalMessageId` | string | original message id|
| `originalMessageLink` | string | origial message link |
| `parentId` | string | parent id |
| `quoteTweetId` | string | quote tweet id |    
| `subject` | string | subject | 
| `cc` | string | cc email addresses |  
| `contents` | [content](#content)[] | content array| 
| `mentionedAgentIds` | integer[] | only for Note, @mentioned agents id array |
| `isRead`| boolean | | 
| `sendStatus` | string | `sucess`, `sending`, `failed` |
| `sendertId`| string | id of agent or contact | 
| `senderType`| string | `agent` or `contact` or `system` | 
| `time` | datetime | the sent time of the message | 
 
### content
| Name | Type | Description | 
| - | - | - | 
| `type` | string | content type, `text`, `htmlText`, `media`, `file`, `location` |  
| `data` | object | [text content](#text-content) or [html text content](#html-text-content) or [file message content](#file-message-content) or [media message content](#media-message-content) or [location message content](#location-message-content)| 

### text content
| Name | Type | Description | 
| - | - | - | 
| `id` | string | content unique id |
| `messageId` | string | message id |
| `type` | string | content type, `text`| 
| `text` | string | text | 

### html text content
| Name | Type | Description | 
| - | - | - | 
| `id` | string | content unique id |
| `messageId` | string | message id |
| `type` | string | content type, `htmlText`| 
| `htmlText` | string | html text |

### file message content
| Name | Type | Description | 
| - | - | - | 
| `id` | string | content unique id |
| `messageId` | string | message id |
| `type` | string | content type, `file`| 
| `name` | string | file name| 
| `url` | string | download link | 

### media message content
| Name | Type | Description |
| - | - | - |  
| `id` | string | content unique id |
| `messageId` | string | message id |
| `type` | string | content type, `media`| 
| `title` | string | media title| 
| `url` | string | download link | 

### location message content
| Name | Type | Description |  
| - | - | - | 
| `id` | string | content unique id |
| `messageId` | string | message id |
| `type` | string | content type, `location`| 
| `latitude` | string | latitude | 
| `longitude` | string | longitude | 
| `scale` | string | scale for location |
| `desc` | string | description | 
 
## EndPoints

### Post a message 
`post api/v3/anytime/platform/conversations/{id}/messages` 
- Parameters  
    - type: string, required, `email`, `reply`, `socialMessage`,
    - integrationAccountId: string, channel account id,
    - contactIdentityId: string, contact identity id,
    - originalId: string,
    - originalLink: string,
    - subject: string, for email message, email subject,
    - parentId: string, 
    - quoteTweetId: string,
    - cc: string, message cc emails, 
    - contents: [content](#content)[],
    - sendByType: string, `agent`, 
    - sendById: string, agent id,
- Response 
    - [message](#message) 

### Feedback result
`post api/v3/anytime/platform/conversations/messages/{id}`
- Parameters
    - sendStatus: string, `success`, `failed`
    - message: string
