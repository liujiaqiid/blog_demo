# API Design

目录
- [Changes Log](#chg_log)
- [API设计步骤](#api_principle)
- [服务端资源设计](#resource_desigin)
- [资源接口详细设计](#api_desigin)
  - [通用应答](#api_common_answer)
  - [登陆接口](#api_login)
  - [上传图片接口](#api_up_img)
  - [获取图片接口](#api_get_img)
  - [添加主任/老师/医生信息接口](#api_add_doctor)
  - [查询主任/老师/医生信息接口](#api_find_doctor)
  - [添加用户(会员)信息接口](#api_add_user)
  - [查看联系人列表接口](#api_find_contacts)
  - [查看用户详细信息](#api_find_one_user)
  - [查看用户的关怀记录(分页查询)](#api_find_user_cr)
  - [医生新增用户的关怀记录](#api_add_user_cr)
  - [检验(根据手机号、身份证号)患者是否已存在接口](#api_check_user_exists)
  - [新增用户反馈信息接口](#api_add_feedback)
  - [新增预约接口](#api_add_appointment)
  - [医生新增竞价时间](#api_add_resource)
  - [医生竞价资源查询](#api_find_resource)
  - [购买医生竞价资源](#api_purchase_resource)
  - [医生竞价资源修改](#api_update_resource)



<div id="chg_log"></div>
## Changes Log
<table>
  <th>
    <td>时间</td>
    <td>修改人</td>
    <td>说明</td>
    <td>审核人</td>
  </th>
  <tr>
    <td></td>
    <td>2014-04-01</td>
    <td>刘家齐</td>
    <td>完成API资源借口的基本设计</td>
    <td>Evan</td>
  </tr>
  <tr>
    <td></td>
    <td>2014-04-23</td>
    <td>刘家齐</td>
    <td>修改部分资源接口设计、修改登陆业务接口</td>
    <td>...</td>
  </tr>
  <tr>
    <td> </td>
    <td>2014-05-17</td>
    <td>刘家齐</td>
    <td>添加整理v0.5业务涉及的接口</td>
    <td>...</td>
  </tr>
  <tr>
    <td> </td>
    <td>2014-05-27</td>
    <td>刘家齐</td>
    <td>添加整理v0.5.6业务涉及的接口,医生资源的CRUD</td>
    <td>...</td>
  </tr>
  <tr>
    <td> </td>
    <td>2014-06-02</td>
    <td>刘家齐</td>
    <td>添加API文档目录</td>
    <td>...</td>
  </tr>
  <tr>
    <td> </td>
    <td> </td>
    <td> </td>
    <td> </td>
    <td> </td>
  </tr>
</table>

<h2 id="api_principle">API设计原则及步骤</h2>

  基本原则包括如下几点：
  + 以RESTful架构风格组织服务端资源
    + Resource as abstraction
    + Stateless
    + URI as resource identifier
    + Unique method (GET/POST/PUT/DELETE)
    + Compatibility & Extensibility
  + 不绑定前端features
  + 不绑定后端schema
	+ Security(Eg. Login/HIPAA)
	+ Reduce network require times

<div id="resource_design"></div>
## 云端服务器资源设计

+ 医生/专家/老师/主任信息

| Method | Path | Input | Output | Usage |
| ------ | ---- | ----- | ------ | ----- |
| POST | /api/v1/doctors | {doctor_data} | {common_response} | 后端添加医生 |
| GET  | /api/v1/doctors | {} | {doctor_data_list} | 查看医生列表信息 |
| ~~PUT~~  | /api/v1/doctors | {} | {} | 禁止更新列表 |
| ~~DELETE~~ | /api/v1/doctors | {} | {} | 禁止删除列表 |

+ (单个)医生/专家/老师/主任信息

| Method | Path | Input | Output | Usage |
| ------ | ---- | ----- | ------ | ----- |
| ~~POST~~ | /api/v1/doctors/:uuid | {} | {} | 禁止 |
| GET  | /api/v1/doctors/:uuid | {} | {doctor_data} | 查看某医生信息 |
| PUT  | /api/v1/doctors/:uuid | {doctor_data} | {common_response} | 更新某医生信息 |
| DELETE | /api/v1/doctors/:uuid | {} | {common_response} | 删除某医生信息 |

+ 医生的基本信息/doctors/:uuid/profile

| Method | Path | Input | Output | Usage |
| ------ | ---- | ----- | ------ | ----- |
| ~~POST~~ | /api/v1/doctor/:uuid/profile | {} | {} | 禁用 |
| GET  | /api/v1/doctor/:uuid/profile | {} | { doctor_profile } | 查看基本信息 |
| PUT  | /api/v1/doctor/:uuid/profile | {doctor_profile_data} | {common_response} | 更新基本信息 |
| ~~DELETE~~ | /api/v1/doctor/:uuid/profile | {} | {} | 禁止 |

+ 医生的执业地点信息/doctors/:uuid/places

| Method | Path | Input | Output | Usage |
| ------ | ---- | ----- | ------ | ----- |
| ~~POST~~ | /api/v1/doctors/:uuid/places | {} | {} | 禁用 |
| GET  | /api/v1/doctors/:uuid/places | {} | {doctor_profile} | 查看执业点信息 |
| PUT  | /api/v1/doctors/:uuid/places | {doctor_profile_data} | {common_response} | 更新执业点信息 |
| ~~DELETE~~ | /api/v1/doctors/:uuid/places | {} | {} | 禁止 |

+ ~~医生的头像信息/doctors/:uuid/avatar~~

+ 医生的患者联系人(关系)/doctors/:uuid/patients

| Method | Path | Input | Output | Usage |
| ------ | ---- | ----- | ------ | ----- |
| ~~POST~~ | /api/v1/doctors/:uuid/patients | {} | {} | 禁用 |
| GET  | /api/v1/doctors/:uuid/patients | {} | {doctor_profile} | 查看医患关系列表 |
| ~~PUT~~  | /api/v1/doctors/:uuid/patients | {} | {} | 禁止 |
| ~~DELETE~~ | /api/v1/doctors/:uuid/patients | {} | {} | 禁止 |

+ 患者信息

| Method | Path | Input | Output | Usage |
| ------ | ---- | ----- | ------ | ----- |
| POST | /api/v1/patients | {patient_data} | {common_response} | 新增患者 |
| GET  | /api/v1/patients | {} | { patient_data_list } | 查看患者列表信息 |
| ~~PUT~~  | /api/v1/patients | {} | {} | 禁止 |
| ~~DELETE~~ | /api/v1/patients | {} | {} | 禁止 |

+ (某个)患者信息

| Method | Path | Input | Output | Usage |
| ------ | ---- | ----- | ------ | ----- |
| ~~POST~~ | /api/v1/patients/:uuid | {} | {} | 禁用 |
| GET  | /api/v1/patients/:uuid | {} | {patient_data} | 查看患者信息 |
| PUT  | /api/v1/patients/:uuid | {patient_data} | {common_response} | 修改患者信息 |
| DELETE | /api/v1/patients/:uuid | {} | {common_response} | 删除患者 |

+ 患者基本信息的子文档/patients/:uuid/profile

| Method | Path | Input | Output | Usage |
| ------ | ---- | ----- | ------ | ----- |
| ~~POST~~ | /api/v1/patients/:uuid/profile | {} | {} | 禁用 |
| GET  | /api/v1/patients/:uuid/profile | {} | {patient_profile_data} | 查看患者基本信息 |
| PUT  | /api/v1/patients/:uuid/profile | { patient_profile_data } | {common_response} | 修改患者基本信息 |
| ~~DELETE~~ | /api/v1/patients/:uuid/profile | {} | {common_response} | 禁用 |

+ 患者检测数据子文档/patients/:uuid/observation/:type

| Method | Path | Input | Output | Usage |
| ------ | ---- | ----- | ------ | ----- |
| POST | /patients/:uuid/observation/:type | {observation_data} | {common_response} | 新增观测记录 |
| GET  | /patients/:uuid/observation/:type | {} | {observation_data_list} | 获取观测列表数据 |
| PUT  | /patients/:uuid/observation/:type | {observation_data} | {common_response} | 修改观测数据信息 |
| DELETE | /patients/:uuid/observation/:type | {} | {common_response} | 删除患者当前的观测记录 |

+ ~~ 患者的头像子文档/patients/:uuid/avatar ~~

+ 患者的病历子文档/patients/:uuid/records

| Method | Path | Input | Output | Usage |
| ------ | ---- | ----- | ------ | ----- |
| POST | /patients/:uuid/records | {record_data} | {common_response} | 新增患者病历 |
| GET  | /patients/:uuid/records | {} | {record_data_list} | 获取患者病历列表 |
| ~~PUT~~  | /patients/:uuid/records | {} | {} | 禁用 |
| ~~DELETE~~ | /patients/:uuid/records | {} | {} | 禁用 |

+ 短病历资源集/records

| Method | Path | Input | Output | Usage |
| ------ | ---- | ----- | ------ | ----- |
| POST | /api/v1/records | {record_data} | {common_response} | 新增患者病历 |
| GET  | /api/v1/records | {} | {record_data_list} | 获取患者病历列表 |
| ~~PUT~~  | /api/v1/records | {} | {} | 禁用 |
| ~~DELETE~~ | /api/v1/records | {} | {} | 禁用 |

+ 某个短病历资源/records/:uuid

| Method | Path | Input | Output | Usage |
| ------ | ---- | ----- | ------ | ----- |
| ~~POST~~ | /api/v1/records/:uuid | {} | {} | 禁用 |
| GET  | /api/v1/records/:uuid | {} | {record_data} | 获取某份病历 |
| PUT | /api/v1/records/:uuid | {record_data} | {common_response} | 修改某份病历 |
| DELETE | /api/v1/records/:uuid | {} | {common_response} | 删除某份病历 |

+ 地点信息/places

| Method | Path | Input | Output | Usage |
| ------ | ---- | ----- | ------ | ----- |
| POST | /api/v1/places | {record_data} | {common_response} | 新增地点信息 |
| GET  | /api/v1/places | {} | {record_data_list} | 获取执业地点列表 |
| ~~PUT~~  | /api/v1/places | {} | {} | 禁用 |
| ~~DELETE~~ | /api/v1/places | {} | {} | 禁用 |

+ 某个地点信息/places/:uuid

| Method | Path | Input | Output | Usage |
| ------ | ---- | ----- | ------ | ----- |
| ~~POST~~ | /api/v1/places/:uuid | {} | {} | 禁用 |
| GET  | /api/v1/places/:uuid | {} | {record_data} | 获取执业地点列表 |
| PUT  | /api/v1/places/:uuid | {} | {common_response} | 修改地点信息 |
| DELETE | /api/v1/places/:uuid | {} | {common_response} | 删除执业点信息 |

+ 用户反馈信息/feedbacks

| Method | Path | Input | Output | Usage |
| ------ | ---- | ----- | ------ | ----- |
| POST | /api/v1/feedbacks | {feedback_data} | {common_response} | 新增反馈信息 |
| GET  | /api/v1/feedbacks | {} | {feedback_data_list} | 获取反馈信息列表 |
| ~~PUT~~  | /api/v1/feedbacks | {} | {} | 禁用 |
| ~~DELETE~~ | /api/v1/feedbacks | {} | {} | 禁用 |

+ 用户反馈信息/feedbacks/:uuid

| Method | Path | Input | Output | Usage |
| ------ | ---- | ----- | ------ | ----- |
| ~~POST~~ | /api/v1/feedbacks/:uuid | {} | {} | 禁用 |
| GET  | /api/v1/feedbacks/:uuid | {} | {feedback_data} | 获取某条反馈信息 |
| PUT  | /api/v1/feedbacks/:uuid | {feedback_data} | {common_response} | 修改某条反馈信息 |
| DELETE | /api/v1/feedbacks/:uuid | {} | {common_response} | 删除某条反馈信息 |

+ 医生资源信息/resources

| Method | Path | Input | Output | Usage |
| ------ | ---- | ----- | ------ | ----- |
| POST | /api/v1/resources | {resource_data} | {common_response} | 新增医生资源信息 |
| GET  | /api/v1/resources | {} | {resource_data_list} | 获取资源信息列表 |
| ~~PUT~~  | /api/v1/resources | {} | {} | 禁用 |
| ~~DELETE~~ | /api/v1/resources | {} | {} | 禁用 |

+ (某条)医生资源信息/resources/:uuid

| Method | Path | Input | Output | Usage |
| ------ | ---- | ----- | ------ | ----- |
| ~~POST~~ | /api/v1/resources/:uuid | {} | {} | 禁用 |
| GET  | /api/v1/resources/:uuid | {} | {resource_data} | 获取某条资源信息 |
| PUT  | /api/v1/resources/:uuid | {resource_data} | {common_response} | 修改某条资源信息 |
| DELETE | /api/v1/resources/:uuid | {} | {common_response} | 删除某条资源信息 |

<h2 id='api_desigin'> 云端服务器资源接口详细设计 </h2>

<h3 id='api_common_answer'> 1.通用应答格式 </h3>

<hr/>
```Json
  {
    status:0,
    msg:"",
    res:{}
  }
```
<h3 id='api_login'>2.登陆</h3>

<hr/>
Usage: 用户登录认证接口

Host: Api server

Path: /api/v1

Method: POST

Request JSON:

```Json
  {
    name:xxxx,
    pwd:xxx
  }
```
Response(成功应答)
```Json
  {
    status:0,
    msg:"",
    res:{
      "_id":"",
      "avatar":"",
      "employeeNo":"",
      "name":"",
      "latestUpdate":""
      "cdnInfo": {
        "uri": "http://zlycare-static-files.qiniudn.com/"
        "token": "Wi2x96mF8LCgvZMx7eDCliZ56-HHzBxWXfvf8yo9:t3LE29YMDTWsCDHfGSanZdQG1D8=:eyJzY29wZSI6InpseWNhcmUtc3RhdGljLWZpbGVzIiwiZGVhZGxpbmUiOjEzOTgyNDE3NDF9"
        "smallView": "imageview2/2/w/50/h/50"
        "middleView": "imageview2/2/w/150/h/150"
      }
    }
  }
```
(失败应答)
```Json
  {
    status:-N,
    msg:"Error message"
  }
```

<h3 id='api_up_img'> 3.上传图片 </h3>

<hr/>
Usage:  上传静态图片接口

Host: Qiniu Server

Path: /

Method: POST

Content-Type: multipart/form-data;

Request form:
```Json
  {
    token: "...UploadToken...",
    file: ImageFile,
    key: "...UUID..."
  }
```

Response: {}

<h3 id='api_get_img'> 4.获取静态图片</h3>

<hr/>
Usage: 获取静态图片接口

Host: Qiniu Server

Path: /:uuid

Params:
参考[七牛处理API](http://developer.qiniu.com/docs/v6/api/reference/fop/image/imageview2.html)

Method: GET

Request Json: {}

Response: 图片信息

<h3 id='api_add_doctor'> 5.添加主任/老师/医生信息</h3>

<hr/>
Usage: 后台添加主任/老师/医生信息

Host: Api Server

Path: /api/v1/doctors

Params: NULL

Method: POST

Request JSON:
```Json
 {
    "name":"朱一生",
    "sex":"男",
    "pwd":"112"
    "birthday": "1397620337",
    "nationality":"中国",
    "nation":"香港",
    "birthplace":"中国",
    "phoneNum":"13100000000",
    "employeeNo":"1003"
    ...
 }
```
Response JSON:

成功应答
```Json
{
  status:0,
  msg:"",
  res:{}
}
```
失败应答:
```Json
{
  status:-N,
  msg:"Error message"
}
```

<h3 id='api_find_doctor'> 6.查询主任/老师/医生详细信息</h3>

<hr/>
Usage: 后台查看医生用户信息

Host:  Api Server

Path: /api/v1/doctors/:uuid

Params: NULL

Method: GET

Request JSON:{}

Response:

返回详细信息
```Json
  {
    "_id":"",
    "avatar":"",
    "name":"",
    "sex":"",
    "birthday":"",
    "nationality":"",
    "nation":"",
    "birthplace":"",
    "phoneNum":"",
    "phoneAuth":"",
    "prcIdCard":"",
    "medicineId":"",
    "familyAddress":"",
    "eduBackground":"",
    "schoolGraduating":"",
    "emergencyContact":"",
    "emergencyContactPhone":"",
    "employeeNo":"",
    "state":"",
    "position":"",
    "workingUnit":"",
    "department":"",
    "team":"",
    "specialistArea":"",
    "recommender":"",
    "comment":5,
    "description":"",
    "instantMessage":"",
    "graduateTime":"",
    "tutor":"",
    "paper":"",
    "attraction":"",
    "serveContent":"",
    "intent":"",
    "joinTime":"",
    "income":"",
    "requirement":"",
    "memo":"",
    "latestUpdate":"",
    "registerDate":"",
    "workBeginTime":""
    "places":[""]
  }
```

<h3 id='api_add_user'>7.添加用户(会员)信息</h3>

<hr/>
Usage: (主任、医生、老师)新增用户信息

Host:  Api Server

Path: /api/v1/patients

Params: NULL

Method: POST

Request JSON:
```Json
{
  "avatar": "",
  "name": "张三",
  "sex": "",
  "birthday": "",
  "height": 172,
  "weight": 68,
  "nation": "中国",
  "nationality": "汉",
  "nativePlace": "北京",
  "phoneNum": "13900000001",
  "phoneAuth": false,
  "prcIdCard": "",
  "pwd": "123",
  "emergencyContact": "张三儿",
  "emergencyContactPhone": "13900000002",
  "workUnit": "央视",
  "workAddress": "北京",
  "workPhone": "10000",
  "isAppointment": true,
  "bloodType": "A",
  "medicineId": "YBH00001",
  "type": "",
  "familyAddress": "北京市海淀区",
  "familyHistory": ["糖尿病"],
  "selfHistory": ["慢性胃炎"],
  "allergyHistory": ["花粉","花生"],
  "insuranceCenCode": "",
  "insuranceCenName": "",
  "patientSource": "",
  "homeDoctorId": "",
  "homeDoctorName": "",
  "creatorType": "",
  "creatorDoctorId": "534eb428c0d6ce4b3359225b",
  "creatorDoctorName": "",
  "roamingDoctorId": "",
  "roamingDoctorName": ""
}
```

Response JSON:

成功应答
```Json
{
  status:0,
  msg:"",
  res:{}
}
```
失败应答:
```Json
{
  status:-N,
  msg:"Error message"
}
```

<h3 id='api_find_contacts'> 8.查看联系人列表</h3>

<hr/>
Usage: (主任、医生、老师)查看联系人列表

Host:  Api Server

Path: /api/v1/doctors/:uuid/patients

Params: NULL

Method: GET

Request JSON:{}

Response JSON:
```Json
"status":0,
"msg":"",
"res":{
"relations":[
{
"_id":"534f51d084a5453348ac661e",
"name":"张三",
"phoneNum":"13900000001",
"pinyin":[
],
"avatar":"10000001",
....
},
{
"_id":"534f521684a5453348ac661f",
"name":"李四",
"phoneNum":"13900000002",
"pinyin":[
],
"avatar":"111111",
...
},
{
"name":"王五",
"phoneNum":"13900000001",
"_id":"534f719d3aa1511a49271ab6",
"pinyin":[
],
"avatar":"534f6ec9e24289f1489a2de2",
...
}
],
"appointments":[
{
"_id":"5358d1d0a26ae7ec6bf991c4",
"creatorId":"534eb428c0d6ce4b3359225b",
"creatorName":"",
"appointDoctorId":"534eb428c0d6ce4b3359225b",
"appointDoctorName":"",
"appointPatientId":"534f51d084a5453348ac661e",
"appointPatientName":"",
"appointPatientDate":"2014-05-04T16:00:00.000Z",
"status":1
}
]
}
}
```

<h3 id='api_find_one_user'> 9.查看用户详细信息</h3>

<hr/>
Usage: (主任、医生、老师)查看用户详细信息

Host:  Api Server

Path: /api/v1/patients/:uuid

Params: NULL

Method: GET

Request JSON:{}

Response JSON:
```Json
"status":0,
"msg":"",
"res":{
[{
"_id":"5369e0a32840775329bd6fad",
"birthday":"1959-12-31T16:00:00.000Z",
"bloodType":"A",
"creatorDoctorId":"534eb428c0d6ce4b3359225b",
"creatorDoctorName":"",
"creatorType":"主任",
"emergencyContact":"",
"emergencyContactPhone":"",
"familyAddress":"",
"homeDoctorId":"534eb428c0d6ce4b3359225b",
"homeDoctorName":"",
"medicineId":"YBH00004",
"name":"张卫健",
"nation":"汉",
"nationality":"中国",
"nativePlace":"中国",
"phoneNum":"12990000032",
"prcIdCard":"430430333333333333",
"roamingDoctorId":"",
"sex":"男",
"workAddress":"",
"workPhone":"",
"workUnit":"",
"status":1,
"patientSource":"",
"allergyHistory":[""],
"selfHistory":[""],
"familyHistory":[""],
"createDate":"2014-05-07T07:47:07.259Z",
"latestUpdate":"2014-05-07T10:24:23.424Z",
"weight":34,
"height":199,
"pinyin":["zhang","wei","jian"],
"avatar":"",
"vipNum":10000}]
}
```

<h3 id='api_find_user_cr'> 10.查看用户的关怀记录(分页查询)</h3>

<hr/>
Usage: (主任、医生、老师)查看用户的关怀记录列表

Host:  Api Server

Path: /api/v1/patients/:uuid/records

Params:

| Param | Usage | isRequired |
| ----- | ----- | ---------- |
| pageSize | 分页大小 | 非必需，默认10 |
| pageNum | 当前页数 | 非必需，默认为0 |

Method: GET

Request JSON:{}

Response JSON:

```Json
{
"status":0,
"msg":"",
"res":[
{
"recordCode":"w123",
"customerId":"534f719d3aa1511a49271ab6",
"customerName":"tom",
"doctorId":"534eb428c0d6ce4b3359225b",
"doctorName":"Adoctor",
"placeId":"534f719d3aa1511a49271ab6",
"placeName":"",
"outpatientCode":"",
"insuranceCode":"",
"symptom":"有点晕...",
"diagnose":"没睡好...",
"prescription":"",
"paymentType":"现金",
"imagesDescription":"测试头像测试头像",
"_id":"5359a1089422a50a05553acd",
"status":1,
"images":[
"534eb0e81097e01b33e55196",
"534f6ec9e24289f1489a2de2"
],
"diagnoseDate":"2014-04-24T23:40:56.457Z",
"createDate":"2014-04-24T23:40:56.457Z",
"latestUpdate":"2014-04-24T23:40:56.457Z"
}
]
}
```

<h3 id='api_add_user_cr'> 11.医生新增用户的关怀记录</h3>

<hr/>
Usage: (主任、医生、老师)新增用户的关怀记录

Host:  Api Server

Path: /api/v1/patients/:uuid/records

Params: NULL

Method: POST

Request JSON:

```Json
{
  "recordCode":"w123",
  "customerId":"534f719d3aa1511a49271ab6",
  "customerName":"tom",
  "doctorId":"534eb428c0d6ce4b3359225b",
  "doctorName":"A doctor",
  "placeId": "534f719d3aa1511a49271ab6",
  "placeName":"",
  "outpatientCode":"",
  "insuranceCode":"",
  "symptom":"有点晕...",
  "diagnose":"没睡好...",
  "prescription":"",
  "paymentType":"现金",
  "images": ["534eb0e81097e01b33e55196","534f6ec9e24289f1489a2de2"],
  "imagesDescription":"测试头像测试头像",
  "status":1
}
```
Response JSON:

成功应答:
```Json
{
  status:0,
  msg:"",
  res:{}
}
```
失败应答:
```Json
{
  status:-N,
  msg:"Error message"
}
```

<h3 id='api_check_user_exists'>12.检验(根据手机号、身份证号)患者是否已存在接口</h3>

<hr/>
Host:  Api Server

Path: /api/v1/patients/:para/check

Params: NULL

Method: GET

Response JSON:

患者存在：
```Json
{
  status:0,
  msg:"",
  res:{}
}
```
患者不存在：
```Json
{
  status:1,
  msg:XXX,
  res:{}
}
```

失败应答:
```Json
{
  status:-N,
  msg:"Error message"
}
```

<h3 id='api_add_feedback'> 13.新增用户反馈信息接口</h3>

<hr/>
Host:  Api Server

Path: /api/v1/feedbacks

Params: NULL

Method: POST

Request JSON:

```Json
{
  "userId":"534eb428c0d6ce4b3359225b",
  "content":"随医行建议"
}
```
Response JSON:

成功应答
```Json
{
  status:0,
  msg:"",
  res:{}
}
```
失败应答:
```Json
{
  status:-N,
  msg:"Error message"
}
```

<h3 id='api_add_appointment'> 14.新增预约接口</h3>
<hr/>
Host:  Api Server

Path: /api/v1/doctors/patients/appointment

Params: NULL

Method: POST

Request JSON:

```Json
{
  creatorId: ObjectId,
  creatorName: String,
  appointDoctorId: ObjectId,
  appointDoctorName: String,
  appointPatientId: ObjectId,
  appointPatientName: String,
  appointPatientDate: Date,
  comment: String
}
```
Response JSON:

成功应答
```Json
{
  status:0,
  msg:"",
  res:{}
}
  ```
失败应答:
```Json
{
  status:-N,
  msg:"Error message"
}
```

<h3 id='api_add_resource'> 15. 医生新增竞价时间</h3>

<hr/>
Host:  Api Server

Path: /api/v1/resources

Params: NULL

Method: POST

Request JSON:

```Json
{
"type":0,
"ownerId":"5382bf314018f269a1e7cdb0",
"ownerName": "张医生",
"ownerTel": "13100000011",
"timeBegin": 1402000711103,
"timeEnd": 1402100711103,
"price": 1000
}
```

Response JSON:

成功应答
```Json
{
  status:0,
  msg:"",
  res:{}
}
```
失败应答:
```Json
{
  status:-N,
  msg:"Error message"
}
```

<h3 id='api_find_resource'> 16. 医生竞价资源查询</h3>

<hr/>
Host:  Api Server

Path: /api/v1/resources

Params:

| Param | Usage | isRequired |
| ----- | ----- | ---------- |
| status | 是否已被订购，0-已订购 | 非必需|
| owner  | 资源的发布者UUID | 非必需|
| pageSize | 分页大小 | 非必需，默认100 |
| pageNum | 当前页数 | 非必需，默认为0 |

Method: GET

Request JSON: {}

Response JSON:

成功应答

```Json
{
  status:0,
  msg:"",
  res:[
   {
    ownerId: "5382bf314018f269a1e7cdb0"
    ownerName: "张医生"
    ownerTel: "13100000011"
    _id: "5382c107aa0a5bbe0b547a3d"
    createDate: 1401077982362
    price: 1000
    timeEnd: 1402100711103
    timeBegin: 1402000711103
    purchaseStatus: 1
    type: 0
  },{
    ownerId: "536a2a1df0cf6db5b73b78be"
    ownerName: "Test"
    ownerTel: "1370000000"
    _id: "53800da7014438fa4af0e609"
    createDate: 1401087037988
    price: 1000
    timeEnd: 0
    timeBegin: 0
    purchaseStatus: 1
    type: 0
    }
  ]
}
```
失败应答:

```Json
{
  status:-N,
  msg:"Error message"
}
```

<h3 id='api_purchase_resource'> 17. 购买医生竞价资源</h3>

<hr/>
Host:  Api Server

Path: /api/v1/resources/:uuid?action=purchase

Params:

| param | usage | isRequired |
| ----- | ----- | ---------- |
| action=purchase | 购买动作 | 必需 |

Method: PUT

Request JSON:

```Json
{
"purchaserId":""
"purchaserName":""
}
```
Response JSON:

成功应答
```Json
{
  status:0,
  msg:"",
  res:{}
}
```
失败应答:
```Json
{
  status:-N,
  msg:"Error message"
}
```

<h3 id='api_update_resource'> 18. 医生竞价资源修改</h3>

<hr/>
Host:  Api Server

Path: /api/v1/resources/:uuid

Params: NULL

Method: PUT

Request JSON:

```Json
{
"type":0,
"ownerId":"5382bf314018f269a1e7cdb0",
"ownerName": "张医生",
"ownerTel": "13100000011",
"timeBegin": 1402000711103,
"timeEnd": 1402100711103,
"price": 1000,
"purchaserId": "",
"purchaserName": "",
"purchaserStatus": 0
}
```
Response JSON:

成功应答
```Json
{
  status:0,
  msg:"",
  res:{}
}
```
失败应答:
```Json
{
  status:-N,
  msg:"Error message"
}
```
