---
title: scala 使用 org.apache.http 4.5.2 访问 https 地址忽略证书
date: 2018-09-17 10:49:23
tags: ["tech","技术"]
author: wangxiuwen
categories: ["技术"]
layout: post
---

问题：
```
Caused by: sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
```

原因:

>我们只能找到CA机构的指纹，而找不到中间证书的指纹，所以在Java中无法验证整条证书链的有效性，所以导致Java程序在通过HTTPS协议访问浏览器访问正常的域名时发生证书错误。通常的解决办法是在Http Client端设置忽略证书错误，或是将缺少的中间证书导入Java keystore

插曲：

`Httpclient SSLContextBuilder deprecated`

原因：
>SSLContextBuilder从原来的org.apache.http.conn.ssl 包挪到了org.apache.http.ssl 包

代码：

```
  def createSSLClientDefault: CloseableHttpClient = {
    try {
      val sslContext = new SSLContextBuilder().loadTrustMaterial(null, new TrustStrategy {
        override def isTrusted(x509Certificates: Array[X509Certificate], s: String): Boolean = true
      }).build()

      val sslsf = new SSLConnectionSocketFactory(sslContext)
      return HttpClients.custom.setSSLSocketFactory(sslsf).build
    } catch {
      case e: KeyManagementException =>
        LOGGER.error(e.getMessage, e)
      case e: NoSuchAlgorithmException =>
        LOGGER.error(e.getMessage, e)
      case e: KeyStoreException =>
        LOGGER.error(e.getMessage, e)
    }
    HttpClients.createDefault
  }
```


完整代码：


