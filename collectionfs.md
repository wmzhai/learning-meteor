# 文件处理


[CollectionFS](https://github.com/CollectionFS/Meteor-CollectionFS)

CollectionFS包使用2个很重要的全局变量：FS.File和FS.Collection

- FS.File 在client端和server端封装一个文件和它的数据。它有点像浏览器的File对象，也可以从File对象创建，不过他有一些额外的属性和方法。 如果它的示例是通过find或者fideOne返回的话，则它的很多方法都是reactive的。
- FS.Collection 提供了一个用来存储各种文件信息的colletion。 它实际上有一个对应的Mongo.Collection对象，很多针对collection的方法都可以调用。

从FS.Collection里面获取的一个文档被表现成FS.File


## 起步

使用这个package的第一步是定义一个FS.Collection

	var Images = new FS.Collection("images", {
	  stores: [new FS.Store.FileSystem("images", {path: "~/uploads"})]
	});

在这个例子里，我们定义了一个名叫“images”的FS.Collection，它将成为mongo数据库里面的一个新的名为"cfs.images.filerecord"的Collection。 同时，我们指定它使用文件系统保存文件，并且将文件保存在本地文件系统的~/upload目录里。  如果你不指定一个特定的path，则自动使用一个项目里面的cfs/files目录。