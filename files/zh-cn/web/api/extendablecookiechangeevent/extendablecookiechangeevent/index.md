---
title: ExtendableCookieChangeEvent：ExtendableCookieChangeEvent() 构造函数
slug: Web/API/ExtendableCookieChangeEvent/ExtendableCookieChangeEvent
l10n:
  sourceCommit: 60c3843f55839380e0c0cdc293ea694fe9943158
---

{{securecontext_header}}{{APIRef("Cookie Store API")}}{{AvailableInWorkers("service")}}

**`ExtendableCookieChangeEvent()`** 构造函数创建了一个新的 {{domxref("ExtendableCookieChangeEvent")}} 对象，当匹配上 Service Worker 的 cookie 变更订阅列表的 cookie 发生更改时，该对象就是传递给 {{domxref("ServiceWorkerGlobalScope")}} 触发的 {{domxref("ServiceWorkerGlobalScope/cookiechange_event", "cookiechange")}} 事件的事件类型。当发生变化事件时，浏览器会调用该构造函数。

> **备注：** 生产环境的站点通常不需要此事件构造函。它主要用于需要该事件实例的测试。

## 语法

```js-nolint
new ExtendableCookieChangeEvent(type)
new ExtendableCookieChangeEvent(type, options)
```

### 参数

- `type`
  - : 包含事件名称的字符串。大小写敏感，并且浏览器总是设置为 `cookiechange`。
- `options` {{optional_inline}}
  - : 一个对象, _除了 {{domxref("ExtendableEvent/ExtendableEvent", "ExtendableEvent()")}} 中定义的属性外_, 还可以包含以下属性：
    - `changed` {{optional_inline}}
      - : 包含被变更的 cookie 的数组。
    - `deleted` {{optional_inline}}
      - : 包含被删除的 cookie 的数组。

### 返回值

一个新的 {{domxref("ExtendableCookieChangeEvent")}} 对象。

## 规范

{{Specifications}}

## 浏览器兼容性

{{Compat}}
