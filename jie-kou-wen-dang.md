# 接口说明

#### 1.客户端消息类 :ClientMessage

调用相关接口后, 对应人人法响应的数据均封装在此消息类中, 包括响应状态码,响应提示信息。

**消息类属性:**

```
  private String statusCode;         //返回状态码
  private String statusInfo;         //返回状态信息
  private String reqJsonBody;        //请求源数据
  private String respJsonBody;       //响应源数据
  private Sign sign;                 //客户端签名对象
  private List<Sign> signList;       //客户端签名对象列表
```

#### 2. 客户端签署对象:Sign

> 此对象封装了人人法响应的相关签署的数据.

> **签署对象属性:**

```
privateString signSn;          //签署唯一编号(人人法服务器生成)
privateString businessNum;     //签署唯一业务编号(用户服务器生成)
privatebyte[]signedPdf;        //签署完成后的 pdf 文件
privateint state;              //签署状态
privateStringqrcode;           //签署二维码数据
privateString creatTime;       //签署创建时间
privateString signTime;        //签署时间
```

#### 3.客户端签署接口服务类:ElecSignService

包含电子签署相关的所有接口,即3.2.4, 3.2.5, 3.2.6 的所有接口

#### 4.客户端签署相关接口

##### 4.1 设置应用帐号 :setAccount

**功能简介**: 设置应用帐号。

**原型**: public void setAccount\(String appId, String appSecret\)

**参数说明**:![](/assets/5.png)

##### 4.2 设置合同模板数据 :setTemplate

**功能简介**: 设置合同模板数据。

**原型: **public voidsetTemplate\(String templateSn, String\[\]answers,boolean underline, Map&lt;Integer, String\[\]&gt; addTableContent\)

**参数说明:**

| 参数 | 类型 | 约束 | 说明 |
| :---: | :---: | :---: | :---: |
| templateSn | String | 必选 | 模板编号,用于人人法调用对应的合同模板。 |
| answers | String\[\] | 可空 | 默认为 null,模板填充数据,这些数据会填充到对 应的合同模板中,从而生成相对应的合同。 |
| underline | boolean | 必选 | 默认为 flase,生成的合同中, 未填充数据处是否保 留下划线 |
| addTableContent | Map&lt;Integ er, String\[\]&gt; | 可空 | 默认为 null,表格填充数据 |

**参数示例:**

`String templateSn= "CT17040611593771672859858E5BD29D";`

`String[]answers= {"ZTTZ-10000703","2017 年 01 月 12 日","2017 年 03 月 12 日","张三"};`

`boolean underline=false;`

##### 4.3 设置签署数据 :setSignData

**功能简介: **设置此次签署相关数据。

**原型: **public void setSignData\(String businessNum,int signType, String notifyUrl\)

**参数说明:**

| 参数 | 类型 | 约束 | 说明 |
| :--- | :--- | :--- | :--- |
| businessNum | String | 必选 | 签署唯一业务编号,由用户自己生成,合同签署完成后人人法会将此编号传回。 |
| signType | int | 必选 | 签署类型, signType=1:仅有企业签章。 signType=2:仅有个人默认签名。 signType=3:企业签章加个人默认签名。 signType=4:企业签章加个人手写签名。 |
| notifyUrl | String | 可空 | 签署完成后的回调地址,仅在 signType=4 时有效,即当signType=4 需要在移动设备上手签时必需提 供。 |

**参数示例:**

`String businessNum="JKHT20161111";`

`int signType=3;`

`String notifyUrl="https://www.renrenlaw.com/sign/notify";`

##### 4.4 设置企业签章\(基于证书\):setSealsOnCert

**功能简介:**设置企业签章数据,已经申请了企业签章证书。

**原型: **public void setSealsOnCert\(List&lt;Map&lt;String,Object&gt;&gt; sealList\)

**参数说明:**

