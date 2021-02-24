# Basic Data Structure

## Ticket

```javascript
const ticket = {
  id: 1, // number
  guid: "00000000-0000-0000-0000-000000000000", // string
  relatedType: "contact", // string: contact, visitor, agent
  relatedId: 1, // number
  subject: "", // string
  lastMessageId: 1, // number
  lastMessageChannelId: "00000000-0000-0000-0000-000000000000", // string
  lastMessageChannelAccountId: "00000000-0000-0000-0000-000000000000", // string
  firstMessageId: 1, // number
  firstMessageChannelId: "00000000-0000-0000-0000-000000000000", // string
  firstMessageChannelAccountId: null, // string
  originalId: "", // string
  status: "pendingExternal", // string: new, pendingInternal, pendingExternal, onHold, resolved
  priority: "normal", // string: urgent, high, normal, low
  assignedType: "agent", // string: agent, bot
  assignedBotId: null, // string
  assignedAgentId: 1, // number
  assignedDepartmentId: null, // string
  lastStatusChangedAt: "2020-10-26T08:23:46.073Z", // ISO 8601 time format
  lastUpdatedAt: "2020-10-26T09:53:34.573Z", // ISO 8601 time format
  isEditable: true, // boolean
  isDeleted: false, // boolean
  isRead: true, // boolean
  isReadByContact: false, // boolean
  hasDraft: false, // boolean
  isActive: false, // boolean
  isMultiChannel: true, // boolean
  isHidden: false, // boolean
  createdById: 1, // number
  createdByType: "agent", // string: agent, contact, system, visitor
  createdAt: "2020-10-26T08:22:04.670Z", // ISO 8601 time format
  lastRepliedAt: "2020-10-26T08:23:46.073Z", // ISO 8601 time format
  lastRepliedById: 16, // number
  lastRepliedByType: "agent", // string: agent, contact, system
  tagIds: ["00000000-0000-0000-0000-000000000000"],
  customFields: [
    {
      name: "", // string
      id: "00000000-0000-0000-0000-000000000000", // string
      value: "", // string
    },
  ],
  mentionedAgents: [
    {
      agentId: 0, // number
      isRead: false, // boolean
      messageId: 0, // number
    },
  ],
  slaPolicyId: "00000000-0000-0000-0000-000000000000", // string
  firstRespondBreachAt: "0001-01-01T00:00:00.000Z", // ISO 8601 time format
  nextRespondBreachAt: "0001-01-01T00:00:00.000Z", // ISO 8601 time format
  resolveBreachAt: "2020-10-26T11:22:04.670Z", // ISO 8601 time format
  nextSLABreachAt: "2020-10-26T11:22:04.670Z", // ISO 8601 time format
  totalReplies: 2, // number
  contactOrVisitor: {
    id: 0, // number
    name: "", // string
    alias: "", // string
    contactIdentities: [
      {
        id: 1, // number
        contactId: 1, // number
        type: "email", // string
        value: "", // string
        name: "", // string
        avatarURL: "", // string
        infoURL: "", // string
        identityIconUrl: "", // string
        originalContactPageUrl: "", // string
        screenName: "", // string
        orderNum: 1, // number
        isDeleted: false, // boolean
      },
    ],
    description: "", // string
    company: "", // string
    title: "", // string
    phone: "", // string
    fax: "", // string
    mailingAddress: "", // string
    city: "", // string
    stateOrProvince: "", // string
    countryOrRegion: "", // string
    postalOrZipCode: "", // string
    createdTime: "2020-07-22T04:47:28.620Z", // ISO 8601 time format
    isDeleted: false, // boolean
    tags: [],
  },
};
```

## Message

