# General

# Basic Data Structure

  ```javascript
  //ticket
  const ticket = {
    id: 1, //number
    subject: '', //string
    agentAssignee: 
    {
        id: 1,    // number
        email: '', //string
        displayName: '', //string 
        firstName: '', //string
        lastName: '', //string	 
        title: '', //string	 
        bio: '', //string	 
        mobilePhone: '', //string	
        isAdmin: false // boolean
    },
    departmentAssignee: 
    {
        id: 1, //number
        name: '' //string
    },
    contact: 1, //number
    {
        id: number, //number
        name: '', //string  // todo：
        identities:
        {
            id: 1, //number
            type: '', //string, email, SSOUserId, externalId
            value: '', //string, the value of this identity
        },
        description: '', //string
        company: '', //string
        title: '', //string
        phoneNumber: '', //string
        faxNumber: '', //string
        address: '', //string
        city: '', //string
        stateOrProvince: '', //string
        country: '', //string
        postcode: '', //string
        createTime: '' //string
    },
    channel: '', // string, Portal, Email,
    status: '', //string, new, pendingExternal, pendingInternal, onHold, closed
    priority: '', //string, low, normal, high, urgent
    isRead: false, //boolean
    isHaveDraft: true, //boolean
    receivingAccount: '', //string
    createdTime: '2018-12-08T12:03:07.563', //ISO 8601 time format
    closedTime: '2018-12-08T12:03:07.563', //ISO 8601 time format
    lastActivityTime: '2018-12-08T12:03:07.563', //ISO 8601 time format
    lastStatusChangeTime: '2018-12-08T12:03:07.563', //ISO 8601 time format
    firstRespondBreachAt: '2018-12-08T12:03:07.563', //ISO 8601 time format
    nextRespondBreachAt: '2018-12-08T12:03:07.563', //ISO 8601 time format
    resolveBreachAt: '2018-12-08T12:03:07.563', //ISO 8601 time format
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
            value: '' //string
        }
    ], 
    mentionedAgents:[
        {
            id: 1, //number,agent id
            messageId: 1, // message Id
            isRead: false, //boolean
        }
    ],
    attachments:[
        {
            id: 1, //number
            guid: "6a8f3e5f-4e31-4ee7-b074-13d0ea278eab", //string
            url: '', //string
            fileName:'', //string
            isAvailable: true
        }
    ]
  }
  
  //message
  const message = {
        id: 1, //number
        type: '', //string, note, email, reply
        source: '', //string, gentConsole, API, helpDesk, webForm, Chat, offlineMessage,
        htmlBody: '', //string, html text of the message
        plainBody: '', //string, plain text of the message
        quote: '', //string
        subject: '', //string
        from: '', //string
        to: '', //string
        cc: '', //string
        time: '', //string
        sender: 
        {
            id: 1, //number
            type:'', //string
            name: '', //string
            email: '', //string   
            avatarUrl: '', //string
        }
        attachments:[
            {
                id: 1, //number
                guid: "6a8f3e5f-4e31-4ee7-b074-13d0ea278eab", //string
                url: '', //string
                fileName:'', //string
                isAvailable: true //boolean
            }
        ]
    }

 // attachment
  const  attachment = {
    id: 1,    // number
    fileName: '', // string
    url: '',// string
    guid: '', // string
    isAvailable: true //boolean
  }
  
  // agent
  const  agent = {
    id: 1,    // number
    email: '', //string
    displayName: '', //string 
    firstName: '', //string
    lastName: '', //string	 
    title: '', //string	 
    bio: '', //string	 
    mobilePhone: '', //string	
    isAdmin: false // boolean
  }
```

# Objects
## Agent

  ```javascript
  /** @type {object(agent)} **/
    Comm100AgentConsoleAPI.get('agentconsole.currentAgent');
    Comm100AgentConsoleAPI.get('agentconsole.currentAgent.ticketStatus');   
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
//Example
Comm100AgentConsoleAPI.set('agentconsole.ticket.currentTicket.contact', { id: 1 });
```

### department assignee

```javascript
Comm100AgentConsoleAPI.get('agentconsole.ticket.currentTicket.departmentAssignee');
Comm100AgentConsoleAPI.set('agentconsole.ticket.currentTicket.departmentAssignee', value);
//Example
Comm100AgentConsoleAPI.set('agentconsole.ticket.currentTicket.departmentAssignee', { id: 1 });
```
### agent assignee

```javascript
Comm100AgentConsoleAPI.get('agentconsole.ticket.currentTicket.agentAssignee');
Comm100AgentConsoleAPI.set('agentconsole.ticket.currentTicket.agentAssignee', value);
//Example
Comm100AgentConsoleAPI.set('agentconsole.ticket.currentTicket.agentAssignee', {id: 1 });
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
Comm100AgentConsoleAPI.do('agentconsole.ticket.currentTicket.tags.add', value});
Comm100AgentConsoleAPI.do('agentconsole.ticket.currentTicket.tags.remove', value});
//Example
Comm100AgentConsoleAPI.do('agentconsole.ticket.currentTicket.tags.add', {id: 1});
Comm100AgentConsoleAPI.do('agentconsole.ticket.currentTicket.tags.remove', {id: 1});
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
Comm100AgentConsoleAPI.on('agentconsole.currentTicket.contactId.changed', function(event) {});
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
  