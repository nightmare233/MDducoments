# Comm100 API
# Resource List 
|Name|EndPoint|Note| 
|---|---|---| 
|[Contact](#contact)| /api/v3/contacts|for contact| 
|[Agent](#agent)| /api/v3/agents|for agent|

# Contact
## objects
### contact
| Name | Type | Description |
| - | - | - |
| `id` | integer | id of the contact |
| `name` | string |  the name of the contact |
| `alias` | string |  the alias name of the contact |
| `identities` | [identity](#identity)[] | identity array  |
| `description` | string | a small description of the contact |
| `company` | string | the primary company name which this contact belongs to |
| `title` | string | the title of the contact|
| `phoneNumber` | string | telephone number of the contact|
| `faxNumber` | string | fax number of the contact |
| `address` | string | the address of the contact  |
| `city` | string | the city of the contact  |
| `stateOrProvince` | string | the state or province of the contact |
| `country` | string |  the country of the contact |
| `postcode` | string | the postcode of the contact  |
| `createdTime` | datetime | the time the contact was created |
  
### identity
| Name | Type | Description | 
| - | - | - | 
| `id` | integer | the id of identity |
| `type` | string | `email`, `SSOUserId`, `externalId` |
| `value` | string | the value of this identity, should be unique |

- Note: We currently only allow one for each type.

### endpoints
#### Get a contact by contact id
`get  /api/v3/contacts/{id}`
- Parameters
    - id: integer, id of the contact
- Response
    - [contact object](#contact)

#### Create a contact
`post  /api/v3/contacts`
- Parameters 

| Name | Type | Description |
| - | - | - |
| `name` | string |  the name of the contact, required |
| `alias` | string |  the alias name of the contact |
| `identities` | [identity](#identity)[] | the array of identities |
| `description` | string | a small description of the contact |
| `company` | string | the primary company name which this contact belongs to |
| `title` | string | the title of the contact |
| `phoneNumber` | string | telephone number of the contact|
| `faxNumber` | string | fax number of the contact |
| `address` | string | the address of the contact  |
| `city` | string | the city of the contact  |
| `stateOrProvince` | string | the state or province of the contact |
| `country` | string |  the country of the contact |
| `postcode` | string | the postcode of the contact  |

- Response
    - contact: [contact object](#contact)

#### Search contacts
- Max 50 contacts are responded for each request.
- `get  /api/v3/contacts`
- Parameters
    - pageIndex, integer, default 1
    - keywords, string, search scope includes: name/identity value/alias 
- Response
    - contacts: [contact object](#contact) list
    - total: int, total number of contacts.
    - previousPage: string, next page uri, the first page return null.
    - nextPage: string, the last page return null.
    - currentPage: string, current page uri.
- Example
    - `get  /api/v3/contacts?keywords=comm100`
- Note
    - Deleted contact will not be included in the results.
    - The query must be URL encoded.
    - Fuzzy search is supported.

#### Update a contact
`put  /api/v3/contacts/{id}`
- Parameters

| Name | Type | Description |
| - | - | - |
| `name` | string |  the name of the contact |
| `alias` | string |  the alias name of the contact |
| `description` | string | a small description of the contact |
| `company` | string | the primary company name which this contact belongs to|
| `title` | string | the title of the contact|
| `phoneNumber` | string | telephone number of the contact|
| `faxNumber` | string | fax number of the contact |
| `address` | string | the address of the contact  |
| `city` | string | the city of the contact  |
| `stateOrProvince` | string | the state or province of the contact |
| `country` | string |  the country of the contact |
| `postcode` | string | the postcode of the contact |

- Response
    - contact: [contact object](#contact)

#### Delete a contact
 `delete  /api/v3/contacts/{id}`
- Parameters
    - id: integer, id of the contact
- Response
    - http status code and message

#### Add identity
`post  /api/v3/contacts/{contactId}/identities`
- Parameters
    - contactId: integer, contact id
    - type: string, identity type
    - value: string, identity value
- Response
    - identity: [identity object](#identity)

#### Update identity
`put  /api/v3/contacts/{contactId}/identities/{id}`
- Parameters
    - contactId, integer, contact id
    - id, integer, contact identity id
    - value, string, the value of the identity
- Response
    - identity: [identity object](#identity)

#### Delete identity
 `delete  /api/v3/contacts/{contactId}/identities/{id}`
- Parameters
    - contactId, integer, contact id
    - id, integer, contact identity id
- Response
    - http status code and message

# Agent
[Reference document](https://www.comm100.com/doc/api/introduction.htm#/Account?id=agent-json-format)