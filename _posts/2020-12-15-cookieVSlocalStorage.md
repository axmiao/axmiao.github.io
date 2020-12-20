---
layout: post
title: "面试准备（2）-- cookie VS localStorage"
subtitle: "关于缓存数据"
date:   2020-12-15 22:57:00 +0800
author: "axmiao"
header-img: "img/post-bg-20201215.png"
categories: jekyll update
catalog: true
tags: 
    - 面试题
    - js/ts
---

这是面试题总结的第二篇，内容关于cookie，localStorage和sessionStorage。在前端应用开发中，有很多时候数据的存储不能只依靠后端，一些数据例如：商城应用未登录状态下加入到购物车中的商品，长登录状态下的用户信息等，都需要在前端进行存储。这样就需要用一些前端数据存储方式，前端可实现的数据存储方式有很多种，其中较常见的，在面试中常被问到的就是此篇总结的三种存储方法。

#cookie

cookie的全称为HTTP cookie，看名字就知道，cookie的存储或使用与http网络请求有关。cookie会被携带在每一个http请求的头部中在服务端和客户端之间传输。为了防止对http请求的畸形封装，浏览器会对请求长度做限制，这也导致了cookie的存储是有上限的，目前这个上限大概为4KB左右，由于浏览器的版本及种类不同有微小差异。

cookie是浏览器提供的一种机制，可以通过document.cookie的方法将document对象的cookie属性提供给Javascript创建，写入或读取。

cookie在用户的硬盘上以文件形式存在，每个cookie文件对应着一个域名，由此，cookie可以被同域名下的不同网页访问，但是不可以跨于访问。

当通过document.cookie获取cookie时，将会得到一串字符串，以"key=value; key=value; "的形式存储，其中需要注意的是key和value需要时URL编码，且";"后面是有空格的。

cookie可以通过domain/path的方法设置在那些路径下cookie可见。这样在一些页面里就可以不携带cookie发送网络请求了。

cookie可以通过expires/max-age设置失效时间，前者在原始的http协议中使用，后者在新的http协议中作为expires的替代属性，默认情况下cookie的expires/max-age值为Session，这意味着当浏览器被关闭了cookie就被清除了。设置expires时需要将时间转换为GMT时间，可以通过toGMTString()方法获得。当expires设为一个过去的时间点，即删除此条cookie。max-age的值为cookie生效时长，以秒为单位，当max-age为正数时即从生效时刻开始指定时长之后失效，当max-age为负数时，即与session相同。当max-age为0时，则删除此条cookie。

cookie可以通过设置httponly的属性来指定读取和存储cookie的操作为网络请求，若httponly属性设置为true，则意味着页面中的js无法获取和更改此条cookie，这样可以避免一部分xss攻击的危害，防止用户信息被盗取。但从另外一个方面来看，当设置了这个属性之后，对cookie的更改只能从后端实现。

cookie中存在唯一一个不以健值对形式存在的属性secure，若cookie中存在该字段，则意味着只能在使用SSL连接时才会将改cookie发送到服务端。

#localStorage

localStorage顾名思义为本地存储，它与cookie不同，不会在网络请求中被发送到服务端，这样对比cookie就节省了一部分带宽。与cookie相同的是，localStorage也在本地硬盘也存在对应的文件，且同样遵循同源策略，可以被同域名，同端口的不同页面共享。

localStorage是HTML5中新加入的window属性，可以通过window.localStorage来获得所存储的信息。同时因为是H5中新加入的属性，在低版本的浏览器中的支持性不好。在使用时可以使用如下的判断来区别是否支持：

> if(window.localStorage) {
>	alert("支持");
> } else {
>	alert("不支持");
> }

localStorage的存储上限大概为5MB，这主要是因为存储在localStorage中的数据以字符串形式存在，在对其进行读取/存储的过程中需要经过字符串操作，而JavaScript中的字符串长度上限大致为5MB左右。其中需要注意的是，当localStorage存储的内容过大，操作localStorage将会消耗大量内存，这将导致页面卡顿。

localStorage在隐私模式下是不可读取的，且在任何模式下都不可以被爬虫抓取。

localStorage的存储有三种方式，分别为：

> let storage = window.localStorage;
> storage.a = "a";	 

> storage["b"] = 100;

> storage.setItem("c", "c");

对应的也有三种读取方式，分别为：

> let storage = window.localStorage;
> let a = storage.a;

> let b = storage["b"];

> let c = storage.getItem("c");

官方推荐的存取方式为getItem/setItem方法，在此之外还有clear方法用来清空整个localStorage，removeItem方法用来移除指定的某个值。

这里还有一个值得注意的点是，如上的demo中的storage["b"]，虽然在存储时是int类型，取回后将会为string类型。

由于localStorage存储时以字符串形式存在，在存之前需要JSON.stringify()来将内容转为字符串，使用之前也需要用JSON.parse()的方法将其转为对象。

localStorage对象长期有效，除非手动将其清除，不然将一直存在。由于这个原因在使用时就需要注意是不是需要检查该对象是否过期，是不是需要被清除掉。

#SessionStorage

SessionStorage和localStorage极其相似，但也存在不同点，sessionStorage的生存周期仅到浏览器关闭为止，且session无法被同域名不同端口的页面共享。

#共同需要注意的点

无论是cookie，localStorage或SessionStorage，只要打开控制台就可以被看到，当网站存在xss风险时，用户数据就可能遭到窃取或篡改，因此在存储某些用户敏感信息的时候要加倍小心，以降低泄密风险。