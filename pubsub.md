# 发布与订阅

传统web程序通过采用"request-respond"的形式进行client与server端的**单向**数据交互。client通过RESTFul HTTP请求向服务器获取HTML或JSON数据，而server则无法主动push数据到client。而Meteor则通过发布订阅机制达到双向数据交互的目的。


## 发布 Publication

发布仅能写在服务端

```js
Meteor.publish('Lists.public', function() {
  return Lists.find({
    userId: {$exists: false}
  }, {
    fields: Lists.publicFields
  });
});

```




## 订阅 Subscription

