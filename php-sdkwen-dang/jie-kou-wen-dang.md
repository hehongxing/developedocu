# PHP接口说明

### 1.接口工具类

PDF 签章工具类构造方法,该工具类包含所有电子签署相关接口。

#### 1.1工具类原型

```php
public ElecSignService($serverUrl,$port=443)
```

#### 1.2参数说明

| **参数** | **类型** | **约束** | **说明** |
| :---: | :---: | :---: | :--- |
| $ serverUrl | String | 必选 | 人人法电子签署 ip |
| $ port | String | 可选 | 人人法电子签署端口 |

### 2.设置应用帐号

#### 2.1接口原型

```php
public function setAccount($appId,$appSecret)
```

#### 2.2参数说明

| **参数** | **类型** | **约束** | **说明** |
| :---: | :---: | :---: | :--- |
| $appId | String | 必选 | 应用 id |
| $appSecret | String | 必选 | 应用密钥 |

### 3**.查询单个签署文件信息**

#### 3.1接口原型

```php
public function querySingleContract($signSn)
```

#### 3.2参数说明

| **参数** | **类型** | **约束** | **说明** |
| :---: | :---: | :---: | :--- |
| $signSn | String | 必选 | 文件编号 |

#### 3.3返回结果

```
array("statusCode"=>"200",                       //响应状态码
      "statusMsg"=>"success",                    //状态说明
      "signData"=>                               //签署信息,仅在 statusCode为200时有
           array("signSn"=>"",                   //签署文件编号
                 "signedPdf"=>"",                //签署完成文件(base64编码)
                 "signedTime"=>""))              //签署完成时间
```

### 4**.查询文件签署状态**

#### 4.1接口原型

```php
public function querySignState($signSn)
```

#### 4.2参数说明

| **参数** | **类型** | **约束** | **说明** |
| :---: | :---: | :---: | :--- |
| $signSn | String | 必选 | 文件编号 |

#### 4.3返回结果

```
array("statusCode"=>"200",                       //响应状态码
      "statusMsg"=>"success",                    //状态说明
      "signData"=>                               //签署信息,仅在 statusCode为200时有
           array("signSn"=>"",                   //签署文件编号
                 "state"=>""))                   //签署状态
```

### 5**.根据模板或文件完成签署**

#### 5**.1 相关说明**

##### 5.1.1 企业签章

```
1.企业已经申请签章证书完成签署，需提供企业签章编号sealSn，可以设置多个;
```

##### 5.1.2 个人签字

```
1.手写签字，需要签署人在移动设备上进行手写签字从而完成签署，仅支持单个签字;

2.默认签字，签署人无需任何操作 ，人人法根据签署人姓名生成默认签字从而完成签署，可以设置多个。
```

#### 5**.2 设置模板数据**

##### 5.2.1接口原型

```php
public function setTemplate($templateSn,$answer,$underline=false)
```

##### 5.2.2 参数说明

| **参数** | **类型** | **约束** | **说明** |
| :---: | :---: | :---: | :--- |
| $templateSn | String | 必选 | 模板编号 |
| $answer | Array | 必选 | 模板填充数据,二维键值对数组. |
| $underline | boolean | 可选 | 默认为false,填充数据处是否保留下划线 |

##### 5.2.3参数示例

```
$answer=array(array("questionNum"=>"",                     //问题编号
                    "answer"=>"",                          //对应答案
              array("questionNum"=>"",
                    "answer"=>""),
              ……)
```

#### 5.3设置签署信息

##### 5.3.1接口原型

```php
public function setSignData($businessNum,$signType,$notifyUrl="")
```

##### 5.3.2**参数说明**

| **参数** | **类型** | **约束** | **说明** |
| :---: | :---: | :---: | :--- |
| $businessNum | String | 必选 | 签署业务编号,由用户自定义,签署完成后会返回. |
| $signType | int | 必选 | 签署类型:  1 为单位签章\(仅有,没有个人签字\),  2 为个人签字\(仅有,没有单位签章\),  3 为混合签署\(单位签章+个人默认签字\)。 4 为混合签署\(单位签章+个人扫码签字\) |
| $notifyUrl | String | 可选 | 签署完成后的异步通知地址,仅当$signType=4 时需要 |

#### 5.4设置企业签章

**说明：**需提供企业签章编号

##### 5.4.1接口原型

```php
public function setSealsOnCert($sealArray)
```

##### 5.4.2参数说明

