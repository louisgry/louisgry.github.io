---
title: 'Review: 计算机网络-剑指Java'
date: 2019-11-04 11:56:35
tags: 
- Review
description: 计网简介、TCP三次握手、四次挥手、TCP、HTTP、Socket
---
<!-- more -->
- 计网简介
	- OSI七层模型
	- TCP/IP五层模型
- TCP三次握手、四次挥手
	- 三次握手
		- `->` SYN=1、seq=x
		- `<-` SYN=1、ACK=1、seq=y、ack=x+1
		- `->` ACK=1、seq=x+1、ack=y+1
	- 四次挥手
		- `->` FIN=1、seq=u
		- `<-` ACK=1、seq=v、ack=u+1
		- `<-` FIN=1、ACK=1、seq=w、ack=u+1
		- `->` ACK=1、seq=u+1、ack=w+1
- TCP
	- TCP和UDP的区别
	- TCP的滑窗
- HTTP
	- HTTP状态码
	- 浏览器键入URL按下回车后经历的流程
	- GET和POST的区别（GET在URL、POST在报文体中）
	- Cookie和Session的区别（Cookie在客户端、Session在服务端）
	- HTTP和HTTPS的区别
- Socket
	- Socket是对TCP/IP协议的抽象，是操作系统对外开放的接口
	- TCP和UDP实现客户端和服务器的通信