
# 代码规范

## 1. 目录规范

学习rails项目的优点，特定目录放特定内容，不要放错地方
- client 客户端代码
- server 服务器端代码
- lib 两端都有的代码
- public 
- packages 包


## 2. 命名规则

### 目录命名
  小写字母

### 文件名
 驼峰规则，比如ContactList.coffee
  
### Collection命名
 Collection变量首字母大写驼峰复数，数据库名称首字母小写驼峰复数，比如
 
  @ContactLists =  new Mongo.Collection "contactLists"
 

## 3. Template规范

### 独立文件
 避免多个template共用文件。   
 每个template独立文件，Template 的文件名, Template 名字, Template 的 js/coffee 文件的命名一致   
 如果你的 template 叫做"ContactList", 那么对应的文件也应叫叫做 ContactList.coffee ContractList.jade ContractList.less 

### 模板粒度尽量小
如果你发现需要卷好几屏幕才能看完一个 Template 的代码, 那么可以考虑分解成更小的 Template 了

### 小模板效率高

 有循环以及each结构，内部卡片尽量使用单独模板，这样数据变化时只会局部刷新，而不会全局刷新。

## 4. Schema规范

主要的 Collection 都需要 Attach Schema.   
用 Collection2 代替 Collection. 无论是否使用 Autoform


## 5. Router规范

## Meteor methods 的 check
每一个 Meteor 的 methods 都需要在一开始进行 check. 保证传入参数合法.


## 6. 其他

### 避免使用 jQuery

前端代码使用 jQuery 不是灵丹妙药. 如果可以用template.find 解决就避免 jquery. jQuery 会在全局进行 DOM 操作, 有可能引起未知的后果. 比如页面规模很大的时候, 有可能碰巧修改了其他 Template 里面的元素. 这种 bug 很难发现.

### Server 代码善用 throw new Meteor.Error

返回有价值的错误信息. 便于查错.