| **参数** | **类型** | **约束** | **说明** |
| :---: | :---: | :---: | :--- |
| $sealArray | String | 必选 | 一维或多维数组 |
| companyName | String | 必选 | 企业名称. |
| sealSn | String | 必选 | 企业签章编号. |
| phone | String | 可选 | 待签章公司联系人手机号 |
| keyword | String | 必选 | 企业签章关键字,合同中必需包含此关键字,同时关键字默认为合同文档中倒数搜索到的第一个。 |
| sealWidth | String | 可选 | 生成的合同中,企业签章的宽度,默认为 0,0 为图片实际宽度大小 |
| sealHeight | String | 可选 | 生成的合同中,企业签章的高度,默认为 0,0 为图片实际高度大小 |
| moveSize | String | 可选 | 生成的合同中,企业签章相对于初始位置的左右偏移大小,正数向左偏移,负数向右偏移 |
| heightMoveSize | String | 可选 | 生成的合同中,企业签章相对于初始位置的上下偏移大小,正数向上偏移,负数向下偏移 |
| keywordType | String | 可选 | 合同文档中关键字匹配类型,0 为所有关键字,1 为单个关键字,默认为所有关键字 |
| searchOrder | String | 可选 | 合同文档中关键字的搜索顺序,默认倒序,1:正序、2: 倒序 |
| coverType | String | 可选 | 合同文档中企业印章覆盖类型,默认居右,1:重叠、2: 居下、3:居右 |

##### 5.4.3参数示例

```
$sealArray=array("sealSn"=>"CS8382983229328", //企业签章编号
                 "companyName"=>"中国xx科技有限公司",                   //公司名称
                 "phone"=>"13200000000",                             //手机号
                 "keyword"=>"甲方盖章：",                              //签章关键字
                 "sealWidth"=>"120",                                 //印章图片宽度
                 "sealHeight"=>"120",                                //印章图片高度
                 "moveSize"=>"-50",                                  //印章图片左右偏移
                 "heightMoveSize"=>"20",                             //印章图片上下偏移
                 "keywordType"=>"1",                                 // 关键字类型
                 "searchOrder"=>"1",                                 //关键字搜索顺序
                 "coverType"=>"3");                                  // 印章覆盖类型
```

#### 5.5设置个人签字

##### 5.5.1接口原型

```php
public function setSignatures($signatureArray)
```

##### 5.5.2参数说明

| **参数** | **类型** | **约束** | **说明** |
| :---: | :---: | :---: | :--- |
| $signatureArray | array | 必选 | 个人签字相关数据,为一维或多维键值对数组 |
| name | String | 必选 | 签署人姓名. |
| idcard | String | 必选 | 签署人身份证号. |
| phone | String | 必选 | 签署人手机号 |
| verifyCode | String | 可选 | 手机验证码 。如果加入此参数,必需在此之前调用发送手机验证码 接口给该签署人发送验证码,否则无法完成签署. 仅当 signType=2,3 时有用. |

| isVerifyCode | String | 可选 | 为 ture 或 false,默认为 true, 手写签名过程中是否给签署人发送手机验证码进行验证. 仅当 signType=4 即手写签名时有用 |
| :---: | :---: | :---: | :--- |
| keyword | String | 必选 | 签署关键字,个人签字将会位于此关键字后面,默认从 合同最后开始找到的第一个 |
| sealWidth | String | 可选 | 个人签名的宽度,默认为 0,0 为图片实际宽度大小 |
| sealHeight | String | 可选 | 个人签名的高度,默认为 0,0 为图片实际高度大小 |
| moveSize | String | 可选 | 个人签名相对于初始位置的左右偏移大小,正数向左偏移,负数向右偏移 |
| heightMoveSize | String | 可选 | 个人签名相对于初始位置的上下偏移大小,正数向上偏移,负数向下偏移 |
| keywordType | String | 可选 | 合同文档中关键字匹配类型,0 为所有关键字,1 为单个 关键字,默认为所有关键字 |
| searchOrder | String | 可选 | 合同文档中关键字的搜索顺序,默认倒序,1:正序、2:倒 序 |
| coverType | String | 可选 | 合同文档中个人签名覆盖类型,默认居右,1:重叠、2: 居下、3:居右 |

##### 5.5.3参数示例

```
$signatureArray=array("name"=>"张三",   //签署人姓名
                      "idcard"=>"4211228382838223",
                      "phone"=>"13200000000",                       //手机号
                      "keyword"=>"签字1：",                          //关键字
                      "sealWidth"=>"150",                           //印章图片宽度
                      "sealHeight"=>"30",                           //印章图片高度
                      "moveSize"=>"-50",                            //印章图片左右偏移
                      "heightMoveSize"=>"20",                       //图片上下偏移
                      "keywordType"=>"1",                           // 关键字类型
                      "searchOrder"=>"1",                            //关键字搜索顺序
                      "coverType"=>"3");                            // 印章覆盖类型
```

