[toc]
# General

# Basic Data Structure

  ```javascript
  //ticket
  const ticket = {
    id: 1, //number
    subject: '', //string
    agentAssignee: 1, //number
    departmentAssignee: 1, //number
    contactId: 1, //number
    channel: '', // string, Internal, Portal, Email, Facebook Messenger, Facebook Visitor Post, Facebook Wall Post, Tweet, Twitter Direct Message, SMS, WeChat Message
    status: '', //string, new, pendingExternal, pendingInternal, onHold, closed
    priority: '', //string, low, normal, high, urgent
    isRead: false, //boolean
    receivingAccount: '', //string
    createdBy: 1, //number
    createdTime: '2018-12-08T12:03:07.563', //ISO 8601 time format
    closedTime: '2018-12-08T12:03:07.563', //ISO 8601 time format
    lastActivityTime: '2018-12-08T12:03:07.563', //ISO 8601 time format
    lastStatusChangeTime: '2018-12-08T12:03:07.563', //ISO 8601 time format
    totalReplies: 3, //number
    tags: [
        {
            id: 1, //number
            name: '', //string
        }
    ],
    customFields:[
        {
            fieldId: 1, //number
            fieldName:'', //string
            value: '', //string
        }
    ],
    mentionedAgents[
        {
            id: 1, //number,agent id
            noteId: 1, // note Id
            isRead: false, //boolean
        }
    ],
    attachments:[
        {
            id: 1, //number
            guid: "6a8f3e5f-4e31-4ee7-b074-13d0ea278eab", //string
            url: '', //string
            fileName:'', //string
        }
    ]
  }
  
  //message
  const message = {
        id: 1, //number
        ticketId: 1, //number
        source: '', //string, Agent Console, API, Help Desk, Web Form, Chat, Offline Message, Facebook Messenger, Facebook Visitor Post, Facebook Wall Post, Tweet, Twitter Direct Message, SMS, WeChat Message
        originalId: '', //string
        content: '', //string, html text of the message
        contentText: '', //string, plain text of the message
        quote: '', //string
        subject: '', //string
        from: '', //string
        to: '', //string
        cc: '', //string
        sentTime: ''//string
        senderId:1, //string
        senderType:'',//string
        senderName: '', //string
        attachments:[
            {
                id: 1, //number
                guid: "6a8f3e5f-4e31-4ee7-b074-13d0ea278eab", //string
                url: '', //string
                fileName:'', //string
            }
        ]
    }

 // attachment
  const  attachment = {
    id: 1,    // number
    fileName: '', // string
    url: '',// string
    guid: '', // string
  }
  
  // agent
  const  agent = {
    id: 1,    // number
    name: '', // string
    email: '',// string
    status: '', // string, online/away
    ticketStatus: '', //string, available, unavailable
    chats: 3,   // number, ongoing chats
    isAdmin: false, // boolean
  }
  
  //contact
  const contact = {
    id: number, //number
    name: '', //string 
    email: '', //string
    phoneNumber: '', //string
    ssoUserId: '', //string
    externalId: '', //string
  }

```

# Objects
## Agent

  ```javascript
  /** @type {object(agent)} **/
  Comm100AgentConsoleAPI.get('agentconsole.currentAgent');
  ```

## Ticket
### get the information of current ticket

```javascript
/** @type {object(ticket)} **/
Comm100AgentConsoleAPI.get('agentconsole.ticket.currentTicket');
```

### subject
   
```javascript
Comm100AgentConsoleAPI.get('agentconsole.ticket.currentTicket.subject');
Comm100AgentConsoleAPI.set('agentconsole.ticket.currentTicket.subject', value);
```
### contact

```javascript
Comm100AgentConsoleAPI.get('agentconsole.ticket.currentTicket.contact');
//Only the ID of the contact can be set
Comm100AgentConsoleAPI.set('agentconsole.ticket.currentTicket.contact', value);
//Sample
Comm100AgentConsoleAPI.set('agentconsole.ticket.currentTicket.contact', 
{
    id: number, //required
    name: '', //string, optional 
    email: '', //string, optional
    phoneNumber: ''//string, optional
    ssoUserId: '', //string, optional
}});
```

### department assignee

```javascript
Comm100AgentConsoleAPI.get('agentconsole.ticket.currentTicket.departmentAssignee');
Comm100AgentConsoleAPI.set('agentconsole.ticket.currentTicket.departmentAssignee', value);
```
### agent assignee

```javascript
Comm100AgentConsoleAPI.get('agentconsole.ticket.currentTicket.agentAssignee');
Comm100AgentConsoleAPI.set('agentconsole.ticket.currentTicket.agentAssignee', value);
```

### priority

```javascript
Comm100AgentConsoleAPI.get('agentconsole.ticket.currentTicket.priority');
Comm100AgentConsoleAPI.set('agentconsole.ticket.currentTicket.priority', value);
```

### status

```javascript
Comm100AgentConsoleAPI.get('agentconsole.ticket.currentTicket.status');
Comm100AgentConsoleAPI.set('agentconsole.ticket.currentTicket.status', value);
```
### tags

```javascript
Comm100AgentConsoleAPI.get('agentconsole.ticket.currentTicket.tags');
Comm100AgentConsoleAPI.set('agentconsole.ticket.currentTicket.tags', value);
```

### custom fields

```javascript
Comm100AgentConsoleAPI.get('agentconsole.ticket.currentTicket.customFields:[field id]');
Comm100AgentConsoleAPI.set('agentconsole.ticket.currentTicket.customFields:[field id]', value);
//sample
Comm100AgentConsoleAPI.set('agentconsole.ticket.currentTicket.customFields:123', 'test');
```

# Event
## Change Events
- this event is triggered when ticket property value has changed.

```javascript
    const propertyChangedEvent = {
        data:{
            oldData,
            newData
        }
    }

Comm100AgentConsoleAPI.on('agentconsole.currentTicket.subject.changed', function(event) {});
Comm100AgentConsoleAPI.on('agentconsole.currentTicket.contact.changed', function(event) {});
Comm100AgentConsoleAPI.on('agentconsole.currentTicket.departmentAssignee.changed', function(event) {});
Comm100AgentConsoleAPI.on('agentconsole.currentTicket.agentAssignee.changed', function(event) {});
Comm100AgentConsoleAPI.on('agentconsole.currentTicket.priority.changed', function(event) {});
Comm100AgentConsoleAPI.on('agentconsole.currentTicket.status.changed', function(event) {});
Comm100AgentConsoleAPI.on('agentconsole.currentTicket.tags.changed', function(event) {});
Comm100AgentConsoleAPI.on('agentconsole.currentTicket.customFields:[field id].changed', function(event) {});
```

# Action
- Append text to the input box of current ticket

```javascript
Comm100AgentConsoleAPI.do('agentconsole.ticket.currentTicket.reply.append', value)
Comm100AgentConsoleAPI.do('agentconsole.ticket.currentTicket.note.append', value)
```
  