```javascript
const message = {
  id: 1, // number
  conversationId: 1, // number
  channelId: "00000000-0000-0000-0000-000000000000", // string
  type: "message", // string: message, note
  directType: "receive", // string: receive, send
  channelAccountId: null, // string
  contactIdentityId: 484, // number
  originalMessageId: "00000000-0000-0000-0000-000000000000", // string
  originalMessageUrl: "", // string
  parentId: 1, // number
  subject: "", // string
  cc: "", // string
  bcc: "", // string
  quote: null, // string
  contents: [
    {
      id: 1, // number
      type: "htmlText", // string: text, htmlText, video, audio, picture, file, location, webView, quickReply
      text: "", // string
      htmlText: "", // string
      name: "", // string
      url: "", // string
      previewUrl: "", // string
      mime: "", // string
      attachmentId: "", // string
      latitude: "", // string
      longitude: "", // string
      desc: "", // string
      title: "", // string
      fallbackUrl: "", // string
      webviewHeight: "", // string
      payload: "", // string
      orderNum: 0, // number
    },
  ],
  mentionedAgentIds: [],
  isRead: false, // boolean
  sendStatus: "success", // string: success, sending, failed
  sender: {
    id: 0, // number
    name: "", // string
    alias: "", // string
    contactIdentities: [
      {
        id: 1, // number
        contactId: 1, // number
        type: "email",
        value: "", // string
        name: "", // string
        avatarURL: "", // string
        infoURL: "", // string
        identityIconUrl: "", // string
        originalContactPageUrl: "", // string
        screenName: "", // string
        orderNum: 1, // number
        isDeleted: false, // boolean
      },
    ],
    description: "", // string
    company: "", // string
    title: "", // string
    phone: "", // string
    fax: "", // string
    mailingAddress: "", // string
    city: "", // string
    stateOrProvince: "", // string
    countryOrRegion: "", // string
    postalOrZipCode: "", // string
    createdTime: "2020-07-22T04:47:28.620Z", // ISO 8601 time format
    isDeleted: false, // boolean
    tags: [],
  },
  sentById: 337, // number
  sentByBotId: null, // string
  sentByType: "contact", // string: agent, contact, system, bot
  sentTime: "2020-10-26T08:20:03.427Z", // ISO 8601 time format
  receivedTime: "2020-10-26T08:20:03.443Z", // ISO 8601 time format
  contact: {
    id: 0, // number
    name: "", // string
    alias: "", // string
    contactIdentities: [
      {
        id: 484, // number
        contactId: 337, // number
        type: "email",
        value: "", // string
        name: "", // string
        avatarURL: "", // string
        infoURL: "", // string
        identityIconUrl: "", // string
        originalContactPageUrl: "", // string
        screenName: "", // string
        orderNum: 1, // number
        isDeleted: false, // boolean
      },
    ],
    description: "", // string
    company: "", // string
    title: "", // string
    phone: "", // string
    fax: "", // string
    mailingAddress: "", // string
    city: "", // string
    stateOrProvince: "", // string
    countryOrRegion: "", // string
    postalOrZipCode: "", // string
    createdTime: "2020-07-22T04:47:28.620Z", // ISO 8601 time format
    isDeleted: false, // boolean
    tags: [],
  },
  errorMessage: "", // string
  hasAttachmentButNotReceived: false, // boolean
};
```

## Agent

```javascript
const agent = {
  id: 1, // number
  email: "", // string
  displayName: "", // string
  firstName: "", // string
  lastName: "", // string
  isAdmin: true, // boolean
  isActive: true, // boolean
  phone: "", // string
  title: "", // string
  bio: "", // string
  timeZone: "GMT Standard Time", // string
  datetimeFormat: "MM/dd/yyyy HH:mm:ss", // string
  createdTime: "2020-04-21T06:01:32.7", // ISO 8601 time format
  isLocked: false, // boolean
  lockedTime: "1900-01-01T00:00:00", // ISO 8601 time format
  lastLoginTime: "2021-02-23T06:18:03.46", // ISO 8601 time format
  roleIds: [],
  departmentIds: [],
  permissionIds: [],
  shiftIds: [],
};
```

# Get and set objects

## Agent

### Get the information of current agent

```javascript
/** @type {object(agent)} **/
Comm100AgentConsoleAPI.get("agentconsole.currentAgent");
```

## Ticket

### Get the information of current ticket

```javascript
/** @type {object(ticket)} **/
Comm100AgentConsoleAPI.get("agentconsole.ticketing.currentTicket");
```

### Get and set the `subject` of current ticket

```javascript
Comm100AgentConsoleAPI.get("agentconsole.ticketing.currentTicket.subject");
Comm100AgentConsoleAPI.set(
  "agentconsole.ticketing.currentTicket.subject",
  value
);
```

### Get and set the `contact` of current ticket

```javascript
Comm100AgentConsoleAPI.get("agentconsole.ticketing.currentTicket.contact");
//Only the ID of the contact can be set
Comm100AgentConsoleAPI.set(
  "agentconsole.ticketing.currentTicket.contact",
  value
);
//Example
Comm100AgentConsoleAPI.set("agentconsole.ticketing.currentTicket.contact", {
  id: 1,
});
```