#### 5.6完成签署

##### 5.6.1基于模板

在调用以上几个接口\(2.8.2~2.8.5\)完成相关参数设置之后,可以调用此接口完成文件的最终签署.

\(1\) 接口原型

```php
public function signContract($pdfSavePath=null);
```

\(2\) 参数说明

| **参数** | **类型** | **约束** | **说明** |
| :---: | :---: | :---: | :--- |
| $pdfSavePath | String | 可选 | 签署完成后的 pdf 文件保存路径 |

\(3\) 返回结果

```
array("statusCode"=>"200",                          //响应状态码
      "statusMsg"=>"success",                       //状态说明
      "signData"=>                                  //签署信息,仅在 statusCode为200时有
           array("businessNum"=>"",                 //签署业务编号
                 "signSn"=>"",                      //签署文件编号
                 "signedPdf"=>"",                   //签署完成后的文件数据(为byte数组)
                 "signedTime"=>""))                 //签署完成时间
```



##### 5.6.2 基于文件

支持pdf,doc,docx,html格式

在调用以上几个接口\(2.8.2~2.8.5\)完成相关参数设置之后,可以调用此接口完成文件的最终签署.

\(1\) 接口原型

```php
public function signContractByFile($filePath,$pdfSavePath=null)
```

\(2\) 参数说明

| **参数** | **类型** | **约束** | **说明** |
| :---: | :---: | :---: | :--- |
| $filePath | String | 必选 | 待签署文件绝对路径 |
| $pdfSavePath | String | 可选 | 签署完成后的 pdf 文件保存路径 |

\(3\) 返回结果

```
array("statusCode"=>"200",                          //响应状态码
      "statusMsg"=>"success",                       //状态说明
      "signData"=>                                  //签署信息,仅在 statusCode为200时有
           array("businessNum"=>"",                 //签署业务编号
                 "signSn"=>"",                      //签署文件编号
                 "signedPdf"=>"",                   //签署完成后的文件数据(为byte数组)
                 "signedTime"=>""))                 //签署完成时间
```

##### 5.6.3基于二维码

仅限一个个人签字.

在调用以上几个接口\(7.1~7.6\)完成相关参数设置之后，可以调用此接口获取个人进行签字的二维码数据，在个人用移动设备扫描二维码进行签名后，便完成了整个文件的签署.

二维码有效期为半个小时,超过这个时间后需要根据返回的编号signSn重新获取二维码数据.

\(1\) 接口原型

```php
public function getSignatureQRcode()
```

\(2\) 返回结果

```
array("statusCode"=>"200",                                             //响应状态码
      "statusMsg"=>"success",                                          //状态说明
      "signSn"=>"SI093933999393939393",                                // 签署编号
      "qrcode"=>"data:image/png;base64,MTgxNDE5MTgxMTRfU0kxNjEyMj")    //二维码数据
```

##### 5.6.4 重新获取二维码

\(1\) 接口原型

```php
public function getSignatureQRcodeAgain($signSn)
```

\(2\) 参数说明

| **参数** | **类型** | **约束** | **说明** |
| :---: | :---: | :---: | :--- |
| $signSn | String | 必选 | 签署编号 |

\(3\) 返回结果

```
array("statusCode"=>"200",                                             //响应状态码
      "statusMsg"=>"success",                                          //状态说明
      "signSn"=>"SI093933999393939393",                                // 签署编号
      "qrcode"=>"data:image/png;base64,MTgxNDE5MTgxMTRfU0kxNjEyMj")    //二维码数据
```

##### 5.6.5 人人法签署完成后异步通知

###### 5.6.5.1 异步通知

**说明：**仅当signType=4有异步通知，当签署完成时，人人法会根据请求参数中的通知地址notifyUrl进行异步通知，收到通知后必须给人人法对应的响应，否则人人法将会在一段时间内不断通知。

**通知数据类型：**JSON

**请求方式：**POST

**\(1\)参数说明：**

| **属性** | **类型** | **说明** |
| :---: | :---: | :--- |
| businessNum | String | 业务编号 |
| signSn | String | 签署业务唯一标志号 |
| signedPdf | String | 签署完成后的文件数据,为 byte数组 |
| signedTime | String | 签署完成时间 |

**\(2\)参数示例:**

```
{"businessNum":"123456789",
 "signSn":"SI160918154516272348",
 "signedPdf":"dsds8f8dss87fd8fd8fdf99",
 "signedTime":"2016-06-06 12:12:12"}
```

###### 5**.6.5.2异步通知响应**

**响应说明：**收到人人法异步通知后，必须给予对应的响应，否则人人法将会在一段时间内不断通知。

**响应数据类型：** String

**响应数据：** success

### 



