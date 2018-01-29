Kuaidi100API
=====

API analyzed from <http://www.kuaidi100.com>. Use at your own risk! Update on Jan.25th, 2018

Licensed under GPL 3.0.

Sample Code
-----

Java [PackageTracker](https://github.com/fython/PackageTracker).

APIs
-----

### Auto Detect Express Company

`POST`<http://www.kuaidi100.com/autonumber/autoComNum>

#### Parameters

In query string:

| Name | Description                            |
| ---- | -------------------------------------- |
| text | Express ID provided by express company |

Example:

```
POST /autonumber/autoComNum?text=1600887249033 HTTP/1.1
Origin: http://www.kuaidi100.com
Accept-Encoding: gzip, deflate
Host: www.kuaidi100.com
Accept-Language: zh-CN,zh;q=0.8,en;q=0.6
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/48.0.2564.116 Safari/537.36
Accept: application/json, text/javascript, */*; q=0.01
Referer: http://www.kuaidi100.com/
X-Requested-With: XMLHttpRequest
Content-Type: application/x-www-form-urlencoded
Connection: close
Content-Length: 0

```

#### Response

Example:

```
{
  "comCode": "",
  "num": "700074134800",
  "auto": [
    {
      "comCode": "yuantong",
      "id": "",
      "noCount": 275377,
      "noPre": "7000",
      "startTime": ""
    },
    {
      "comCode": "huitongkuaidi",
      "id": "",
      "noCount": 63541,
      "noPre": "7000",
      "startTime": ""
    }
  ]
}
```

> The result of re-sending the request may be different since we simplified the result "auto" array.

Explanation:

| Path             | Description                            |
| ---------------- | -------------------------------------- |
| comCode          | Unknown                                |
| num              | Express ID provided by express company |
| auto             | *Array* Auto detected express company  |
| auto[].comCode   | Express company code                   |
| auto[].id        | Unknown                                |
| auto[].noCound   | Unknown                                |
| auto[].noPre     | Unknown                                |
| auto[].startTime | Unknown                                |

Notes:

If no express company matched, the "auto" array should be an empty array.

### Query Express Status

`GET`<http://www.kuaidi100.com/query>

#### Parameters

In query string:

| Name     | Explanation                             |
| -------- | --------------------------------------- |
| type     | Express company code                    |
| postid   | Express ID provided by express company  |

Example:

```
GET /query?type=yunda&postid=1600887249033 HTTP/1.1
Accept-Encoding: gzip, deflate, sdch
Host: www.kuaidi100.com
Accept-Language: zh-CN,zh;q=0.8,en;q=0.6
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/48.0.2564.116 Safari/537.36
Accept: */*
Referer: http://www.kuaidi100.com/
X-Requested-With: XMLHttpRequest
```

#### Response

Example:

```
{
  "message": "ok",
  "nu": "700074134800",
  "ischeck": "1",
  "com": "yuantong",
  "status": "200",
  "condition": "F00",
  "state": "3",
  "data": [
    {
      "time": "2015-11-26 19:09:25",
      "context": "客户 签收人 : 近邻宝代收点 已签收  感谢使用圆通速递，期待再次为您服务",
      "ftime": "2015-11-26 19:09:25"
    },
    {
      "time": "2015-11-18 21:18:01",
      "context": "北京市海淀区学清路公司 已收入",
      "ftime": "2015-11-18 21:18:01"
    }
  ]
}
```

> The result of re-sending the request may be different since we simplified the result "auto" array.

Error Example:

```
{
  "status": "201",
  "message": "快递公司参数异常：单号不存在或者已经过期"
}
```

Explanation:

| Path           | Description                            |
| -------------- | -------------------------------------- |
| message        | API query result status message        |
| nu             | Express ID provided by express company |
| ischeck        | Meaningless                            |
| com            | Express company code                   |
| status         | API query result status                |
| condition      | Meaningless                            |
| state          | Express status, see notes              |
| data           | *Array* Express status data            |
| data[].time    | Express status creation time           |
| data[].context | Express status context                 |
| data[].ftime   | Express status creation time           |

Notes:

There are 7 possible values of "state":

| Value | Name         | Explanation                              |
| ----- | ------------ | ---------------------------------------- |
| 0     | Transporting | Express is being transported             |
| 1     | Accepted     | Express is accepted by the express company |
| 2     | Trouble      | Express is in knotty problem             |
| 3     | Delivered    | Express is successfully delivered        |
| 4     | Rejected     | Express is rejected by the receiver and has been successfully redelivered to the sender |
| 5     | Delivering   | Express is being delivered               |
| 6     | Rejecting    | Express is rejected by the receiver and is being redelivered to the sender |

### Express Company Codes

GET <http://www.kuaidi100.com/js/share/company.js>

#### Response

JavaScript.

Then remove `var jsoncom=` and replace `};` to `}` and `'` to `"` to convert it to json.

Returns **int**  and **array** of (**strings** , , , , , , , , , ,					, , , , ,  and **number** ) company.

Explanation:

| Path                      | Description                          |
| ------------------------- | ------------------------------------ |
| error_size                | Unknown                              |
| company                   | Express companies                    |
| company[].cid             | Meaningless                          |
| company[].id              | Express company id                   |
| company[].companyname     | Express company name                 |
| company[].shortname       | short for express company name       |
| company[].tel             | hotline of express company           |
| company[].url             | Meaningless                          |
| company[].code            | *comCode* of the company             |
| company[].comurl          | website of express company           |
| company[].isavailable     | **string** `0` when available        |
| company[].promptinfo      | Unknown                              |
| company[].testnu          | example package number               |
| company[].freg            | RegExp of a valid package number     |
| company[].freginfo        | package number description           |
| company[].telcomplaintnum | complaint hotline of express company |
| company[].queryurl        | official express query url           |
| company[].serversite      | delivery sites list url              |
| company[].hasvali         | **int** Unknown                      |

Example:

```javascript
var jsoncom={'company':[{'cid':'5','id':'1','companyname':'申通快递','shortname':'申通','tel':'95543','url':'st','code':'shentong','hasvali':0,'comurl':'http://www.sto.cn','isavailable':'0','promptinfo':'系统升级，请到申通官网查询','testnu':'229855869255','freg':'^[0-9]{12,13}$','freginfo':'申通单号由12位数字组成，常见以268*、368*、58*等开头','telcomplaintnum':'95543','queryurl':'http://www.sto.cn/web%20select.asp','serversite':'http://www.kuaidi100.com/network/province_5.htm'}],'error_size':-1};
```

#### Caution

顺丰快递的查询会被跳转到官网，请使用时注意。
