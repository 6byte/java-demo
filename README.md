<h1 align="center"><a href="https://github.com/kuaidi100-api/kuadi100-api/" target="_blank">java-demo Project</a></h1>

## Introduce

java-demo 是由[快递100](https://api.kuaidi100.com/home)官方提供的java sdk，方便调试使用。

java-demo 集成了实时查询、订阅推送、智能判断、云打印相关、电子面单相关、短信等接口。

## Features

- 提供了快递100接口请求参数实体类、返回实体类。
- 支持maven集成。
- 提供测试类调试。

## Getting started

java-demo使用和测试可参考[java-demo-test](https://github.com/kuaidi100-api/kuadi100-api/blob/master/src/test/java/BaseServiceTest.java)。

```
# git clone https://github.com/kuaidi100-api/java-demo.git
```

#### Add Config

使用sdk的可以先配置[account.properties](https://github.com/kuaidi100-api/kuadi100-api/blob/master/src/main/resources/account.properties)，账号信息可以登录快递100获取https://poll.kuaidi100.com/manager/page/myinfo/enterprise


### Use Junit Test

```java
public class BaseServiceTest {

    private String key = PropertiesReader.get("key");
    private String customer = PropertiesReader.get("customer");
    private String secret = PropertiesReader.get("secret");
    private String siid = PropertiesReader.get("siid");
    private String userid = PropertiesReader.get("userid");
    private String tid = PropertiesReader.get("tid");
    /**
     * 查询物流轨迹
     */
   @Test
    public void testQueryTrack(){
        QueryTrack queryTrack = new QueryTrack();
        QueryTrackReq queryTrackReq = new QueryTrackReq();
        QueryTrackParam queryTrackParam = new QueryTrackParam();
        queryTrackParam.setCom(CompanyConstant.ST);
        queryTrackParam.setNum("773039762404825");
        queryTrackParam.setPhone("17725390266");
        queryTrackReq.setParam(queryTrackParam);
        queryTrackReq.setCustomer(customer);
        queryTrackReq.setSign(SignUtils.sign(new Gson().toJson(queryTrackParam)+key+customer));
        System.out.println(queryTrack.queryTrack(queryTrackReq));
    }

    /**
     * 订阅
     */
    @Test
    public void testSubscribe(){
        SubscribeReq subscribeReq = new SubscribeReq();
        subscribeReq.setSchema(ApiInfoConstant.SUBSCRIBE_SCHEMA);
        SubscribeParam subscribeParam = new SubscribeParam();
        SubscribeParameters subscribeParameters = new SubscribeParameters();
        subscribeParameters.setCallbackurl("http://www.baidu.com");
        subscribeParam.setParameters(subscribeParameters);
        subscribeParam.setCompany(CompanyConstant.ST);
        subscribeParam.setNumber("773039762404825");
        subscribeParameters.setPhone("17725390266");
        subscribeParam.setKey(key);
        subscribeReq.setParam(subscribeParam);
        Subscribe subscribe = new Subscribe();
        System.out.println(new Gson().toJson(subscribe.subscribe(subscribeReq)));
    }

    /**
     * 智能识别接口（正式用户可以使用）
     */
    @Test
    public void testAutoNum(){
        AutoNum autoNum = new AutoNum();
        System.out.println(autoNum.getFirstComByNum(key,"773039762404825"));
    }

    /**
     * 电子面单图片接口
     */
    @Test
    public void testPrintImg(){
        PrintImgReq printImgReq = new PrintImgReq();
        PrintImgParam printImgParam = new PrintImgParam();
        printImgReq.setKey(key);
        printImgReq.setMethod(ApiInfoConstant.ELECTRONIC_ORDER_PIC_METHOD);

        String t = System.currentTimeMillis() + "";
        printImgReq.setT(t);
        printImgParam.setKuaidicom(CompanyConstant.ZJS);
        printImgParam.setSendManName("张三");
        printImgParam.setSendManMobile("15999566666");
        printImgParam.setSendManPrintAddr("广东省深圳市南山区科技南十二路");
        printImgParam.setRecManName("李四");
        printImgParam.setRecManMobile("15999566666");
        printImgParam.setRecManPrintAddr("北京市海淀区xxx路");
        printImgParam.setType("10");
        printImgParam.setTempid("180c7c8f646742ca871a92c976392b05");
        printImgParam.setCount("1");

        String sign = SignUtils.sign(new Gson().toJson(printImgParam)+t+key+secret);
        printImgReq.setSign(sign);
        printImgReq.setParam(printImgParam);
        PrintImg printImg = new PrintImg();
        System.out.println(new Gson().toJson(printImg.printImG(printImgReq)));
    }

    /**
     * 电子面单html接口
     */
    @Test
    public void testPrintHtml(){
        PrintHtmlReq printHtmlReq = new PrintHtmlReq();
        PrintHtmlParam printHtmlParam = new PrintHtmlParam();
        ManInfo recManInfo  = new ManInfo();
        ManInfo sendManInfo = new ManInfo();
        printHtmlReq.setKey(key);
        printHtmlReq.setMethod(ApiInfoConstant.ELECTRONIC_ORDER_HTML_METHOD);

        String t = System.currentTimeMillis() + "";
        printHtmlReq.setT(t);
        printHtmlParam.setKuaidicom(CompanyConstant.ZJS);
        recManInfo.setName("张三");
        recManInfo.setMobile("15999566666");
        recManInfo.setPrintAddr("广东省深圳市南山区科技南十二路");
        sendManInfo.setName("李四");
        sendManInfo.setMobile("15999566666");
        sendManInfo.setPrintAddr("北京市海淀区xxx路");
        printHtmlParam.setCount("1");
        //如果需要返回电子面单，需要设置
        printHtmlParam.setNeedTemplate("1");
        printHtmlParam.setSendMan(sendManInfo);
        printHtmlParam.setRecMan(recManInfo);
        String sign = SignUtils.sign(new Gson().toJson(printHtmlParam)+t+key+secret);
        printHtmlReq.setSign(sign);
        printHtmlReq.setParam(printHtmlParam);
        PrintHtml printHtml = new PrintHtml();
        System.out.println(new Gson().toJson(printHtml.print(printHtmlReq)));
    }

    /**
     * 电子面单打印
     */
    @Test
    public void testPrintCloud(){
        PrintCloudReq printCloudReq = new PrintCloudReq();
        PrintCloudParam printCloudParam = new PrintCloudParam();
        ManInfo recManInfo  = new ManInfo();
        ManInfo sendManInfo = new ManInfo();
        printCloudReq.setKey(key);
        printCloudReq.setMethod(ApiInfoConstant.ELECTRONIC_ORDER_PRINT_METHOD);

        String t = System.currentTimeMillis() + "";
        printCloudReq.setT(t);
        printCloudParam.setKuaidicom(CompanyConstant.ZJS);
        recManInfo.setName("张三");
        recManInfo.setMobile("15999566666");
        recManInfo.setPrintAddr("广东省深圳市南山区科技南十二路");
        sendManInfo.setName("李四");
        sendManInfo.setMobile("15999566666");
        sendManInfo.setPrintAddr("北京市海淀区xxx路");
        printCloudParam.setCount("1");
        printCloudParam.setSiid(siid);
        printCloudParam.setTempid("180c7c8f646742ca871a92c976392b05");
        printCloudParam.setSendMan(sendManInfo);
        printCloudParam.setRecMan(recManInfo);
        String sign = SignUtils.sign(new Gson().toJson(printCloudParam)+t+key+secret);
        printCloudReq.setSign(sign);
        printCloudReq.setParam(printCloudParam);
        PrintCloud printCloud = new PrintCloud();
        System.out.println(new Gson().toJson(printCloud.print(printCloudReq)));
    }

    /**
     * 云打印自定义
     */
    @Test
    public void testCloudCustom() throws Exception{
        PrintBaseReq<CloudPrintCustomParam> printCustomParamReq = new PrintBaseReq<CloudPrintCustomParam>();
        CloudPrintCustomParam cloudPrintCustomParam = new CloudPrintCustomParam();
        cloudPrintCustomParam.setSiid(siid);
        cloudPrintCustomParam.setCallBackUrl("http://www.baidu.com");
        cloudPrintCustomParam.setTempid("180c7c8f646742ca871a92c976392b05");
        String t = System.currentTimeMillis() + "";
        printCustomParamReq.setKey(key);
        printCustomParamReq.setMethod(ApiInfoConstant.CLOUD_PRINT_CUSTOM_METHOD);
        printCustomParamReq.setT(t);
        printCustomParamReq.setSign(SignUtils.sign(new Gson().toJson(cloudPrintCustomParam)+t+key+secret));
        printCustomParamReq.setParam(cloudPrintCustomParam);
        CloudPrintCustom cloudPrintService = new CloudPrintCustom();
        System.out.println(cloudPrintService.print(printCustomParamReq));
    }

    /**
     * 云打印附件
     */
    @Test
    public void testCloudAttachment() throws Exception{
        CloudPrintAttachmentReq cloudPrintAttachmentReq = new CloudPrintAttachmentReq();
        CloudPrintAttachmentParam cloudPrintAttachmentParam = new CloudPrintAttachmentParam();
        cloudPrintAttachmentParam.setSiid(siid);
        cloudPrintAttachmentParam.setCallBackUrl("http://www.baidu.com");
        String t = System.currentTimeMillis() + "";
        cloudPrintAttachmentReq.setKey(key);
        cloudPrintAttachmentReq.setMethod(ApiInfoConstant.CLOUD_PRINT_ATTACHMENT_METHOD);
        cloudPrintAttachmentReq.setT(t);
        cloudPrintAttachmentReq.setSign(SignUtils.sign(new Gson().toJson(cloudPrintAttachmentParam)+t+key+secret));
        cloudPrintAttachmentReq.setParam(cloudPrintAttachmentParam);
        cloudPrintAttachmentReq.setFile(new File("C:\\Users\\Administrator.-20171106WFEKLN\\Desktop\\1.png"));
        CloudPrintAttachment cloudPrintAttachment = new CloudPrintAttachment();
        System.out.println(cloudPrintAttachment.print(cloudPrintAttachmentReq));
    }

    /**
     * 复打
     */
    @Test
    public void testCloudPrintOld() throws Exception{
        PrintBaseReq<CloudPrintOldParam> printCustomParamReq = new PrintBaseReq<CloudPrintOldParam>();
        CloudPrintOldParam cloudPrintOldParam = new CloudPrintOldParam();
        cloudPrintOldParam.setTaskId("2B70DF2F433F64C4A8C6F23DD50368DC");
        String t = System.currentTimeMillis() + "";
        printCustomParamReq.setKey(key);
        printCustomParamReq.setMethod(ApiInfoConstant.CLOUD_PRINT_OLD_METHOD);
        printCustomParamReq.setT(t);
        printCustomParamReq.setSign(SignUtils.sign(new Gson().toJson(cloudPrintOldParam)+t+key+secret));
        printCustomParamReq.setParam(cloudPrintOldParam);
        CloudPrintOld cloudPrintService = new CloudPrintOld();
        System.out.println(cloudPrintService.print(printCustomParamReq));
    }

    /**
     * 发送短信
     */
    @Test
    public void testSendSms(){
        SendSms sendSms = new SendSms();
        SendSmsReq sendSmsReq = new SendSmsReq();
        sendSmsReq.setCallback("http://www.baidu.com");
        Map<String,String> content = new HashMap<String, String>();
        content.put("username","测试用户");
        sendSmsReq.setContent(new Gson().toJson(content));
        sendSmsReq.setPhone("15999523333");
        sendSmsReq.setSeller("贵司名称");
        sendSmsReq.setUserid(userid);
        sendSmsReq.setTid(tid);
        sendSmsReq.setSign(SignUtils.sign(key+userid));
        System.out.println(sendSms.sendSms(sendSmsReq));
    }
}
```

## FAQ

### 问题1.快递100api接口里sign该如何处理？

快递100加密方式统一为MD5后转大写，可以参考[签名](https://github.com/kuaidi100-api/kuadi100-api/blob/master/src/main/java/com/kuaidi100/sdk/utils/SignUtils.java)

```java
org.apache.commons.codec.digest.DigestUtils.md5Hex(msg).toUpperCase();
```

### 问题2.使用了快递100订阅接口后，该如何处理回调接口？

可以参考推送[订阅回调](https://github.com/kuaidi100-api/kuadi100-api/blob/master/src/main/java/com/kuaidi100/sdk/api/Subscribe.java#L56)

```java
public SubscribeResp callBackUrl(HttpServletRequest request){
        String param = request.getParameter("param");
        String sign = request.getParameter("sign");
        //建议记录一下这个回调的内容，方便出问题后双方排查问题
        log.debug("快递100订阅推送回调结果|{}|{}",param,sign);
        //订阅时传的salt,没有可以忽略
        String salt = "";
        String ourSign = SignUtils.sign(param + salt);
        SubscribeResp subscribeResp = new SubscribeResp();
        subscribeResp.setResult(Boolean.TRUE);
        subscribeResp.setReturnCode("200");
        subscribeResp.setMessage("成功");
        //加密如果相等，属于快递100推送；否则可以忽略掉当前请求
        if (ourSign.equals(sign)){
            //TODO 业务处理
            return subscribeResp;
        }
        return null;
    }
```

### 问题3.使用电子面单图片接口，该如何处理返回的base64图片？

返回的是一个字符串json数组，多个子单时会有多个。

添加前缀“`data:image/png;base64,`”，把“`\\\\n`”替换成“”；可以参考[处理返回的base64字符串](https://github.com/kuaidi100-api/kuadi100-api/blob/master/src/main/java/com/kuaidi100/sdk/api/PrintImg.java#L47)

```java
public List<String> getBase64Img(String imgBase64){
        List<String> stringList= new Gson().fromJson(imgBase64,
                                 new TypeToken<List<String>>() {}.getType());
        List<String> base64Img = new ArrayList<String>();
        if (stringList != null && stringList.size()>0){
            for (String s : stringList) {
                s = "data:image/png;base64,"+s.replace("\\\\n","");
                base64Img.add(s);
            }
        }
        return base64Img;
    }
```

