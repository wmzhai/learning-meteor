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
- Object

