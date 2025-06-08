---
title: 事件驱动组件
description: 如何使用JavaScript/TypeScript实现事件驱动组件
keywords: 'JavaScript/TypeScript,事件驱动'
top_img: EventBus/1-1.jpg
cover: EventBus/1-1.jpg
tags:
  - JavaScript/TypeScript
  - Design Mode
categories:
  - 代码浪潮
abbrlink: 95aaf69
date: 2024-10-02 17:05:43
---
# 概述
- 本文打算实现一个由事件驱动的组件，实现的功能：
    - 允许自定义事件以及对应的事件处理方法
    - 允许移除自定义事件以及对应的某个事件方法
    - 支持对同一个事件添加多个处理方法，并且移除其中的某一个处理方法
    - 支持只触发一次、节流、防抖

# 实现思路
![事件驱动组件结构](EventBus/1-2.png)

- 事件驱动的组件采用了 **观察者模式** 和 **代理模式** 两种软件设计模式实现的。
- 数据结构主要采用了js中的对象中的属性和属性值，用来存储自定义事件和处理方法。
- 首先分别定义事件和处理方法的接口

```typescript
interface event {
  id: string,
  listeners: listener[]
}

interface listener {
  handler: (...args: any[]) => any;
  once: boolean;
}
```

- 为了可以实现对某一个自定义事件进行一系列的listener处理依次处理，用了数组结构

```typescript
export class EventBus {
  private events: { [key: string]: event };

  constructor() {
    this.events = {}
  }

  // subscribe event once
  subscribeOnce(event: string, handler: (...args: any[]) => any) {
    this._subscribe(event, handler, true);
  }

  // subscribe event
  subscribe(event: string, handler: (...args: any[]) => any) {
    this._subscribe(event, handler);
  }

  // subscribe debounce event
  subscribeDebounce(event: string, handler: (...args: any[]) => any, wait: number = 300) {
    this._subscribe(event, this._debounce(handler, wait))
  }

  // subscribe trottle event
  subscribeTrottle(event: string, handler: (...args: any[]) => any, limit: number = 300) {
    this._subscribe(event, this._trottle(handler, limit));
  }

  // cancel subscribition event handler
  unsubscribe(event: string, listener: listener) {
    if (!this.events[event]) {
      return;
    }
    const listeners = this.events[event].listeners;

    if (listeners.find(l => l === listener)) {
      const index = listeners.findIndex(l => l === listener)
      listeners.splice(index, 1)
    }
  }

  // do handler chain
  dispatch(event: string, ...args: any[]) {
    const listeners = this.events[event]?.listeners;
    if (listeners && Array.isArray(listeners)) {
      listeners.forEach(listener => {
        listener.handler(...args);
        if (listener.once) {
          this.unsubscribe(event, listener);
        }
      })
    }
  }

  private _subscribe(event: string, handler: (...args: any[]) => any, once: boolean = false) {
    const listeners = this.events[event]?.listeners;
    const listener = { handler, once };
    if (listeners) {
      listeners.push(listener);
    } else {
      this.events[event] = { id: event, listeners: [listener] };
    }
  }

  // debounce function
  private _debounce(fn: (...args: any[]) => any, wait: number) {
    let timeout: NodeJS.Timeout | null = null;

    return (...args: any[]) => {
      if (timeout) {
        clearTimeout(timeout);
      }

      timeout = setTimeout(() => {
        fn(...args);
      }, wait);
    }
  }

  // trottle function
  private _trottle(fn: (...args: any[]) => any, limit: number) {
    let timeout: NodeJS.Timeout | null = null;

    return (...args: any[]) => {
      if (timeout) return;

      timeout = setTimeout(() => {
        fn(...args);
        timeout = null;
      }, limit);
    }
  }
}
```

- 防抖实现：
    - 实现一个Timeout，如果触发了防抖，判断是否存在Timeout。如果存在，就清理之后重新计时；不存在就直接创建。
- 节流实现：
    - 实现一个Timeout，如果触发了节流，判断是否存在Timeout。如果存在，就不处理；如果不存在，就创建一个Timeout。
- 只触发一次实现：
    - 在触发对应事件之后，判断listener中的once属性。如果为true，就会移除该事件处理方法。