### Get and set the `department assignee` of current ticket

```javascript
Comm100AgentConsoleAPI.get(
  "agentconsole.ticketing.currentTicket.departmentAssignee"
);
//Only the ID of the department can be set
Comm100AgentConsoleAPI.set(
  "agentconsole.ticketing.currentTicket.departmentAssignee",
  value
);
//Example
Comm100AgentConsoleAPI.set(
  "agentconsole.ticketing.currentTicket.departmentAssignee",
  { id: 1 }
);
```

### Get and set the `agent assignee` of current ticket

```javascript
Comm100AgentConsoleAPI.get(
  "agentconsole.ticketing.currentTicket.agentAssignee"
);
//Only the ID of the agent can be set
Comm100AgentConsoleAPI.set(
  "agentconsole.ticketing.currentTicket.agentAssignee",
  value
);
//Example
Comm100AgentConsoleAPI.set(
  "agentconsole.ticketing.currentTicket.agentAssignee",
  {
    id: 1,
  }
);
```

### Get and set the `priority` of current ticket

```javascript
Comm100AgentConsoleAPI.get("agentconsole.ticketing.currentTicket.priority");
// Only one of urgent, high, normal, low can be set
Comm100AgentConsoleAPI.set(
  "agentconsole.ticketing.currentTicket.priority",
  value
);
```

### Get and set the `status` of current ticket

```javascript
Comm100AgentConsoleAPI.get("agentconsole.ticketing.currentTicket.status");
// Only one of new, pendingInternal, pendingExternal, onHold, resolved can be set
Comm100AgentConsoleAPI.set(
  "agentconsole.ticketing.currentTicket.status",
  value
);
```

### Get, add and remove the `tags` of current ticket

```javascript
Comm100AgentConsoleAPI.get("agentconsole.ticketing.currentTicket.tags");
Comm100AgentConsoleAPI.do(
  "agentconsole.ticketing.currentTicket.tags.add",
  value
);
Comm100AgentConsoleAPI.do(
  "agentconsole.ticketing.currentTicket.tags.remove",
  value
);
//Example
Comm100AgentConsoleAPI.do("agentconsole.ticketing.currentTicket.tags.add", {
  id: 1,
});
Comm100AgentConsoleAPI.do("agentconsole.ticketing.currentTicket.tags.remove", {
  id: 1,
});
```

### Get and set the `custom fields` of current ticket by field id

```javascript
Comm100AgentConsoleAPI.get(
  "agentconsole.ticketing.currentTicket.customFields:[field id]"
);
Comm100AgentConsoleAPI.set(
  "agentconsole.ticketing.currentTicket.customFields:[field id]",
  value
);
//Example
Comm100AgentConsoleAPI.set(
  "agentconsole.ticketing.currentTicket.customFields:123",
  "test"
);
```

# Event

### Add event listener for ticket property change

```javascript
const propertyChangedEvent = {
  data: {
    oldData,
    newData,
  },
};

Comm100AgentConsoleAPI.on(
  "agentconsole.currentTicket.selectChanged",
  function (event) {}
);
Comm100AgentConsoleAPI.on(
  "agentconsole.currentTicket.subject.changed",
  function (event) {}
);
Comm100AgentConsoleAPI.on(
  "agentconsole.currentTicket.contact.changed",
  function (event) {}
);
Comm100AgentConsoleAPI.on(
  "agentconsole.currentTicket.departmentAssignee.changed",
  function (event) {}
);
Comm100AgentConsoleAPI.on(
  "agentconsole.currentTicket.agentAssignee.changed",
  function (event) {}
);
Comm100AgentConsoleAPI.on(
  "agentconsole.currentTicket.priority.changed",
  function (event) {}
);
Comm100AgentConsoleAPI.on(
  "agentconsole.currentTicket.status.changed",
  function (event) {}
);
Comm100AgentConsoleAPI.on(
  "agentconsole.currentTicket.tags.changed",
  function (event) {}
);
Comm100AgentConsoleAPI.on(
  "agentconsole.currentTicket.customFields:[field id].changed",
  function (event) {}
);
```

# Action

### Append text to the input box of current ticket

```javascript
Comm100AgentConsoleAPI.do(
  "agentconsole.ticketing.currentTicket.reply.append",
  value
);
Comm100AgentConsoleAPI.do(
  "agentconsole.ticketing.currentTicket.note.append",
  value
);
```
