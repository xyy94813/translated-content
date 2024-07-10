---
title: ExtendableCookieChangeEvent
slug: Web/API/ExtendableCookieChangeEvent
l10n:
  sourceCommit: 60c3843f55839380e0c0cdc293ea694fe9943158
---

{{securecontext_header}}{{APIRef("Cookie Store API")}}{{AvailableInWorkers("service")}}

{{domxref("Cookie Store API", "", "", "nocode")}} 的 **`ExtendableCookieChangeEvent`** 接口是在 {{domxref("ServiceWorkerGlobalScope")}} 触发任何匹配 service worker cookie 变更订阅列表的 cookie 发生变更时，传递给 {{domxref("ServiceWorkerGlobalScope/cookiechange_event", "cookiechange")}} 事件的事件类型。cookie 变更事件由 cookie 和类型组成。（要么是 "已更改"，要么是 "已删除"）。

导致 ExtendableCookieChangeEvent 被派发的 Cookie 变更包括：

- Cookie 刚被创建且没有立刻被移除。那么认为 Cookie 是 “已变更” 的。
- Cookie 刚被创建但又立刻被移除。那么认为 Cookie 是 “已删除” 的。
- Cookie 被移除。那么认为 Cookie 是 “已删除” 的。

> **备注：** 因插入具有相同名称、域和路径的另一个 cookie 而被替换的 cookie 将被忽略，不会触发任何变更事件。

{{InheritanceDiagram}}

## 构造函数

- {{domxref("ExtendableCookieChangeEvent.ExtendableCookieChangeEvent", "ExtendableCookieChangeEvent()")}}
  - : 创建一个新的 `ExtendableCookieChangeEvent`。

## 实例属性

_此接口同样继承来自于 {{domxref("ExtendableEvent")}} 的属性。_

- {{domxref("ExtendableCookieChangeEvent.changed")}} {{ReadOnlyInline}}
  - : 返回包含已变更的 cookie 的数组。
- {{domxref("ExtendableCookieChangeEvent.deleted")}} {{ReadOnlyInline}}
  - : 返回包含已删除的 cookie 的数组。

## 实例方法

_此接口同样继承来自于 {{domxref("ExtendableEvent")}} 的方法。_

## 示例

在下面的示例中，我们使用 {{domxref("CookieStoreManager.getSubscriptions()")}} 获取现有订阅列表。（在 Service Worker 中，需要订阅才能监听事件。）我们使用 {{domxref("CookieStoreManager.unsubscribe()")}} 取消现有订阅，然后使用 {{domxref("CookieStoreManager.unsubscribe()")}} 订阅名称为 “COOKIE_NAME” 的 cookie。如果更改了 cookie，事件监听器就会将事件记录到控制台。这将是一个 `ExtendableCookieChangeEvent` 对象，其中的 {{domxref("ExtendableCookieChangeEvent.changed","changed")}} 或 {{domxref("ExtendableCookieChangeEvent.deleted","deleted")}} 属性包含修改后的 cookie。

```js
self.addEventListener("activate", (event) => {
  event.waitUntil(async () => {
    const subscriptions = await self.registration.cookies.getSubscriptions();

    await self.registration.cookies.unsubscribe(subscriptions);

    await self.registration.cookies.subscribe([
      {
        name: "COOKIE_NAME",
      },
    ]);
  });
});

self.addEventListener("cookiechange", (event) => {
  console.log(event);
});
```

## 规范

{{Specifications}}

## 浏览器兼容性

{{Compat}}