| 参数 | 类型 | 约束 | 说明 |
| :--- | :--- | :--- | :--- |
| companyName | String | 必选 | 签章企业名称。 |
| sealSn | String | 必选 | 企业签章编号,仅在企业基于已经申请的签章证书进行签署时有效,人人法根据此编号调用对应的签章和证书。 |
| keyword | String | 必选 | 企业签章关键字,合同中必需包含此关键字,同时关键字默认为合同文档中倒数搜索到的第一个。 |
| sealWidth | String | 可选 | 生成的合同中,企业签章的宽度,默认为 0,0 为图片实际宽度大小 |
| sealHeight | String | 可选 | 生成的合同中,企业签章的高度,默认为 0,0 为图片实际高度大小 |
| moveSize | String | 可选 | 生成的合同中,企业签章相对于初始位置的左右偏移大小,正数向左偏移,负数向右偏移 |
| heightMoveSize | String | 可选 | 生成的合同中,企业签章相对于初始位置的上下偏移大小,正数向上偏移,负数向下偏移 |
| keywordType | String | 可选 | 合同文档中关键字匹配类型,0 为所有关键字,1 为 单个关键字,默认为所有关键字 |
| searchOrder | String | 可选 | 合同文档中关键字的搜索顺序,默认倒序,1:正序、 2:倒序 |
| coverType | String | 可选 | 合同文档中企业印章覆盖类型,默认居右,1:重叠、 2:居下、3:居右 |

**参数示例:**

`String sealSn="CS17021416183335921148158A2BD598";`

`List<Map<String,Object>> certSealList=new ArrayList<Map<String,Object>>();`

`Map<String,Object> certSeal=new TreeMap<String,Object>();`

`certSeal.put("companyName","北京人人法科技有限公司");`

`certSeal.put("sealSn", sealSn);`

`certSeal.put("keyword","丙方盖章:");`

`certSeal.put("sealWidth","150");`

`certSeal.put("sealHeight","150");`

`certSeal.put("moveSize","-200");`

`certSeal.put("heightMoveSize","100");`

`certSeal.put("keywordType","1");`

`certSeal.put("searchOrder","2");`

`certSeal.put("coverType","2");`

`certSealList.add(certSeal);`

##### 4.5 设置个人签字:setSignatures

**功能简介:** 设置个人签字数据。

**原型:** public void setSignatures\(List&lt;Map&lt;String,Object&gt;&gt; signatureList\)

**参数说明:**

| 参数 | 类型 | 约束 | 说明 |
| :--- | :--- | :--- | :--- |
| phone | String | 可选 | 签署人手机号,当 signType=4即需要手写签名时必需提供。 |
| name | String | 必选 | 签署人真实姓名。 |
| idcard | String | 可选 | 签署人身份证号,建议提供。 |
| keyword | String | 必选 | 个人签名关键字 合同中必需包含此关键字,,同时关键字默认为合 同文档中倒数搜索到的第一个。 |
| verifyCode | String | 可选 | 手机验证码 如果加入此参数,必需在此之前调用发送手机验证 码接口给该签署人发送验证码,否则无法完成签署,仅当 signType=2,3 时有用. |
| isVerifyCode | String | 可选 | 为 ture 或 false,默认为 true 签名过程中是否给签署人发送手机验证码进行验证,仅当 signType=4 即手写签名时有用 |

| sealWidth | String | 可选 | 个人签名的宽度,默认为 0,0 为图片实际宽度大小 |
| :--- | :--- | :--- | :--- |
| sealHeight | String | 可选 | 个人签名的高度,默认为 0,0 为图片实际高度大小 |
| moveSize | String | 可选 | 个人签名相对于初始位置的左右偏移大小,正数向 左偏移,负数向右偏移 |
| heightMoveSize | String | 可选 | 个人签名相对于初始位置的上下偏移大小,正数向上偏移,负数向下偏移 |
| keywordType | String | 可选 | 合同文档中关键字匹配类型,0 为所有关键字,1 为 单个关键字,默认为所有关键字 |
| searchOrder | String | 可选 | 合同文档中关键字的搜索顺序,默认倒序,1:正序、 2:倒序 |
| coverType | String | 可选 | 合同文档中个人签名覆盖类型,默认居右,1:重叠、 2:居下、3:居右 |

**参数示例:**

`List<Map<String,Object>> signatureList=new ArrayList<Map<String,Object>>();`

`Map<String,Object> signatureMap=new TreeMap<String,Object>();`

`signatureMap.put("name","张三");`

`signatureMap.put("idcard","421122188809298288383");`

