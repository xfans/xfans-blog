---
title: 快速的将Android-async-http升级到OKhttp
author: xfans
type: post
date: 2018-02-24T03:30:45+00:00
url: /archives/148
views:
  - 746
categories:
  - Android
tags:
  - okhttp

---
## 背景

Android-async-http已经不更新了。ps:还为这个库提交过功能<https://github.com/android-async-http/android-async-http/pull/840>

Android-async-http是基于HttpClient的。

## 替换方案

将项目中所有的请求代码都替换工作量巨大，也容易出问题。所以采用将Okhttp封装为Android-async-http的方式进行替换。

android-async-http的使用方法:

<pre class="wp-block-preformatted"><code>AsyncHttpClient client = &lt;strong>new &lt;/strong>AsyncHttpClient();
String url = &lt;strong>"http://xfans.me/api/v1/list"&lt;/strong>;
client.get(url, &lt;strong>new &lt;/strong>AsyncHttpResponseHandler() {
    @Override
    &lt;strong>public void &lt;/strong>onSuccess(&lt;strong>int &lt;/strong>statusCode, Header[] headers, &lt;strong>byte&lt;/strong>[] responseBody) {
    }
    @Override
    &lt;strong>public void &lt;/strong>onFailure(&lt;strong>int &lt;/strong>statusCode, Header[] headers, &lt;strong>byte&lt;/strong>[] responseBody, Throwable error) {
    }
});</code></pre>

从代码可以看出,想要快速的替换需要:

1.使用okhttp来实现`AsyncHttpClient`

2.实现`AsyncHttpResponseHandler`,`FileAsyncHttpResponseHandler`,`JsonHttpResponseHandler`,`TextHttpResponseHandler`等项目中使用的Handler即可.