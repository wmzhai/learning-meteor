# SimpleSchema

## 安装
	
	$ meteor add aldeed:simple-schema

## 基本使用

可以通过创建SimpleSchema实例来验证对象。 

把aldeed:collection2包加入app，可以将SimpleSchema实例附加到collections上自动验证插入和更新操作。

把包aldeed:autoform包加入app，可以将SimpleSchema实例附加表单上，从而根据schema生成和验证表单。

## 示例

	// Define the schema
	BookSchema = new SimpleSchema({
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
	
	// Validate an object against the schema
	obj = {title: "Ulysses", author: "James Joyce"};
	
	isValid = BookSchema.namedContext("myContext").validate(obj);
	// OR
	isValid = BookSchema.namedContext("myContext").validateOne(obj, "keyToValidate");
	// OR
	isValid = Match.test(obj, BookSchema);
	// OR
	check(obj, BookSchema);
	
	// Validation errors are available through reactive methods
	if (Meteor.isClient) {
	  Meteor.startup(function() {
	    Tracker.autorun(function() {
	      var context = BookSchema.namedContext("myContext");
	      if (!context.isValid()) {
	        console.log(context.invalidKeys());
	      }
	    });
	  });
	}

## SimpleSchema组合使用

可以使用sub-schema来组合使用SimpleSchema，比如

	AddressSchema = new SimpleSchema({
	  street: {
	    type: String,
	    max: 100
	  },
	  city: {
	    type: String,
	    max: 50
	  },
	  state: {
	    type: String,
	    regEx: /^A[LKSZRAEP]|C[AOT]|D[EC]|F[LM]|G[AU]|HI|I[ADLN]|K[SY]|LA|M[ADEHINOPST]|N[CDEHJMVY]|O[HKR]|P[ARW]|RI|S[CD]|T[NX]|UT|V[AIT]|W[AIVY]$/
	  },
	  zip: {
	    type: String,
	    regEx: /^[0-9]{5}$/
	  }
	});
	
	CustomerSchema = new SimpleSchema({
	  billingAddress: {
	    type: AddressSchema
	  },
	  shippingAddresses: {
	    type: [AddressSchema],
	    minCount: 1
	  }
	});


## 抽取部分Schema

有时候你有一个大型的SimpleSchema对象，不过只需要它的一个子集，这时可以将部分key抽取出来形成一个新的schema，这时可以使用pick方法

	var profileSchema = new SimpleSchema({
	  firstName: {type: String},
	  lastName: {type: String},
	  username: {type: String}
	});
	
	var nameSchema = profileSchema.pick(['firstName', 'lastName']);


## Schema规则

下面是一些可以在Schema里面定义的规则

### type

类型可以是标准的javascript类型，比如

- String  
- Number
- Boolean
- Date
- Object
- Array


### label

在验证错误信息里面使用的用来表示这个字段的内容。 默认来说，就是这个字段内容的描述，如果针对不同场合需要有不同内容，可以使用一个回调函数来表示，比如

	MySchema = new SimpleSchema({
	  firstName: {
	    type: String,
	    label: function () {
	      return Session.get("lang") == "de"
	            ? "Vorname" : "first name";
	    }
	  }
	});

### optional

一般来说，所有key都是必须的，可以设置 `optional: true` 来改变这一点。

### min/max

如果type是`Number`，则定义了最大或者最小的数值。  
如果type是`String`，则定义了最大或者最小的字符串长度。
如果type是`Date`，则定义了最大或者最小的日期。


### exclusiveMin/exclusiveMax

如果设置成true的话，则表示数值区域是一个需要被排除的区域。默认是false，表示包含区域。


### decimal

如果类型是Number且设置成true是，则表示允许非整数，默认是false。


### minCount/maxCount

数组的最小最大长度。仅当类型为Array的时候使用。

### allowedValues

一个允许值的数组。

### regEx

正则表达式。

