---
title: Swift-perfect开发抓取程序
comments: true
date: 2018-04-10 14:36:06
tags:
 - Swift
 - perfect
 - 抓取
categories:
 - iOS
photos:
thumbnail:
---

Swift-perfect开发抓取程序

<!-- more -->

## 创建基本程序

#### 创建文件夹并进入

```objc
~ mkdir Literary_Crawler
~ cd Literary_Crawler
```
#### 创建Package.swift这个文件是SPM（Swift软件包管理器）编译项目时必须要用到的文件

```objc
// 创建Package.swift 
~ vim Package.swift

import PackageDescription
let package = Package(
	name: "LiteraryCrawler", // 项目名称，使用swift package generate-xcodeproj时生成的项目名
	targets: [],
	// 依赖包，使用swift build 时会自动下载包含的所有包
	dependencies: [
		.Package(url: "https://github.com/PerfectlySoft/Perfect-HTTPServer.git", majorVersion: 2),
        .Package(url:"https://github.com/PerfectlySoft/Perfect-MySQL.git", majorVersion: 2),
        .Package(url: "https://github.com/PerfectlySoft/Perfect-CURL.git", majorVersion: 2)
    ]
)
```
#### 创建Sources文件夹保存源程序及其main.swift

```objc
~ mkdir Sources
~ cd Sources
~ vim main.swift
main.swift
import PerfectLib
import PerfectHTTP
import PerfectHTTPServer
import PerfectCURL

import MySQL
let testHost = "127.0.0.1"
let testUser = "root"
let testPassword = "123456"
let testDB = "LiteraryDB"

func fetchData(response:HTTPResponse) {
    let mysql = MySQL() // 创建一个MySQL连接实例
    let connected = mysql.connect(host: testHost, user: testUser, password: testPassword)
    guard connected else {
        // 验证一下连接是否成功
        print(mysql.errorMessage())
        return
    }
    defer {
        mysql.close() //这个延后操作能够保证在程序结束时无论什么结果都会自动关闭数据库连接
    }
    // 选择具体的数据Schema
    guard mysql.selectDatabase(named: testDB) else {
        Log.info(message: "数据库选择失败。错误代码：\(mysql.errorCode()) 错误解释：\(mysql.errorMessage())")
        return
    }
    // 运行查询（比如返回在options数据表中的所有数据行）
    let querySuccess = mysql.query(statement: "SELECT * FROM tb_user")
    // 确保查询完成
    guard querySuccess else {
        return
    }
    // 在当前会话过程中保存查询结果
    let results = mysql.storeResults()! //因为上一步已经验证查询是成功的，因此这里我们认为结果记录集可以强制转换为期望的数据结果。当然您如果需要也可以用if-let来调整这一段代码。
    results.forEachRow { row in
        Log.info(message: "\(row)")
    }
    Log.info(message: "连接成功")
    response.appendBody(string: "mysql连接成功")
    response.completed()
}
/// 1.创建Server
let server = HTTPServer()
/// 2.创建路由表
var routes = Routes()
/// 3.添加路由到路由表
routes.add(method: .get, uri: "/login") { (request, response) in
    
}
/// 4.将路由表添加到Server
server.addRoutes(routes)
/// 1.1 监听8181端口
server.serverPort = 8181
func request()  {
    let url = CURL.init(url: "http://www.jianshu.com/p/457922e0676c")
    let (_, _, bytes) = url.performFully()
    guard let html = String.init(bytes: bytes, encoding: .utf8) else {
        return
    }
    print(html)
}

/// 5.启动Server
do {
    
    try server.start()
    request()
} catch PerfectError.networkError(let err, let msg) {
    print("网络出现错误：\(err) \(msg)")
}

```
#### 生成xcodeproj

 ```objc
"__TFE10FoundationSSCuRxs8SequenceWx8Iterator7Element_zVs5UInt8rfT5bytesx8encodingVES_SS8Encoding_GSqSS_", referenced from:

  "__TFVE10FoundationSS8Encodingau4utf8S0_", referenced from:
 ```

