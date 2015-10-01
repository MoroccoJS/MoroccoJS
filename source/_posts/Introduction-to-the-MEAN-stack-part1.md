title: Introduction to the MEAN stack (part1)
date: 2015-10-01 13:27:28
description: It's an introduction to the MEAN stack, for the ones who wants to be fullstack developers, in the first chapter, we will introduce you to the Model part, MongoDB.
category: tutorials
author: Soufiane ELBAZ
---
# Introduction:

As we said in the description in the first time we will start with the Model part and especially with the well known document based database (NoSQL) MongoDB.

If you are similar with JSON you will rapidly notice the similarity between the two, it's (key : value) based, the values may contain a document, arrays, and arrays of documents the thing that can let you far from complex join relations that are also expensive in what we call performance ...

A MongoDB server can contain multiple databases and each database contains multiple collections that can be in relations (OneToOne, OneToMany, ManyToOne and ManyToMany).

An Example of a MongoDB collection :

{% codeblock lang:json %}
  [
    {
      "_id" : ObjectId("5608d1d84b77357a0c2cdad2"),
      "name" : "John DOE",
      "age" : 25,
      "address" : {
        "street" : "Broadway St.",
        "city" : "New York"
      }
    }
  ]
{% endcodeblock %}

# Installation:

First of all you need to install a MongoDB locally, here is a link in the official documentation that contains all the steps for all environnements :

 - {% link Linux http://docs.mongodb.org/manual/administration/install-on-linux/ true %}
 - {% link OS X http://docs.mongodb.org/manual/tutorial/install-mongodb-on-os-x/ true %}
 - {% link Windows http://docs.mongodb.org/manual/tutorial/install-mongodb-on-windows/ true %}

Now that we have MongoDB installed locally, we will install a tool that will help us to gather the data from a specific database in our server.

The tool is called Mongoose, it's the linker between the web server and the model server.

It's kind usefull and contain many features that will help us a lot, like tha callbacks features.

#### Mongoose installation using npm :

{% codeblock lang: bash %}
	npm install -g mongoose
{% endcodeblock %}

We will play around just to get used of it, first we type the following command:

{% codeblock lang:bash %}
	node
{% endcodeblock %}

Now we will declare a mongoose variable that contains the Mongoose module that also contains all the functions we need .
#### You should now that nodejs is module loading systems, if you want to add functions or objects to your root module you should add them to module.exports

#### Types of modules : 

 - Core modules like the 'http' module, they are contained in the /lib folder and are compiled to binary.
 - File module, files that end with .json , .js and finally .node .

{% codeblock lang:javascript %}
	var mongoose = require("mongoose");
{% endcodeblock %}

Then we connect it to the installed database : 

{% codeblock lang:javascript %}
	var connectionCallBack = function(err){

		if(err)
		    {
		    	console.log("Error!");
		    }	
	  	else
		    {
		    	console.log("Connected to the MongoDB server !");
		    }	
	};

	mongoose.connect('mongodb://localhost/test3', connectionCallBack);
{% endcodeblock %}


Before creating our first Document we should design its Schema first, so we should create a Schema object :

{% codeblock lang:javascript %}
	var Schema = mongoose.Schema;
{% endcodeblock %}

We will create a User Schema : 

{% codeblock lang:javascript %}
	var userSchema = new Schema({
		/*Key, Value*/
		"name" : { type : String, required : true},
		"age" : { type : Number, required : true}
		"address" : {
			"street" : { type : String, default: "none"},
			"city" : { type : String, default: "none"}
		}
		"created_at" : { type : Date, default : Data.now }
	});
{% endcodeblock %}

So our schema contains three properties : name, age and address .

The address is an Embedded Sub-document that contains : street and city, we did that because the little amount of data that can contain an address, if for example 
we had a relation user<=>posts we shall not do this, we will store the posts in a distinct documents and we will set a ManyToOne relation.

An "_id" will be added automatically as a primary key with unique value, its type is ObjectId a type contained in "mongoose.Schema.Types.ObjectId".

Now that our Schema is all that we have to is to add it to the database.

{% codeblock lang:javascript %}
	/*in the first parameter we set the name of the document and in the the second one the the Schema we created*/
	var User = mongoose.model('User', userSchema);
{% endcodeblock %}

### CRUD Operations : 

#### Create : 

{% codeblock lang:javascript %}
	var userCallBack = function(err, user)
	{
		if(err)
		{
			throw new Error("An error occured while adding the user : "+err);
		}

		console.log("The user is successfully added .");
	};

	var firstUser = new User({});

	firstUser.name = "John DOE";
	firstUser.age = 23;
	firstUser.address.street = "Broadway St.";
	firstUser.address.city = "New York";

	/* We can get the _id of the user object before saving it */

	console.log(firstUser._id);

	firstUser.save(userCallBack);

{% endcodeblock %}

#### Read : 

{% codeblock %}
	var usersCallBack = function(err, users)
	{
		if(err)
		{
			throw new Error("An error occured while reading the users : "+err);
		}

		console.log("The users  : "+users);
	};

	/* find(selections, callback); */
	/* Selecting all the users by "{}" */
	/* Similar to SELECT * FROM users */
	User.find({}, usersCallBack);

	/* Now let's select all the mature people ; the projection will only contain name and age*/
	/* Similar to SELECT name, age FROM users WHERE age > 21 */
	User.find({ age : { $gt : 21 } }).select('name age').exec(usersCallBack);

	/*http://mongoosejs.com/docs/api.html*/
{% endcodeblock %}

### Update : 

{% codeblock lang:javascript %}
	var usersCallBack = function(err, users)
	{
		if(err)
		{
			throw new Error("An error occured while updating the users");
		}

		console.log("The users  : "+users);
	};
	/* Now let's let the immature people be mature and rename them as MoroccoJS */
	/* update(selections, updates, options, callback); */
	User.update({ age : { $lt : 21} } , { $set : { age : 21 , name : "MoroccoJS" } } , { multiple : true }, usersCallBack);


{% endcodeblock %}

### Delete : 

{% codeblock lang:javascript %}
	var deletionCallBack = function(err)
	{
		if(err)
		{
			throw new Error("An error occured while deleting the users");
		}

		console.log("Deleted successfully!");
	};
	
	/* Now let's update all users with with "MoroccoJS" name */
	/* For multiple deletion we should add the option { multiple : true } */
	/* If we don't, the first user found with the setted criterias will be the only */
	/* one to be deleted */

	User.remove({ name : { $eq: "MoroccoJs"} }, deletionCallBack);

{% endcodeblock %}

You should take a look {% link here http://mongoosejs.com/docs/api.html true %} for more informations ;) .

That's it for the Model Part :D

## See you next time with "ExpressJS" and "NodeJS", until that we wish you all GoodLuck.

## Special thanks go to Youssef Kababe for his commitments in favor of the Moroccan JS and Ruby community (y).














