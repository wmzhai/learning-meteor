
# 代码规范

## 目录结构

目录名字有含义的, client, server, lib, public 等.不要放错地方

## template规则

每个template独立文件，Template 的文件名, Template 名字, Template 的 heper js 文件的命名一致

避免多个template共用文件。
如果你的 template 叫做"ContactList", 那么对应的文件也应叫叫做 ContactList.coffee ContractList.jade ContractList.less 

如果你发现需要卷好几屏幕才能看完一个 Template 的代码, 那么可以考虑分解成更小的 Template 了



## Meteor methods 的 check
每一个 Meteor 的 methods 都需要在一开始进行 check. 保证传入参数合法.

## Schema

主要的 Collection 都需要 Attach Schema.   
用 Collection2 代替 Collection. 无论是否使用 Autoform

## 避免使用 jQuery

前端代码使用 jQuery 不是灵丹妙药. 如果可以用template.find 解决就避免 jquery. jQuery 会在全局进行 DOM 操作, 有可能引起未知的后果. 比如页面规模很大的时候, 有可能碰巧修改了其他 Template 里面的元素. 这种 bug 很难发现.

## Server 代码善用 throw new Meteor.Error

返回有价值的错误信息. 便于查错.
