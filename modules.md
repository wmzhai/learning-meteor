# modules

ES2015里面引入了module机制，和import及export关键字，这些功能从Meteor1.3开始正式支持。

在Meteor1.3里面，添加ecmascript包的时候，会自动添加modules包，相关的实现都在这个包里。module机制为自定义文件加载顺序带来很好的实现方法。


* 在Meteor1.3之前只能通过全局变量或Session在多个文件间共享数据，引入module以后全局变量就没必要了。
* 如果想实现懒加载(lazily evalutation)，则需要将模块放到`imports/`目录里，然后通过import来引用它，注意`node_modules`目录下的文件也是lazily加载的。
* 目前建议将懒加载的文件放在`client/imports/`或`server/imports`目录下，然后在每个architecture下仅保留一个入口(entry point)文件，`client/main.js`和`server/main.js`。`main.js`加载很早，给程序从其他`imports/`目录倒入module的机会。


