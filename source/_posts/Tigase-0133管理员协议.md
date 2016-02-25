title: Tigase-0133管理员协议
date: 2015-12-10 00:11:25
categories: [XMPP,Tigase]
tags: [XMPP,Tigase]
---
* 将`src/main/groovy/tigase/admin`拷贝到`scripts/`下,`XEP-0133`协议由`groovy`完成。
* 管理员向服务端发送添加用户的请求，发送至`sess-man`

		<iq id="EURERE234234234EEWEWEW" to="sess-man@192.168.0.50" type="set"><command xmlns="http://jabber.org/protocol/commands" node="http://jabber.org/protocol/admin#add-user"></command></iq>
* 接收服务端返回后，提交添加用户数据

		<iq from="admin@192.168.0.50/smack" id="KJKJKJWEW121312312" to="sess-man@192.168.0.50" type="set" xml:lang="en"><command xmlns="http://jabber.org/protocol/commands" node="http://jabber.org/protocol/admin#add-user" sessionid=""><x xmlns="jabber:x:data" type="submit"><field type="hidden" var="FORM_TYPE"><value>http://jabber.org/protocol/admin</value></field><field var="accountjid"><value>test@192.168.0.50</value></field><field var="password"><value>123456</value></field><field var="password-verify"><value>12345</value></field><field var="email"><value>ttt</value></field></x></command></iq>
* [XMPP-0133协议](http://xmpp.org/extensions/xep-0133.html)