```

	import java.io.IOException
	import java.net.URLEncoder
	import java.security.cert.X509Certificate
	import java.security.{KeyManagementException, KeyStoreException, NoSuchAlgorithmException}
	import java.util
	
	import com.alibaba.fastjson.JSON.toJSONString
	import com.google.common.base.Preconditions
	import com.google.common.collect.{ImmutableMap, Lists}
	import org.apache.http.client.ResponseHandler
	import org.apache.http.client.config.RequestConfig
	import org.apache.http.client.entity.UrlEncodedFormEntity
	import org.apache.http.client.methods.{HttpGet, HttpPost}
	import org.apache.http.conn.ssl.{SSLConnectionSocketFactory, TrustStrategy}
	import org.apache.http.impl.client.{CloseableHttpClient, HttpClients}
	import org.apache.http.message.BasicNameValuePair
	import org.apache.http.ssl.SSLContextBuilder
	import org.apache.http.util.EntityUtils
	import org.apache.http.{Consts, HttpResponse, NameValuePair}
	import org.slf4j.{Logger, LoggerFactory}
	
	/**
	  *
	  */
	object HttpClientUtils {
	
	  private val LOGGER: Logger = LoggerFactory.getLogger(HttpClientUtils.getClass)
	
	
	  private val CONNECTION_REQUEST_TIMEOUT = 10000
	  private val SOCKET_TIMEOUT = 10000
	
	  private val defaultRequestConfig = RequestConfig.custom
		.setSocketTimeout(SOCKET_TIMEOUT)
		.setConnectionRequestTimeout(CONNECTION_REQUEST_TIMEOUT)
		.setConnectTimeout(CONNECTION_REQUEST_TIMEOUT)
		.build
	
	
	  def createSSLClientDefault: CloseableHttpClient = {
		try {
		  val sslContext = new SSLContextBuilder().loadTrustMaterial(null, new TrustStrategy {
			override def isTrusted(x509Certificates: Array[X509Certificate], s: String): Boolean = true
		  }).build()
	
		  val sslsf = new SSLConnectionSocketFactory(sslContext)
		  return HttpClients.custom.setSSLSocketFactory(sslsf).build
		} catch {
		  case e: KeyManagementException =>
			LOGGER.error(e.getMessage, e)
		  case e: NoSuchAlgorithmException =>
			LOGGER.error(e.getMessage, e)
		  case e: KeyStoreException =>
			LOGGER.error(e.getMessage, e)
		}
		HttpClients.createDefault
	  }
	
	  /**
		* get 请求
		*
		* @param url
		* @param keys
		* @param enc
		* @return
		*/
	  def get(url: String, keys: Map[String, String] = Map(), enc: String = Consts.UTF_8.name()): String = {
		Preconditions.checkNotNull(url)
		var client: CloseableHttpClient = null
		var returnStr: String = null;
		try {
		  //client = HttpClients.createDefault()
		  client = createSSLClientDefault
	
		  var sb: StringBuilder = new StringBuilder(url)
		  if (keys != null && !keys.isEmpty) {
			import scala.collection.JavaConversions._
			for (entry <- keys.entrySet()) {
			  if (sb.indexOf("?") == -1) {
				sb.append("?")
				sb.append(URLEncoder.encode(entry.getKey, enc))
				sb.append("=")
				sb.append(URLEncoder.encode(entry.getValue, enc))
			  }
			  else {
				sb.append("&")
				sb.append(URLEncoder.encode(entry.getKey, enc))
				sb.append("=")
				sb.append(URLEncoder.encode(entry.getValue, enc))
			  }
			}
		  }
		  val responseHandler = getDefaultHandler
		  LOGGER.info(sb.toString + "=====")
		  val httpget = new HttpGet(sb.toString)
		  httpget.setConfig(defaultRequestConfig)
		  returnStr = client.execute(httpget, responseHandler)
		} catch {
		  case e: Exception => {
			LOGGER.error(e.getMessage, e)
		  }
		} finally {
		  closeClient(client)
		}
		returnStr
	  }
	
	  /**
		* POST 请求 form提交
		*
		* @param url
		* @param keys
		* @param enc
		* @return
		*/
	  def post(url: String, keys: Map[String, String] = Map(), enc: String = Consts.UTF_8.name()): String = {
		Preconditions.checkNotNull(url)
		var returnStr: String = null
		var client: CloseableHttpClient = null
		try {
		  //client = HttpClients.createDefault()
		  client = createSSLClientDefault
	
		  val httpPost: HttpPost = new HttpPost(url)
		  httpPost.setConfig(defaultRequestConfig)
		  if (keys != null && !keys.isEmpty) {
			val formParams: util.List[NameValuePair] = Lists.newArrayListWithExpectedSize(keys.size)
			import scala.collection.JavaConversions._
			for (entry <- keys.entrySet) {
			  formParams.add(new BasicNameValuePair(entry.getKey, entry.getValue))
			}
			val entity: UrlEncodedFormEntity = new UrlEncodedFormEntity(formParams, enc)
			httpPost.setEntity(entity)
		  }
		  val responseHandler: ResponseHandler[String] = getDefaultHandler
		  returnStr = client.execute(httpPost, responseHandler)
		} catch {
		  case e: Exception =>
			LOGGER.error(e.getMessage, e)
		} finally closeClient(client)
		returnStr
	  }
	
	  /**
		* POST 请求 application/json 提交
		*
		* @param url
		* @param data
		* @param enc
		* @return
		*/
	  def postJsonStr(url: String, data: String, enc: String = Consts.UTF_8.name()): String = {
		Preconditions.checkNotNull(url)
		Preconditions.checkNotNull(data)
		var returnStr: String = null
		var client: CloseableHttpClient = null
		try {
		  //client = HttpClients.createDefault()
		  client = createSSLClientDefault
	
		  val httpPost: HttpPost = new HttpPost(url)
		  httpPost.setConfig(defaultRequestConfig)
		  import org.apache.http.entity.StringEntity
		  val entity = new StringEntity(data, enc)
		  entity.setContentType("application/json")
		  httpPost.setEntity(entity)
		  val responseHandler: ResponseHandler[String] = getDefaultHandler
		  returnStr = client.execute(httpPost, responseHandler)
		} catch {
		  case e: Exception =>
			LOGGER.error(e.getMessage, e)
		} finally closeClient(client)
		returnStr
	  }
	
	
	  private def getDefaultHandler = new ResponseHandler[String]() {
		@throws[IOException]
		override def handleResponse(response: HttpResponse): String = {
		  val entity = response.getEntity
		  val returnStr = if (entity != null) EntityUtils.toString(entity)
		  else null
		  EntityUtils.consumeQuietly(entity)
		  returnStr
		}
	  }
	
	  private def closeClient(client: CloseableHttpClient): Unit = {
		if (client != null) try
		  client.close()
		catch {
		  case e: IOException =>
			LOGGER.error(e.getMessage, e)
		}
	  }
	
	}

```