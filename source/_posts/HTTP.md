---
title: HTTP
date: 2021-12-21 14:07:19
tags: 

---

HTTP 超文本传输协议

| 请求报文 Request |                                                      |
| :--------------: | ---------------------------------------------------- |
|      请求行      | GET   /info   HTTP/1.1    (请求方法、path、http版本) |
|     Headers      | Host：api.xxx.xxx                                    |
|       Body       |                                                      |

| 响应报文 Response |                                                    |
| :---------------: | -------------------------------------------------- |
|      状态行       | HTTP 1.1   200  OK    (http版本、状态码、状态消息) |
|      Headers      | Content-Type\Content-Length                        |
|       Body        |                                                    |

| 请求方法 |                                           |
| :------: | ----------------------------------------- |
|   GET    | 用于获取；不发送body（幂等）              |
|   POST   | 用于增加或修改资源；有body                |
|   PUT    | 仅用于修改资源；有body（幂等）            |
|  DELETE  | 用于删除；不发送body（幂等）              |
|   HEAD   | 与GET用法相同；差异在于返回响应中没有body |

| 状态码 |                           |
| :----: | ------------------------- |
|  1xx   |                           |
|  2xx   | 请求成功                  |
|  3xx   |                           |
|  4xx   | 客户端异常  400、401、404 |
|  5xx   | 服务器异常                |

|      Headers      |                                                              |
| :---------------: | ------------------------------------------------------------ |
|   Content-Type    | text/html 、application\x-www-form-urlencoded 、multipart\form-data 、 application\json 、image\jpeg |
|  Content-Length   | 内容长度                                                     |
| Transfer-Encoding | Chunked Transfer Encoding（分块传输）                        |
|     Location      | 重定向的目标url                                              |
|    User-Agent     | 用户代理 （webview设置中也会有涉及）                         |
