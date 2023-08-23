---
title: Flutter cached_network_image使用Dio请求图片
author: xfans
type: post
date: 2021-06-25T10:19:06+00:00
url: /archives/365
categories:
  - Flutter

---
## 背景

在Flutter中通常使用cached\_network\_image控件显示图片，该控件使用http库请求图片，http库无法像dio一样方便的添加拦截器等。

## 实现

CachedNetworkImage使用flutter\_cache\_manager进行图片缓存，可以使用自定义cache_manager来替换http为dio。 如下：

<pre class="wp-block-preformatted">class DioCacheManager extends CacheManager {
  static const <em>key </em>= 'dioCache';

  static DioCacheManager <em>_instance</em>;

  factory DioCacheManager(Dio dio) {
    if (<em>_instance </em>== null) {
      <em>_instance </em>= DioCacheManager._(dio);
    }
    return <em>_instance</em>;
  }

  DioCacheManager._(Dio dio)
      : super(Config(<em>key</em>, fileService: DioHttpFileService(dio)));
}</pre>

<pre class="wp-block-preformatted">class DioHttpFileService extends FileService {
  Dio _dio;

  DioHttpFileService(this._dio);

  @override
  Future&lt;FileServiceResponse&gt; get(String url,
      {Map&lt;String, String&gt; headers}) async {
    Options options =
        Options(headers: headers ?? {}, responseType: ResponseType.stream);

    Response&lt;ResponseBody&gt; httpResponse =
        await _dio.get&lt;ResponseBody&gt;(url, options: options);

    return DioGetResponse(httpResponse);
  }
}

class DioGetResponse implements FileServiceResponse {
  final DateTime _receivedTime = DateTime.now();
  Response&lt;ResponseBody&gt; _response;

  DioGetResponse(this._response);

  @override
  Stream&lt;List&lt;int&gt;&gt; get content =&gt; _response.data.stream;

  @override
  int get contentLength =&gt; _getContentLength();

  @override
  String get eTag =&gt; _response.headers['etag']?.first ?? '-1';

  @override
  String get fileExtension {
    var fileExtension = '';
    final contentTypeHeader =
        _response.headers[Headers.<em>contentTypeHeader</em>]?.first;
    if (contentTypeHeader != null) {
      final contentType = ContentType.<em>parse</em>(contentTypeHeader);
      fileExtension = mimeTypes[contentType.mimeType];
    }
    return fileExtension;
  }

  @override
  int get statusCode =&gt; _response.statusCode;

  @override
  DateTime get validTill {
    // Without a cache-control header we keep the file for a week
    var ageDuration = const Duration(days: 7);
    final controlHeader = _response.headers['cache-control']?.first;
    if (controlHeader != null) {
      final controlSettings = controlHeader.split(',');
      for (final setting in controlSettings) {
        final sanitizedSetting = setting.trim().toLowerCase();
        if (sanitizedSetting == 'no-cache') {
          ageDuration = const Duration();
        }
        if (sanitizedSetting.startsWith('max-age=')) {
          var validSeconds = int.<em>tryParse</em>(sanitizedSetting.split('=')[1]) ?? 0;
          if (validSeconds &gt; 0) {
            ageDuration = Duration(seconds: validSeconds);
          }
        }
      }
    }

    return _receivedTime.add(ageDuration);
  }

  int _getContentLength() {
    try {
      return int.<em>parse</em>(
          _response.headers[Headers.<em>contentLengthHeader</em>]?.first ?? '-1');
    } catch (e) {
      return -1;
    }
  }
}</pre>

## 使用方法

<pre class="wp-block-preformatted">dependencies:
  flutter_cache_manager_dio: ^2.0.0</pre>

<pre class="wp-block-preformatted">CachedNetworkImage(
    cacheManager: DioCacheManager(dio),
    imageUrl: url,
    placeholder: (context, url) =&gt; CircularProgressIndicator(),
    errorWidget: (context, url, error) =&gt; Container(
    child: Text(
        'error',
        ),
    ),
);</pre>

然后就可以使用dio的拦截等功能了，比如使用拦截器添加并刷新token，添加log等功能。

## 地址

<a rel="noreferrer noopener" href="https://pub.dev/packages/flutter_cache_manager_dio" target="_blank">https://pub.dev/packages/flutter_cache_manager_dio</a>

<a rel="noreferrer noopener" href="https://github.com/xfans/flutter_cache_manager_dio" target="_blank">https://github.com/xfans/flutter_cache_manager_dio</a>