# Social Report API
###### 1. 概述
- comm100 livechat 使用social api是通过cookie(LC_ASP.NET_SessionId)验证, 外面使用通过token方式验证.

###### 2. 枚举
 - enumDimension
    - ByTime
    - ByDepartment
    - ByAgent
    - BySocialAccount
    - BySource

 - enumFilter
    - Site
    - Department
    - Agent
    - SocialAccount
    - Source

- enumUnit
    - Daily
    - Week
    - Month

###### 3. 接口列表

| 接口 | 接口类型 | 接口说明 |
| --- | --- | --- |
| [/api/v1/social/reports/realtime/now](#right-now) | GET | 获取当前的指标 |
| [/api/v1/social/reports/realtime/today](#realtime-today) | GET | 获取今天的指标 |
| [/api/v1/social/reports/realtime/departments](#realtime-department) | GET | 获取当前各个department数据 |
| [/api/v1/social/reports/realtime/agents](#realtime-agent) | GET | 获取当前各个agent的数据 |
| [/api/v1/social/accounts](#accounts) | GET | 获取集成账号或者page的信息 |
| [/api/v1/social/reports/volume/export](#export-volume) | GET | 导出volume报表 |
| [/api/v1/social/reports/volume](#report-volume) | GET | 获取volume报表数据 |
| [/api/v1/social/reports/source/export](#export-source) | GET | 导出social source报表 |
| [/api/v1/social/reports/source](#report-source) | GET | 获取social source报表数据 |
| [/api/v1/social/reports/efficiency/export](#export-efficiency) | GET | 导出social efficiency 报表 |
| [/api/v1/social/reports/efficiency](#report-efficiency) | GET | 导出social efficiency 报表 |


###### 接口描述

###### right now
- 名称：/api/v1/social/reports/realtime/now
- 方式：GET
- 参数：
	- siteId: number
- 返回：
 	- unassignedConversations: number
	- openConversations: number
	- newConversations: number
	- pendingInternalConversations: number
	- pendingExternalConversations: number
	- onHoldConversations: number
	- urgentConversations: number
	- highConversations: number

###### realtime today
- 名称：/api/v1/social/reports/realtime/today
- 方式：GET
- 参数:
	- siteId: number
- 返回：
 	- createdConversations: number
	- closedConversations: number
	- repliedConversations: number
	- reopenedConverstations: number

###### realtime department
- 名称：/api/v1/social/reports/realtime/departments
- 方式：GET
- 参数：
	- siteId: number
- 返回：
	- dataList(列表):
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
- 名称：/api/v1/social/reports/realtime/agents
- 方式：GET
- 参数：
	- siteId: number,
	- filterType: string(枚举),
	- filterValue: number,
- 返回：
	- dataList(列表): 
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
	
###### accounts
- 名称：/api/v1/social/accounts
- 方式：GET
- 参数：
	- siteId: number,
- 返回：(数组）
	- id: number,
	- name: string,
	- screenName: string,
	- avatar: string,
	- source: string,
	- ifEnable: boolean,

###### export volume
- 名称：/api/v1/social/reports/volume/export
- 方式：GET 
- 参数：
	- siteId: number,
    - startTime: string(utc 时间),
    - endTime: string(utc 时间),
    - filterType: string(枚举),
	- filterValue: number,
    - timeUnit: string,
    - dimensionType: string,
    - timeOffset： number,
    - dateFormat: string,
- 返回：.csv文件（直接生成）


###### report volume
- 名称：/api/v1/social/reports/volume
- 方式：GET 
- 参数：
    - siteId: number,
    - startTime: string(utc 时间),
    - endTime: string(utc 时间),
    - filterType: string(枚举),
	- filterValue: number,
    - timeUnit: string,
    - dimensionType: string,
- 返回：
	- dataList(列表):
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

###### export source
- 名称：/api/v1/social/reports/source/export
- 方式：GET 
- 参数：
	- siteId: number,
    - startTime: string(utc 时间),
    - endTime: string(utc 时间),
    - filterType: string(枚举),
	- filterValue: number,
    - timeUnit: string,
    - dimensionType: string,
    - timeOffset： number,
    - dateFormat: string,
- 返回：.csv文件（直接生成）

###### report-source
- 名称：/api/v1/social/reports/source
- 方式：GET 
- 参数：
	- siteId: number,
    - startTime: string(utc 时间),
    - endTime: string(utc 时间),
    - filterType: string(枚举),
	- filterValue: number,
    - timeUnit: string,
    - dimensionType: string,
- 返回：
	- dataList（列表）: 
	    - id: number,
		- facebookWallPost: number,
		- facebookWallPostPercentage:  float,	
		- facebookVisitorPost: number,
		- facebookVisitorPostPercentage: float,
		- facebookMessage: number,
		- facebookMessagePercentage: float,
		- facebook: number,
		- facebookPercentage: float,
		- twitterTweet: number,
		- twitterTweetPercentage: float,
		- twitterDirectMessage: number,
		- twitterDirectMessagePercentage: float,
		- twitter: number,
		- twitterPercentage: float,
		- startTime: string,
		- endTime: string,
	- totalFacebookWallPost: number,
	- totalFacebookVisitorPost: number,
	- totalFacebookMessage: number,
	- totalFacebook: number,
	- totalTwitterTweet: number,
	- totalTwitterDirectMessage: number,
	- totalTwitter: number,

###### export efficiency
- 名称：/api/v1/social/reports/efficiency/export
- 方式：GET 
- 参数：
	- siteId: number,
    - startTime: string(utc 时间),
    - endTime: string(utc 时间),
    - filterType: string(枚举),
	- filterValue: number,
    - timeUnit: string,
    - dimensionType: string,
    - timeOffset： number,
    - dateFormat: string,
- 返回：.csv文件（直接生成）

###### report efficiency
- 名称：/api/v1/social/reports/efficiency
- 方式：GET 
- 参数：
	- siteId: number,
    - startTime: string(utc 时间),
    - endTime: string(utc 时间),
    - filterType: string(枚举),
	- filterValue: number,
    - timeUnit: string,
    - dimensionType: string,
- 返回：
	- dataList(列表):
	    - id: number,
		- avgAgentResponseTime: string,
		- avgFirstResponseTime: string,
		- avgConversationTime: string,
		- avgSocialUserMessages: float,
		- avgAgentMessages: float, 
		- startTime: string,
		- endTime: string,
	- totalAvgAgentResponseTime: string,
	- totalAvgFirstResponseTime: string,
	- totalAvgConversationTime: string,
	- totalAvgSocialUserMessages: float,
	- totalAvgAgentMessages: float,
