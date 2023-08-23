---
title: 在Okhttp中使用HTTPDNS
author: xfans
type: post
date: 2018-01-10T12:06:27+00:00
url: /archives/63
views:
  - 1206
categories:
  - Android
tags:
  - dns
  - okhttp

---
## 背景

用户经常在国外使用App，国外的网络环境更加复杂，经常会出现UnknownHostException的异常，无法解析DNS。

## 解决方案

当遇到该异常时就需要自己去解析DNS。常用的自己解析DNS的方式有两种

### 1.自己拿到IP后替换域名（App老的方式）

比如使用dnsjava拿到IP后需要：

<ol start="1.">
  <li>
    对请求的域名进行替换
  </li>
  <li>
    对Header中的Host进行替换
  </li>
  <li>
    对Cookie中的domain进行替换
  </li>
  <li>
    Https下证书校验问题
  </li>
  <li>
    .……
  </li></ol start="1."> 
  
  <p>
    使用起来比较麻烦，问题较多。
  </p>
  
  <h3>
    2.使用Okhttp自定义DNS功能
  </h3>
  
  <p>
    目前刚把App从android-async-http切换到OKhttp。(PS:花了一周多时间，改到吐血)，可以直接使用Okhttp自定义dns功能。
  </p>
  
  <p>
    DNS的解析方式选择了HTTPDNS，HTTPDNS直接使用 HTTP协议去查询DNS，避开了传统UDP方式DNS的DNS劫持，不准确等弊端。
  </p>
  
  <p>
    详见：<a href="【鹅厂网事】全局精确流量调度新思路-HttpDNS服务详解" title="【鹅厂网事】全局精确流量调度新思路-HttpDNS服务详解 ">【鹅厂网事】全局精确流量调度新思路-HttpDNS服务详解 </a>
  </p>
  
  <h3>
    3.Okhttp自定义DNS
  </h3>
  
  <p>
    HTTPDNS选择了<a href="https://www.dnspod.cn/httpdns" title="DNSpod">DNSpod</a>和<a href="https://developers.google.com/speed/public-dns/docs/dns-over-https" title="Google Public DNS">Google Public DNS</a>两家。
  </p>
  
  <p>
    在国外采用Google Public DNS
  </p>
  
  <p>
    在国内采用DNSpod
  </p>
  
  <p>
    实现方式如下：
  </p>
  
  <pre class="line-numbers prism-highlight" data-start="1"><code class="language-java">public class OkDns implements Dns {
    private String mIps;
    private long mLastDnsTime;
    private static final long DNS_TTL = 600 * 1000;//自定义DNS缓存时间
    @Override
    public List&lt;InetAddress&gt; lookup(String hostname) throws UnknownHostException {
        try {
            List&lt;InetAddress&gt; list = Dns.SYSTEM.lookup(hostname);
            return list;
        } catch (Exception e) {
            if (System.currentTimeMillis() - mLastDnsTime &gt; DNS_TTL) {//超过ttl 时间就重新请求dns
                mIps = null;
            }
            if (TextUtils.isEmpty(mIps)) {
                mIps = HttpDns.getInstance().getIp(hostname);
                mLastDnsTime = System.currentTimeMillis();
            }
            if (!TextUtils.isEmpty(mIps)) {
                return Arrays.asList(InetAddress.getAllByName(mIps));
            } else {
                return Dns.SYSTEM.lookup(hostname);
            }
        }
    }
}
</code></pre>
  
  <pre class="line-numbers prism-highlight" data-start="1"><code class="language-java">okHttpClient = new OkHttpClient.Builder().dns(new OkDns())
</code></pre>
  
  <h3>
    4.注意
  </h3>
  
  <p>
    代码中没有默认采用HTTPDNS的方式，当出错后才会使用HTTPDNS。代码只为说明，非实际代码。
  </p>