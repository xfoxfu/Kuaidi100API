YetAnotherExpressHelper
=====

Just another helper for querying Chinese express status built on [React-Native](http://facebook.github.io/react-native/).

Build on hacked APIs of [Kuaidi100](http://www.kuaidi100.com/).

Features
-----

- ~~Query Express Information~~
- ~~Auto Express Company Detect~~
- ~~Notification~~
- ~~SMS Notification Subscription~~

Hacked APIs
-----

### Auto Detect Express Company

`POST`<http://www.kuaidi100.com/autonumber/autoComNum>

#### Parameters

In query string:

|Name|Description                           |
|----|--------------------------------------|
|text|Express ID provided by express company|

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

|Path            |Description                           |
|----------------|--------------------------------------|
|comCode         |Unknown                               |
|num             |Express ID provided by express company|
|auto            |*Array* Auto detected express company |
|auto[].comCode  |Express company code                  |
|auto[].id       |Unknown                               |
|auto[].noCound  |Unknown                               |
|auto[].noPre    |Unknown                               |
|auto[].startTime|Unknown                               |

Notes:

If no express company matched, the "auto" array should be an empty array.

### Query Express Status

`GET`<http://www.kuaidi100.com/query>

#### Parameters

In query string:

|Name    |Explanation                            |
|--------|---------------------------------------|
|type    |Express company code                   |
|postid  |Express ID provided by express company |
|valicode|Meaningless, but to keep it null string|

Example:

```
GET /query?type=yunda&postid=1600887249033&valicode= HTTP/1.1
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

|Path            |Description                           |
|----------------|--------------------------------------|
|message         |API query result status message       |
|nu              |Express ID provided by express company|
|ischeck         |Meaningless                           |
|com             |Express company code                  |
|status          |API query result status               |
|condition       |Meaningless                           |
|state           |Express status, see notes             |
|data            |*Array* Express status data           |
|data[].time     |Express status creation time          |
|data[].context  |Express status context                |
|data[].ftime    |Express status creation time          |

Notes:

There are 7 possible values of "state":

|Value|Name         |Explanation                                                                            |
|-----|-------------|---------------------------------------------------------------------------------------|
|0    |Transporting |Express is being transported                                                           |
|1    |Accepted     |Express is accepted by the express company                                             |
|2    |Trouble      |Express is in knotty problem                                                           |
|3    |Delivered    |Express is successfully delivered                                                      |
|4    |Rejected     |Express is rejected by the receiver and has been successfully redelivered to the sender|
|5    |Delivering   |Express is being delivered                                                             |
|6    |Rejecting    |Express is rejected by the receiver and is being redelivered to the sender             |

### Express Company Codes

| 快递公司代码           | 公司名称                                                                                                                                      |
|------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| auspost                | 澳大利亚邮政(英文结果）                                                                                                                       |
| aae                    | AAE                                                                                                                                           |
| anxindakuaixi          | 安信达                                                                                                                                        |
| huitongkuaidi          | 百世汇通                                                                                                                                      |
| baifudongfang          | 百福东方                                                                                                                                      |
| bht                    | BHT                                                                                                                                           |
| youzhengguonei         | 包裹/平邮/挂号信（暂只支持HtmlAPI,要JSON、XML格式结果和签收状态state请联系企业QQ 800036857转“小佰”）                                          |
| bangsongwuliu          | 邦送物流                                                                                                                                      |
| cces                   | 希伊艾斯（CCES）                                                                                                                              |
| coe                    | 中国东方（COE）                                                                                                                               |
| chuanxiwuliu           | 传喜物流                                                                                                                                      |
| canpost                | 加拿大邮政Canada Post（英文结果）                                                                                                             |
| canpostfr              | 加拿大邮政Canada Post(德文结果）                                                                                                              |
| datianwuliu            | 大田物流                                                                                                                                      |
| debangwuliu            | 德邦物流                                                                                                                                      |
| dpex                   | DPEX                                                                                                                                          |
| dhl                    | DHL-中国件-中文结果                                                                                                                           |
| dhlen                  | DHL-国际件-英文结果                                                                                                                           |
| dhlde                  | DHL-德国件-德文结果（德国国内派、收的件）                                                                                                     |
| dsukuaidi              | D速快递                                                                                                                                       |
| disifang               | 递四方                                                                                                                                        |
| ems                    | EMS(中文结果)（暂只支持HtmlAPI,要JSON、XML格式结果和签收状态state请联系企业QQ 800036857 转“小佰”）                                            |
| ems                    | E邮宝（暂只支持HtmlAPI,要JSON、XML格式结果和签收状态state请联系企业QQ 800036857 转“小佰”）                                                    |
| emsen                  | EMS（英文结果）（暂只支持HtmlAPI,要JSON、XML格式结果和签收状态state请联系企业QQ 800036857 转“小佰”）                                          |
| emsguoji               | EMS-（中国-国际）件-中文结果/EMS-(China-International）-Chinese data(请联系企业QQ 800036857 转“小佰”开通）                                    |
| emsinten               | EMS-（中国-国际）件-英文结果/EMS-(China-International）-Englilsh data(请联系企业QQ 800036857 转“小佰”开通）                                   |
| fedex                  | Fedex-国际件-英文结果（说明：Fedex是国际件的英文结果，Fedex中国的请用“lianbangkuaidi”，Fedex-美国请用fedexus）                                |
| fedexcn                | Fedex-国际件-中文结果                                                                                                                         |
| fedexus                | Fedex-美国件-英文结果(说明：如果无效，请偿试使用fedex）                                                                                       |
| feikangda              | 飞康达物流                                                                                                                                    |
| feikuaida              | 飞快达                                                                                                                                        |
| rufengda               | 凡客如风达                                                                                                                                    |
| fengxingtianxia        | 风行天下                                                                                                                                      |
| feibaokuaidi           | 飞豹快递                                                                                                                                      |
| ganzhongnengda         | 港中能达                                                                                                                                      |
| guotongkuaidi          | 国通快递                                                                                                                                      |
| guangdongyouzhengwuliu | 广东邮政                                                                                                                                      |
| youzhengguonei         | 挂号信（暂只支持HtmlAPI,要JSON、XML格式结果和签收状态state请联系企业QQ 800036857 转“小佰”）                                                   |
| youzhengguonei         | 国内邮件（暂只支持HtmlAPI,要JSON、XML格式结果和签收状态state请联系企业QQ 800036857 转“小佰”）                                                 |
| youzhengguoji          | 国际邮件（暂只支持HtmlAPI,要JSON、XML格式结果和签收状态state请联系企业QQ 800036857 转“小佰”）                                                 |
| gls                    | GLS                                                                                                                                           |
| gongsuda               | 共速达                                                                                                                                        |
| huitongkuaidi          | 汇通快运                                                                                                                                      |
| huiqiangkuaidi         | 汇强快递                                                                                                                                      |
| tiandihuayu            | 华宇物流                                                                                                                                      |
| hengluwuliu            | 恒路物流                                                                                                                                      |
| huaxialongwuliu        | 华夏龙                                                                                                                                        |
| tiantian               | 海航天天                                                                                                                                      |
| haiwaihuanqiu          | 海外环球                                                                                                                                      |
| hebeijianhua           | 河北建华（暂只能查好乐买的单，其他商家要查，请发邮件至 wensheng_chen#kingdee.com(将#替换成@)开通权限                                          |
| haimengsudi            | 海盟速递                                                                                                                                      |
| huaqikuaiyun           | 华企快运                                                                                                                                      |
| haihongwangsong        | 山东海红                                                                                                                                      |
| jiajiwuliu             | 佳吉物流                                                                                                                                      |
| jiayiwuliu             | 佳怡物流                                                                                                                                      |
| jiayunmeiwuliu         | 加运美                                                                                                                                        |
| jinguangsudikuaijian   | 京广速递                                                                                                                                      |
| jixianda               | 急先达                                                                                                                                        |
| jinyuekuaidi           | 晋越快递                                                                                                                                      |
| jietekuaidi            | 捷特快递                                                                                                                                      |
| jindawuliu             | 金大物流                                                                                                                                      |
| jialidatong            | 嘉里大通                                                                                                                                      |
| kuaijiesudi            | 快捷速递                                                                                                                                      |
| kangliwuliu            | 康力物流                                                                                                                                      |
| kuayue                 | 跨越物流                                                                                                                                      |
| lianhaowuliu           | 联昊通                                                                                                                                        |
| longbanwuliu           | 龙邦物流                                                                                                                                      |
| lanbiaokuaidi          | 蓝镖快递                                                                                                                                      |
| lejiedi                | 乐捷递（暂只能查好乐买的单，其他商家要查，请发邮件至 wensheng_chen#kingdee.com(将#替换成@)开通权限                                            |
| lianbangkuaidi         | 联邦快递（Fedex-中国-中文结果）（说明：国外的请用 fedex）                                                                                     |
| lianbangkuaidien       | 联邦快递(Fedex-中国-英文结果）                                                                                                                |
| lijisong               | 立即送（暂只能查好乐买的单，其他商家要查，请发邮件至 wensheng_chen#kingdee.com(将#替换成@)开通权限)                                           |
| longlangkuaidi         | 隆浪快递                                                                                                                                      |
| menduimen              | 门对门                                                                                                                                        |
| meiguokuaidi           | 美国快递                                                                                                                                      |
| mingliangwuliu         | 明亮物流                                                                                                                                      |
| ocs                    | OCS                                                                                                                                           |
| ontrac                 | onTrac                                                                                                                                        |
| quanchenkuaidi         | 全晨快递                                                                                                                                      |
| quanjitong             | 全际通                                                                                                                                        |
| quanritongkuaidi       | 全日通                                                                                                                                        |
| quanyikuaidi           | 全一快递                                                                                                                                      |
| quanfengkuaidi         | 全峰快递                                                                                                                                      |
| sevendays              | 七天连锁                                                                                                                                      |
| rufengda               | 如风达快递                                                                                                                                    |
| shentong               | 伸通（暂只支持HtmlAPI,要JSON、XML格式结果和签收状态state请联系企业QQ 800036857 转“小佰”）                                                     |
| shunfeng               | 顺丰速递（中文结果）（暂只支持HtmlAPI,要JSON、XML格式结果和签收状态state请联系企业QQ 800036857 转“小佰”）                                     |
| shunfengen             | 顺丰（英文结果）（暂只支持HtmlAPI,要JSON、XML格式结果和签收状态state请联系企业QQ 800036857 转“小佰”）                                         |
| santaisudi             | 三态速递                                                                                                                                      |
| shenghuiwuliu          | 盛辉物流                                                                                                                                      |
| suer                   | 速尔物流                                                                                                                                      |
| shengfengwuliu         | 盛丰物流                                                                                                                                      |
| shangda                | 上大物流                                                                                                                                      |
| santaisudi             | 三态速递                                                                                                                                      |
| haihongwangsong        | 山东海红                                                                                                                                      |
| saiaodi                | 赛澳递                                                                                                                                        |
| haihongwangsong        | 山东海红（暂只能查好乐买的单，其他商家要查，请发邮件至 wensheng_chen#kingdee.com(将#替换成@)开通权限）                                        |
| sxhongmajia            | 山西红马甲（暂只能查天天网的单，其他商家要查，请发邮件至 wensheng_chen#kingdee.com(将#替换成@)开通权限)                                       |
| shenganwuliu           | 圣安物流                                                                                                                                      |
| suijiawuliu            | 穗佳物流                                                                                                                                      |
| tiandihuayu            | 天地华宇                                                                                                                                      |
| tiantian               | 天天快递                                                                                                                                      |
| tnt                    | TNT（中文结果）                                                                                                                               |
| tnten                  | TNT（英文结果）                                                                                                                               |
| tonghetianxia          | 通和天下（暂只能查好乐买的单，其他商家要查，请发邮件至 wensheng_chen#kingdee.com(将#替换成@)开通权限）                                        |
| ups                    | UPS（中文结果）                                                                                                                               |
| upsen                  | UPS（英文结果）                                                                                                                               |
| youshuwuliu            | 优速物流                                                                                                                                      |
| usps                   | USPS（中英文）                                                                                                                                |
| wanjiawuliu            | 万家物流                                                                                                                                      |
| wanxiangwuliu          | 万象物流                                                                                                                                      |
| weitepai               | 微特派（暂只能查天天网的单，其他商家要查，请发邮件至 wensheng_chen#kingdee.com(将#替换成@)开通权限)                                           |
| xinbangwuliu           | 新邦物流                                                                                                                                      |
| xinfengwuliu           | 信丰物流                                                                                                                                      |
| xingchengjibian        | 星晨急便（暂不支持，该公司已不存在）                                                                                                          |
| xinhongyukuaidi        | 鑫飞鸿（暂不支持，该公司已不存在）                                                                                                            |
| cces                   | 希伊艾斯(CCES)（暂不支持，该公司已不存在）                                                                                                    |
| xinbangwuliu           | 新邦物流                                                                                                                                      |
| neweggozzo             | 新蛋奥硕物流                                                                                                                                  |
| hkpost                 | 香港邮政                                                                                                                                      |
| yuantong               | 圆通速递（暂只支持HtmlAPI,要JSON、XML格式结果和签收状态state请联系企业QQ 800036857 转“小佰”）                                                 |
| yunda                  | 韵达快运（暂只支持HtmlAPI,要JSON、XML格式结果和签收状态state请联系企业QQ 800036857 转“小佰”）                                                 |
| yuntongkuaidi          | 运通快递                                                                                                                                      |
| youzhengguonei         | 邮政小包（国内），邮政包裹（国内）、邮政国内给据（国内）（暂只支持HtmlAPI,要JSON、XML格式结果和签收状态state请联系企业QQ 800036857 转“小佰”） |
| youzhengguoji          | 邮政小包（国际），邮政包裹（国际）、邮政国内给据（国际）（暂只支持HtmlAPI,要JSON、XML格式结果和签收状态state请联系企业QQ 800036857 转“小佰”） |
| yuanchengwuliu         | 远成物流                                                                                                                                      |
| yafengsudi             | 亚风速递                                                                                                                                      |
| yibangwuliu            | 一邦速递                                                                                                                                      |
| youshuwuliu            | 优速物流                                                                                                                                      |
| yuanweifeng            | 源伟丰快递                                                                                                                                    |
| yuanzhijiecheng        | 元智捷诚                                                                                                                                      |
| yuefengwuliu           | 越丰物流                                                                                                                                      |
| yuananda               | 源安达                                                                                                                                        |
| yuanfeihangwuliu       | 原飞航                                                                                                                                        |
| zhongxinda             | 忠信达快递                                                                                                                                    |
| zhimakaimen            | 芝麻开门                                                                                                                                      |
| yinjiesudi             | 银捷速递                                                                                                                                      |
| yitongfeihong          | 一统飞鸿（暂只能查天天网的单，其他商家要查，请发邮件至 wensheng_chen#kingdee.com(将#替换成@)开通权限)                                         |
| zhongtong              | 中通速递（暂只支持HtmlAPI,要JSON、XML格式结果和签收状态state请联系企业QQ 800036857 转“小佰”）                                                 |
| zhaijisong             | 宅急送                                                                                                                                        |
| zhongyouwuliu          | 中邮物流                                                                                                                                      |
| zhongxinda             | 忠信达                                                                                                                                        |
| zhongsukuaidi          | 中速快件                                                                                                                                      |
| zhimakaimen            | 芝麻开门                                                                                                                                      |
| zhengzhoujianhua       | 郑州建华（暂只能查好乐买的单，其他商家要查，请发邮件至 wensheng_chen#kingdee.com(将#替换成@)开通权限）                                        |
| zhongtianwanyun        | 中天万运                                                                                                                                      |
