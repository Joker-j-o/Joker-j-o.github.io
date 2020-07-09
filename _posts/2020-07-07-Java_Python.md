---
layout:     post                    # 使用的布局（不需要改）
title:      Java_Python           # 标题 
subtitle:   Joker #副标题
date:       2020-07-07              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [python, django]
---

#  <font color="red">AsinSeedApi</font>

###  <font color="blue">不写注释的程序员-加密</font>

###  <font color="blue">将JAVA API接口 改写成 Python</font>

###  <font color=" \#7B68EE ">JAVA</font>

``` java
import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONArray;
import com.alibaba.fastjson.JSONObject;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.DefaultHttpClient;
import org.apache.http.util.EntityUtils;

import java.io.IOException;
import java.security.MessageDigest;

public class AsinSeedApiTest {
    private static final char[] HEX_CHAR = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f'};
    private static final String PARTNER_CODE = "xxx";
    private static final String PARTNER_SECRET_KEY = "xxx-xxx-xxx-xxx";

    private static String byteArrayToHexString(byte[] byteArray) {
        StringBuffer resultSb = new StringBuffer();
        for (int i = 0; i < byteArray.length; i++) {
            int n = byteArray[i];
            n = n < 0 ? n + 256 : n;
            resultSb.append(HEX_CHAR[n / 16]).append(HEX_CHAR[n % 16]);
        }
        return resultSb.toString();
    }

    private static String encode(String origin) {
        String output = null;
        try {
            MessageDigest md = MessageDigest.getInstance("MD5");
            output = byteArrayToHexString(md.digest(origin.getBytes("UTF-8")));
        } catch (Throwable e) {
            e.printStackTrace();
        }
        return null != output ? output.substring(0, 12) : "";
    }

    public static void main(String[] args) {
        String marketplace = "US";
        String asin = "B017H39S5U";
        String partner = PARTNER_CODE;
        String qid = marketplace + "_" + asin + "_" + PARTNER_SECRET_KEY;
        qid = encode(qid);
        String urlStr = "https://www.asinseed.com/api/" + marketplace + "/" + asin + "/" + partner + "/" + qid;
        HttpClient httpClient = new DefaultHttpClient();
        HttpGet httpGet = new HttpGet(urlStr);
        try {
            HttpResponse response = httpClient.execute(httpGet);
            HttpEntity responseEntity = response.getEntity();
            if (null != responseEntity) {
                String responseJsonStr = EntityUtils.toString(responseEntity, "UTF-8");
                JSONObject result = JSON.parseObject(responseJsonStr);
                String code = result.getString("code");
                if ("ok".equalsIgnoreCase(code)) {
                   JSONObject data = result.getJSONObject("data");
                   JSONArray keywords = data.getJSONArray("keywords");
                   //TODO your custom business logic
                   System.out.println(keywords.toJSONString());
                } else {
                   String errMsg = result.get("message").toString();
                   //TODO handle the error msg from api
                   System.out.println("errorMs=>" + errMsg);
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

###  <font color=" \#7B68EE ">Python</font>

``` python
class AsinSeedApiTest:
    HEX_CHAR = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f']
    PARTNER_CODE = PARTNER_CODE
    PARTNER_SECRET_KEY = PARTNER_SECRET_KEY

    @classmethod
    def byteArrayToHexString(cls, byteArray):
        resultSb = ''
        for n in byteArray:
            n = n + 256 if n < 0 else n
            resultSb += (cls.HEX_CHAR[n // 16])
            resultSb += (cls.HEX_CHAR[n % 16])
        return resultSb

    @classmethod
    def encode(cls, origin):
        m = hashlib.md5()
        m.update(origin.encode('utf-8'))
        output = m.hexdigest()
        return output[0:12] if output else ""

    @classmethod
    def main(cls, marketplace, asin):
        marketplace = marketplace
        asin = asin
        partner = cls.PARTNER_CODE
        qid = marketplace + "_" + asin.upper() + "_" + cls.PARTNER_SECRET_KEY  # ASIN
        qid = cls.encode(qid)
        urlStr = "https://www.asinseed.com/api/" + marketplace + "/" + asin + "/" + partner + "/" + qid  # ASIN url
        keyWord_urlStr = "https://www.asinseed.com/api/" + marketplace + "/" + partner + "/" + qid + '?keyword=' + asin  # 关键词 url

        return urlStr, keyWord_urlStr

```
