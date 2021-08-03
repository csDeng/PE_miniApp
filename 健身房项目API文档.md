
**目录**

1\. 注册
2\. 登录
3\. 预约
4\. 取消预约
5\. 历史预约
6\. 我的相关信息
7\. 管理员首页
8\. 管理员设置是否可预约页面
---

**1\. 注册**
###### 接口功能
> 把用户信息写进数据库

###### URL
> [服务器地址]()

> /HomePage/Register

###### 支持格式
> JSON

###### HTTP请求方式
> POST

###### 请求参数
|参数|必选|类型|说明|
|:---  |:-----|:---|---|
|name   |true|  string | 用户的真实姓名|
|phone    |ture    |string|用户的手机账号                         |
|password|true|string|用户即将要设置的密码|

###### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|flag   |bool    |返回结果状态。true：正常；false：错误。 |
|errInfo|string|错误提示信息|


###### 接口示例
> 地址：[http://www.api.com/index.php/register?name="可口可乐"&phone="19129375556"&num="201902010xxx"&password="xxx"]()

> 响应结果
``` json

{
    // "flag": true,
    // "errInfo":"",

    "flag":false,
    "errInfo":"学号已被注册"
}

```
---

**2\.登录**
###### 接口功能
> 请求用户在数据库里的信息以及首页所需的相关参数

###### URL
> [服务器地址]()

> /HomePage/Login

###### 支持格式
> JSON

###### HTTP请求方式
> POST

###### 请求参数
|参数|必选|类型|说明|
|:---  |:-----|:---|---|
|name|true|string|用户名|
|pwd|true|string|用户的密码|

###### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|BreakPromise   |bool    |返回结果状态。<br />`true：用户因违反相关规定不可再次预约；`<br/> `false：可以正常使用。   `|
|date|string|当前的日期|
|notice|string|公告信息显示哪个时间段什么原因不可使用|
|promise|List|所有可预约的时间段对象（内容看下一条 --> object）|
|object|Object|`id:预约时间段的id`;<br/>`total_num:每个时间段可以预约的总人数` <br/>`cur_num:当前已预约人数`<br/>`flag:是否可以继续预约，true为可以继续`|
|flag|bool|`true:登录成功`<br/> `false:登录失败`|
|errInfo|string|登录失败时返回的信息|
|token|string|后续请求所用鉴权字段+header|
|Level|Number|用户的权限等级<br />`0:普通用户` `1：普通管理员` |



###### 接口示例
> 地址：[http://www.api.com/index.php/login?num="201902010xxx"&pwd="xxx"]()

> 响应结果
``` json
{
    "BreakPromise":true,
    "date":"2020-12-16",
    "notice":"今日第5节课健身房由于XXX原因，预约关闭，请同学们合理安排健身时间",
    "Level":0,
    "promise":[
        {
            "id":1,
            "total_num":30,
            "cur_num":5,
            "flag":true

        }
    ],
    "errInfo":"",
    
}
```

***


**3\.预约**
###### 接口功能
> 预约时间

###### URL
> [服务器地址]()

> /HomePage/Main

###### 支持格式
> JSON

###### HTTP请求方式
> POST

###### 请求参数
|参数|必选|类型|说明|
|:---  |:-----|:---|---|
|num|true|string|用户的学工号|
|token|true|String|鉴权字符串|

###### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|flag   |   bool| `true:预约成功；`<br/> `false:预约失败`|
|errInfo|string|失败提示|


###### 接口示例
> 地址：[http://www.api.com/index.php/main?num="201902010xxx"]()

> 响应结果

```json

{
    "flag":true,
    "errInfo":""
}

```
***

**4\.取消预约**
###### 接口功能
> 在所预约的时间开始点提前30mins取消预约

###### URL
> [服务器地址]()

> /Information/Cancel

###### 支持格式
> JSON

###### HTTP请求方式
> POST

###### 请求参数
|参数|必选|类型|说明|
|:---  |:-----|:---|---|
|num|true|string|用户的学工号|
|id|true|num|所预约的时间段id|
|token|true|String|鉴权字符串|

###### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|flag   |   bool| `true:取消预约成功；`<br/> `false:取消预约失败`|
|errInfo|string|失败提示|


###### 接口示例
> 地址：[http://www.api.com/index.php/cancel?num="201902010xxx"&id=1]()

> 响应结果

```json

{
    "flag":true,
    "errInfo":""
}

```
***


**5\.历史预约**
###### 接口功能
> 查询最近的预约信息

###### URL
> [服务器地址]()

> /Information/history

###### 支持格式
> JSON

###### HTTP请求方式
> GET

###### 请求参数
|参数|必选|类型|说明|
|:---  |:-----|:---|---|
|num|true|string|用户的学工号|
|token|true|string|鉴权字符|

###### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|Book_info   |  List | 每一个预约的事件对象 |
|errInfo|string|错误提示|


###### 接口示例
> 地址：[http://www.api.com/index.php/history?num="201902010xxx"]()

> 响应结果

```json

{
    "Book_info":[
        {
            "date":"2020-01-01",
            "id":1, //预约那个时间段所拥有的id
            "status":1 //0 等待使用；  1 已完成； 2 已取消；
        }
    ],
    "errInfo":""
}

```
***


**6\.我的相关信息**
###### 接口功能
> 查询用户在数据库里的相关信息

###### URL
> [服务器地址]()

> /Information/UserInfo

###### 支持格式
> JSON

###### HTTP请求方式
> GET

###### 请求参数
|参数|必选|类型|说明|
|:---  |:-----|:---|---|
|num|true|string|用户的学工号|
|token|true|String|鉴权字符串|

###### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|user_info   |  Object | 用户的相关信息 |
|errInfo|string|错误提示|


###### 接口示例
> 地址：[http://www.api.com/index.php/info?num="201902010xxx"]()

> 响应结果

```json

{
    "user_info":{
        "name":"xxx",
        "break_num":2//爽约的次数

    },
    "errInfo":""
}

```
***


**7\.管理员首页**
###### 接口功能
> 展示给管理员看当前的预约状态

###### URL
> [服务器地址]()

> /Admin/Show

###### 支持格式
> JSON

###### HTTP请求方式
> POST

###### 请求参数
|参数|必选|类型|说明|
|:---  |:-----|:---|---|
|name|true|string |用户名|
|token|true|String|鉴权字符串|

###### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|date|string|当前的日期|
|notice|string|公告信息显示哪个时间段什么原因不可使用|
|promise|List|所有可预约的时间段对象（内容看下一条 --> object）|
|object|Object|`id:预约时间段的id`;<br/>`total_num:每个时间段可以预约的总人数` <br/>`cur_num:当前已预约人数`




###### 接口示例
> 地址：[http://www.api.com/index.php/reset?num="201902010xxx"&name="xxx"&phone="xxx"]()

> 响应结果
``` json
{
    

    "date":"2020-12-16",
    "notice":"今日第5节课健身房由于XXX原因，预约关闭，请同学们合理安排健身时间",
    "promise":[
        {
            "id":1,
            "total_num":30,
            "cur_num":5,
        }
    ],
    "errInfo":"",
    
}
    
}
```

***



**8\.管理员设置是否可预约页面**
###### 接口功能
> 设置时间段是否可预约页面

###### URL
> [服务器地址]()

> /Admin/Change

###### 支持格式
> JSON

###### HTTP请求方式
> POST

###### 请求参数
|参数|必选|类型|说明|
|:---  |:-----|:---|---|
|name|true|string|用户名|
|token|true|String|鉴权字符串|
|id|true|Number|更改的时间段的id|
|reason|true|String|时间不可预约的原因说明|
###### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|flag|bool|`true:更改成功`<br/> `false:更改失败`|




###### 接口示例
> 地址：[http://www.api.com/index.php/change?num="201902010xxx"&pwd="xxx"]()

> 响应结果
``` json
{
    

    "flag":true,
    "errInfo":""
    
}
```

***







