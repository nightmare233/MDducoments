# 这个文档作废了， 挪到同一个文档中了。
# EndPoint List 

| 接口 | 接口类型 | 接口说明 |
| --- | --- | --- |
| [/api/v3/anytime/reports/realtime/now](#right-now) | GET | 获取当前的指标 |
| [/api/v3/anytime/reports/realtime/today](#realtime-today) | GET | 获取今天的指标 |
| [/api/v3/anytime/reports/realtime/departments](#realtime-department) | GET | 获取当前各个department数据 |
| [/api/v3/anytime/reports/realtime/agents](#realtime-agent) | GET | 获取当前各个agent的数据 |
| [/api/v3/anytime/reports/volume/export](#export-volume) | GET | 导出volume报表 |
| [/api/v3/anytime/reports/volume](#report-volume) | GET | 获取volume报表数据 |
| [/api/v3/anytime/reports/channel/export](#export-channel) | GET | 导出 channel报表 |
| [/api/v3/anytime/reports/channel](#report-channel) | GET | 获取 channel报表数据 |
| [/api/v3/anytime/reports/efficiency/export](#export-efficiency) | GET | 导出efficiency 报表 |
| [/api/v3/anytime/reports/efficiency](#report-efficiency) | GET | 导出efficiency 报表 |


###### 接口描述

###### right now
- 名称：/api/v3/anytime/reports/realtime/now
- 方式：GET
- Parameters：
	- siteId: number
- Return：
 	- unassignedConversations: number
	- openConversations: number
	- newConversations: number
	- pendingInternalConversations: number
	- pendingExternalConversations: number
	- onHoldConversations: number
	- urgentConversations: number
	- highConversations: number

###### realtime today
- `GET /api/v3/anytime/reports/realtime/today`
- Parameters:
	- siteId: number
- Return：
 	- createdConversations: number
	- closedConversations: number
	- repliedConversations: number
	- reopenedConverstations: number

###### realtime department
`GET /api/v3/anytime/reports/realtime/departments`
- Parameters：
	- siteId: number
- Return：
	- dataList:
		- departmentId: number,
		- openConversations: number,
		- todayRepliedConversations: number,
		- todayClosedConversations: number,
		- newConversations: number,
		- pendingInternalConversations: number,
		- pendingExternalConversations: number,
		- onHoldConversations: number,
		- urgentConversations: number,
		- highConversations: number,

###### realtime agent
`GET/api/v3/anytime/reports/realtime/agents`
- Parameters：
	- siteId: number,
	- filterType: string, `site`, `agent`, `department`, `account`, `channel`
	- filterValue: number,
- Return：
	- dataList: 
		- agentId: number,
		- openConversations: number,
		- todayRepliedConversations: number,
		- todayClosedConversations: number,
		- newConversations: number,
		- pendingInternalConversations: number,
		- pendingExternalConversations: number,
		- onHoldConversations: number,
		- urgentConversations: number,
		- highConversations: number,
	
###### export volume
`GET /api/v3/anytime/reports/volume/export`
- Parameters：
	- siteId: number,
    - startTime: string(utc 时间),
    - endTime: string(utc 时间),
    - filterType: string, `site`, `agent`, `department`, `account`, `channel`
	- filterValue: number,
    - timeUnit: string,
    - dimensionType: string,
    - timeOffset： number,
    - dateFormat: string,
- Return：csv file


###### report volume
`GET /api/v3/anytime/reports/volume`
- Parameters：
    - siteId: number,
    - startTime: string(utc 时间),
    - endTime: string(utc 时间),
    - filterType: string, `site`, `agent`, `department`, `account`, `channel`
	- filterValue: number,
    - timeUnit: string,
    - dimensionType: string,
- Return：
	- dataList:
	    - id: number,
    	- createdConversations: number,
    	- createdConversationsPercentage:  float,	
    	- closedConversations: number,
    	- closedConversationsPercentage: float,
    	- repliedConversations: number,
    	- repliedConversationsPercentage: float,
    	- reopenedConversations: number,
    	- reopenedConversationsPercentage: float,
    	- openConversations: number,
    	- openConversationsPercentage: float,
    	- startTime: string,
    	- endTime: string,
	- totalCreatedConversations: number,
	- totalClosedConversations: number,
	- totalRepliedConversations: number,
	- totalReopenedConversations: number,
	- totalOpenConversations: number

###### export channel
`GET /api/v3/anytime/reports/channel/export`
- Parameters：
	- siteId: number,
    - startTime: string(utc 时间),
    - endTime: string(utc 时间),
    - filterType: string, `site`, `agent`, `department`, `account`, `channel`
	- filterValue: number,
    - timeUnit: string,
    - dimensionType: string,
    - timeOffset： number,
    - dateFormat: string,
- Return：csv file

###### report-channel
`GET /api/v3/anytime/reports/channel`
- Parameters：
	- siteId: number,
    - startTime: string(utc 时间),
    - endTime: string(utc 时间),
    - filterType: string, `site`, `agent`, `department`, `account`, `channel`
	- filterValue: number,
    - timeUnit: string,
    - dimensionType: string,
- Return：
	- dataList: 
	    - id: number,
		- name: string,
		- channelEfficiencies, [channelEfficiencie](#channel-efficiency)[]
		- startTime: string,
		- endTime: string,
	- channel total number, [channel total number](#Channel-totoal-messages-number)[]

##### Channel Efficiency
| Name | Type | Description | 
| - | - | - | 
| `channelId` | string | channel id |
| `channelName` | number | channel name |
| `channelPercentage` | float |  |

##### Channel totoal messages number
| Name | Type | Description | 
| - | - | - | 
| `channelId` | string | channel id |
| `channelName` | number | channel name |
| `totalNumber` | number |  |

###### export efficiency
`GET /api/v3/anytime/reports/efficiency/export`
- Parameters：
	- siteId: number,
    - startTime: string(utc 时间),
    - endTime: string(utc 时间),
    - filterType: string, `site`, `agent`, `department`, `account`, `channel`
	- filterValue: number,
    - timeUnit: string,
    - dimensionType: string,
    - timeOffset： number,
    - dateFormat: string,
- Return：csv file

###### report efficiency
`GET /api/v3/anytime/reports/efficiency`
- Parameters：
	- siteId: number,
    - startTime: string(utc 时间),
    - endTime: string(utc 时间),
    - filterType: string, `site`, `agent`, `department`, `account`, `channel`
	- filterValue: number,
    - timeUnit: string,
    - dimensionType: string,
- Return：
	- dataList:
	    - id: number,
		- avgAgentResponseTime: string,
		- avgFirstResponseTime: string,
		- avgConversationTime: string,
		- avgContactMessages: float,
		- avgAgentMessages: float, 
		- startTime: string,
		- endTime: string,
	- totalAvgAgentResponseTime: string,
	- totalAvgFirstResponseTime: string,
	- totalAvgConversationTime: string,
	- totalAvgContactMessages: float,
	- totalAvgAgentMessages: float,
