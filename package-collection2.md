# Collection2

将一个schema附加到一个Mongo.Collection上，当插入和更新数据时可以自动验证。

## 安装

	$ meteor add aldeed:collection2

## 为什么使用

- 确保只有可接受的类型和值才能从客户端设置。
- 方便自动定制显示验证报错信息。
- 自动在客户端和服务器端进行验证。
- 使用aldeed:autoform可以自动生成H5表单。


## 将一个Schema附加到一个Collection

假设我们有一个正常的collection

	Books = new Mongo.Collection("books");

我们可以生产一个`SimpleSchema`来表示这个Collection如下：

	var Schemas = {};
	
	Schemas.Book = new SimpleSchema({
	    title: {
	        type: String,
	        label: "Title",
	        max: 200
	    },
	    author: {
	        type: String,
	        label: "Author"
	    },
	    copies: {
	        type: Number,
	        label: "Number of copies",
	        min: 0
	    },
	    lastCheckedOut: {
	        type: Date,
	        label: "Last date this book was checked out",
	        optional: true
	    },
	    summary: {
	        type: String,
	        label: "Brief summary",
	        optional: true,
	        max: 1000
	    }
	});

