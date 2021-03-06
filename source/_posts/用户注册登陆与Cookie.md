---
title: 用户注册登陆与Cookie
date: 2018-06-15 22:14:23
tags: 开发经验
categories: 学习
---
一个简单用户注册登陆与Cookie保存原理。
牵扯到一些简单的后端知识，我学习的时候以Node.js为例学的。

分三个步骤：
1.用户注册
用户注册最常见需要做的时间就是表单校验。三大原则：（1）保证邮箱格式正确；（2）保证密码填写；（3）保证密码和确认密码一致。
校验可以等用户提交数据给后台时后台来检验发现格式错误返回响应码报错，也可以直接前端校验阻止提交表单。
用户注册成功之后，后台直接将用户数据写入数据库；返回成功响应码，前台用promise收到响应码跳转到登陆页面；
如果用户重复注册同一账号，由后台查询数据库中是否已经存在有相同的邮箱，如果有，返回相应的响应码报错；

2.用户登陆
用户在登陆页面输入登陆信息，由后台校验邮箱是否存在，密码是否相等，如果是，则返回成功响应码，前台跳转到登陆后页面；
但是问题是没登陆的用户也可以直接在地址栏输入登陆后的页面的地址直接跳转，并且如果在登陆后的界面里区分不同的用户身份呢？——所以需要通过Cookie认证用户身份；

3.Cookie
用户第一次登陆时，后台会在验证成功之后在响应头上打上Cookie，里面包含了邮箱，那么这个Cookie就会保存在用户本地，之后用每一次请求新的资源都会在请求头中带上这个Cookie，后台可以通过用户直接在地址栏输入登陆后的界面发送请求的时候先检验是否请求头中有Cookie，这个Cookie中的邮箱是否和数据库中的相匹配，如果是才返回成功响应码，否则拒绝。如果成功，后台顺便取出匹配的用户所有信息（都是以哈希形式储存的），作为JSON返回给前台，前台就这样得到了用户的相应信息，可以将其返回填充到页面中，实现了个性化的个人登陆后的页面。    



* Cookie特点
    * 服务器通过Set-Cookie响应头设置Cookie；
    * 浏览器得到Cookie之后，每次请求都要带上Cookie
    * 服务器读取Cookie就知道登录用户的信息（email）
    * Cookie是跟着浏览器走的，只带同一域名的Cookie
    * Cookie可以篡改（不安全）
    * Cookie默认有效期20分钟左右；但是后端可以强制修改有效时间
    * Cookie一般大小在4K以内


* Cookie可以被篡改
所以这是为什么需要使用Session来加密的原因，见下一篇文章。