---
layout: post
title: "面试准备（5）-- 实现eventEmitter"
subtitle: "关于事件分发"
date:   2021-01-06 10:57:00 +0800
author: "axmiao"
header-img: "img/post-bg-20201215.png"
categories: jekyll update
catalog: true
tags: 
    - 面试题
    - js/ts
---

这一篇是面试准备的第五篇，也是我立了每周一篇blog的flag的第五周，还在坚持，继续加油。

这一篇的主题是实现一个eventEmitter类。eventEmitter在使用中，像是实例化了一个观察者模式对象，构成了一个一对多的结构，当一个被观察的条件发生变化时，对应的多个回调函数都将得到执行。平时工作中用到的多是nodejs提供的events类下的eventEmitter。今天来手动实现一个简易的eventEmitter。

eventEmitter中比较常用的方法有`addEventListener`, `removeEventListner`, `on`, `off`, `emit`, `removeAllEventListners`这几种，其中 `on` 和 `addEventListener`，`off` 和 `removeEventListener`功能完全相同。

具体实现代码如下（以TS为例）：

	class EventEmitter {
	  private events: {};
	  constructor() {
	    this.events = {};
	  };
	  on(eventName: string, func: object) {
	    if (!this.events[eventName]) {
	      this.events[eventName] = []
	    }
	    this.events[eventName].push(func);
	  };
	  emit(eventName: string, ...params) {
	    const events = this.events[eventName];
	    if (events) {
	      events.forEach(event => {
	        event.apply(this, params)
	      })
	    }
	  };
	  off(eventName: string, func?: object) {
	    if (this.events[eventName]) {
	      if (!func) {
	        this.events[eventName] = []
	      } else {
	        this.events[eventName].splice(this.events[eventName].indexOf(func), 1)
	      }
	    }
	  };
	  removeAllListners(eventName?: string) {
	  	if(eventName) {
	  		this.events[eventName] = []
		} else {
			this.events = {}
		}
	  }
	}

具体思路:

1. 声明一个局部变量events用来存储绑定的事件与回调函数，类型为对象。

2. 在on方法中传入事件名eventName和回调函数func，检查events中是否已存在eventName对应的回调队列。若已存在就在原有队列尾部加入新的func，若没有则新建队列，并将func推入队列中。

3. 在emit方法中传入事件名称eventName和可选参数params，取出events中保存的eventName对应的函数队列，依次执行。

4. 在off方法中传入事件名eventName和回调函数func， 分两步检查，第一步检查是否已存在eventName对应的回调函数队列，若存在进行第二部检查，func是否为有效值，若是则从回调队列中将func删除。

5. 在removeAllListener方法中传入可选参数eventName，当eventName有值时，只将eventName对应的回调队列清空，否则清空整个events对象。

以上就是简易eventEmitter 实现的思路与过程。