`signatureMap.put("phone","17600000000");`

`signatureMap.put("keyword","甲方(出借人)签字");`

`signatureMap.put("sealWidth","150");`

`signatureMap.put("sealHeight","30");`

`signatureMap.put("moveSize","-450");`

`signatureMap.put("heightMoveSize","70");`

`signatureMap.put("keywordType","1");`

`signatureMap.put("searchOrder","1");`

`signatureMap.put("coverType","1");`

`signatureList.add(signatureMap);`

##### 4.6 完成签署 — 基于模板:signContractByTemplate

**功能简介:** 电子签署接口,基于模板完成签署。

**原型: **public ClientMessage signContractByTemplate\(String pdfSavePath\)

**参数说明:**

| 参数 | 类型 | 约束 | 说明 |
| :--- | :--- | :--- | :--- |
| pdfSavePath | String | 可选 | 签署完成后的 pdf 文件保存路径,默认为 null |

##### 4.7 完成签署 — 基于文件: signContractByFile

**功能简介: **电子签署接口,基于文件完成签署。

**原型: **public ClientMessage signContractByFile\(String filePath ,String pdfSavePath\)

**参数说明:**

| 参数 | 类型 | 约束 | 说明 |
| :--- | :--- | :--- | :--- |
| filePath | String | 必选 | 待签署文件绝对路径,支持 doc,docx,pdf 和 html 格式 |
| pdfSavePath | String | 可选 | 签署完成后的 pdf 文件保存路径,默认为 null |

##### 4.8 完成签署 — 基于签署二维码:getSignatureQRcode

**功能简介:** 电子签署接口,基于生成的二维码,用户利用移动设备扫码进入完成签署。

**原型:**public ClientMessage getSignatureQRcode\(\)

**返回数据:**

调用接口后,会得到二维码的 base64 编码的数据,例如:

`data:image/png;base64,R0lGODlhAwADAIABAL6+vv///yH5BAEAAAEALAAA`

使用:

`<img src="data:image/png;base64,R0lGODlhAwADAIABAL" />`

二维码有效期为半个小时,超过半小时没有完成签署需要重新获取

##### 4.9 完成签署 —重新获取签署二维码:getSignatureQRcodeAgain

**功能简介:** 电子签署接口,基于生成的二维码,用户利用移动设备扫码进入完成签署。

**原型:**public ClientMessagegetSignatureQRcodeAgain\(StringsignSn\)

**参数说明:**

| 参数 | 类型 | 约束 | 说明 |
| :--- | :--- | :--- | :--- |
| signSn | String | 必选 | 签署唯一编号 |

#### 5. 客户端查询接口

##### 5.1 查询签署状态 :getSignState

**功能简介:** 查询合同的签署状态。

**原型:**public ClientMessage getSignState\(String signSn\)

**参数说明:**

| 参数 | 类型 | 约束 | 说明 |
| :--- | :--- | :--- | :--- |
| signSn | String | 必选 | 合同签署的唯一编号 |

##### 5.2查询单个合同:getSingleContract

**功能简介: **查询单个合同。

**原型:**public ClientMessagegetSingleContract\(String signSn\)

**参数说明:**

| 参数 | 类型 | 约束 | 说明 |
| :--- | :--- | :--- | :--- |
| signSn | String | 必选 | 合同签署的唯一编号 |

#### 6. 其他接口

##### 6.1 发送签署验证码 :sendSignVerifyCode

**功能简介: **签署前,发送手机验证码,调用签署接口完成签署时必需传入此验证码。原型:

`public ClientMessage sendSignVerifyCode(List<Map<String,Object>>phoneAndCodeList)`

**参数说明:**

| 参数 | 类型 | 约束 | 说明 |
| :--- | :--- | :--- | :--- |
| phone | String | 必选 | 手机号 |
| verifyCode | String | 必选 | 需要发送的验证码,必须为 6位整数 |

**参数示例:**

`List<Map<String,Object>> codeList=new ArrayList<>();`

`Map<String,Object> codeMap=newHashMap<String,Object>();`

`codeMap.put("phone","18100000000");`

`codeMap.put("verifyCode","938720");`

`codeList.add(codeMap);`

