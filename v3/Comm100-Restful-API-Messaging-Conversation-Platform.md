## Note 
- 这里的API是共享平台提供给APP中心调用的，用于转发第三方APP过来的外部的Message，回调更新消息发送结果，以及更新缓层，不需要开放给客户。

## Objects

 ### content
| Name | Type | Description | 
| - | - | - | 
| `id` | string | guid | 
| `type` | string | content type, `text`, `htmlText`, `video`,`audio`, `picture`, `file`, `location` |  
| `text` | string | text | 
| `htmlText` | string | html text |
| `name` | string | file name| 
| `url` | string | download link | 
| `title` | string | media title| 
| `latitude` | string | latitude | 
| `longitude` | string | longitude | 
| `scale` | string | scale for location |
| `desc` | string | description | 

## EndPoints

### Post a message 
`post api/v3/messaging/platform/conversations/messages` 
- Parameters  
    - channelAccountId: string, channel account id,
    - isReceive: bool, if the message sent from contact
    - createTime: datetime
    - channelAppContactIdentity: 
        - channelaccount: string, 
        - name: string,
        - avatarUrl: string,
        - originalContactInfoUrl: string
    - message
        - channelId： string, channel Id, required,
        - originalParentId: string, 
        - originalConversationId: string
        - originalMessageId: string,
        - originalUrl: string,
        - subject: string, for email message, email subject,
        - cc: string, message cc emails, 
        - contents: [content](#content)[],
- Response 
    - guid: string, message guid

### Callback result
`put api/v3/messaging/platform/conversations/messages/{id}`
- Parameters
    - sendStatus: string, `success`, `failed`
    - errorMessage: string, error message
    - originalMessageId: string, 
    - originalMessageUrl: string,
    - content[]: 
        - id: string
        - url: string 
- Response 
    - httpStatusCode

### Update Cache
`put api/v3/messaging/platform/caches`
- Parameters
    - cacheItem: `channel`, `channelAccount`, `version`, `app`, `versionChannel`
    - actionType: `updated`
- Response
    - httpStatusCode

### Notify the contact is modified
`post api/v3/messaging/platform/contact/{id}`
- No Parameters
- Response
    - httpStatusCode