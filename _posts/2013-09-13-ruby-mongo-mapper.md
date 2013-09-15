---
layout: post
author: tim
title: "{Ruby,MongoDB,MongoMapper} = Strict models and implied schemas"
categories: programming
tags: [ruby, db, mongo]
---

Every web developer that has ever worked with PHP and MySQL (still the most common database solution out there) knows that it can be quite frustrating to set up your database schema.
Most people start with a simple ER diagram that they then translate into SQL (and most web devs will use phpMyAdmin for this 'chore').
When that's done, you can really get cracking.
Looking at your database schema, you can start to write your database interface logic.
If you have a change in functionality you need to translate this to SQL and apply it to your schema.

Because of this separation between your code and the database your schema 'lives' in two places.
This creates a strong coupling between your code and the database schema.
What that means is that if you change one, you need to change the other for everything to still work as expected.
Usually, you want to avoid coupling because it can cause hard to track down bugs.
Sure, you can store your SQL alongside your code in your versioning software, but that's just not good enough (unless you use some arcane magic to update the database schema without data loss on every x runs of your program).

So far for what it's usually like for web developers. I'm now going to describe how [MongoMapper](https://github.com/mongomapper/mongomapper) handles things.
I'll first describe the MongoDB system, for those that haven't encountered it yet.

In its core, mongo is a document-oriented database system, as opposed to a relational database.
Mongo doesn't have tables, but _collections of documents_.
These documents are represented as JSON (or rather BSON, a binary compatible form of JSON).
For instance:

{% highlight json %}
{
	"first_name": "John",
	"last_name": "Doe",
	"initials": "J.D.",
	"age": 24
}
{% endhighlight %}

But the following is also a document:

{% highlight json %}
{
	"name": {
		"first": "John",
		"last": "Doe",
		"initials": "J.D."
	},
	"age": 24,
	"comments": [
		{ "text": "I liked this", "article": { ... } },
		{ "text": "I didn't like this", "article": { ... } },
		{ "text": "This was great!", "article": { ... } }
	]
}
{% endhighlight %}

As you can see, documents are much more versatile than tables because they have a sense of hierarchy.
Note that you can still build relational-like models using mongo.
For instance, in the second example, you would probably include the key of an article in each comment, instead of the actual entire article.

Mongo provides a pretty easy way to access collections:
{% highlight js %}
db.a.save({a: 25})
db.a.find()
 => [ {a: 25} ]
{% endhighlight %}

Good, now on to MongoMapper. It defines itself as 'a mongo object relationship mapper', which means that it provides a mechanism for translating Ruby objects into Mongo and vice-versa.
Now what that means to me is this: persistent storage for your Ruby runtime objects.
And as far as I'm concerned, that is *amazing*.

After I played with it for a night I found that it was a little bit less transparent than I had hoped and I had to mess around with a lot of details before I got it working correctly, but the underlying philosophy is truely beautiful.
You can mark your Ruby objects as Documents and MongoMapper will take care of everything for you (as long as you use their constructor).
Consider these classes:

{% highlight ruby %}
class User
	include MongoMapper::Document

	key :name, Name
	key :age, Integer
	many :comments
	many :articles
end

class Name
	include MongoMapper::EmbeddedDocument

	key :first, String
	key :last, String
	key :initials, String
end

class Comment
	include MongoMapper::Document

	key :text, String
	belongs_to :article
	belongs_to :user
end

class Article
	include MongoMapper::Document
	
	belongs_to :author, :class_name => 'User'
	key :text, String

	many :comments
end
{% endhighlight %}

This will create a model that is represented like this in UML:

![UML diagrams - how enterprisey!](/post-imgs/mongomapper-uml.png)

One of the interesting things to note here is that in class `User`, MongoMapper will read `many :comments` as: a `User` has 0..\infty instances of class Comment.
It derives that info just from me giving the key the name `comments` (and the fact that there are `many`).
In class `Article` you see that I explicitly state that `author` is of type User since I don't want to call the property 'user' in that class.

The thing that really blew my mind was that even though this is stored in Mongo in a relational way (there will be three collections, `user`, `comment` and `article` and relations like `belongs_to :user` will generate a JSON entry `"user_id": "233f32f32f332ras"`), all the information you need is dynamically available if you have a reference to an object.
Assume that the John Doe that I defined earlier is in the database.

{% highlight ruby %}
john = User.first(:age => 24)
puts john.comments[0].article.author.name.first
{% endhighlight %}

That *one* line will perform all the queries nessecary to:

1. Select all comments by John
2. Get the first
3. Select the article that comment was made on using the article id
4. Select the author for that article using the author_id
5. Get the Name object for that author

For those of you with PHP/MySQL experience, think about the monster query you would need to accomplish the same, providing you have `User`, `Name`, `Comment` and `Article` tables (although to be fair you would probably flatten `Name` into `User`).

So, using MongoMapper with Ruby and MongoDB, all the structure in your database comes from your model which solves the coupling problem and also makes working with it a whole lot more fun.
There are a couple of downsides though, the most important of which is that if your database doesn't have an internally defined structure, you need to keep that structure in mind yourself when you make manual edits.
If you need to pre-fill a MySQL database, you would probably just use phpMyAdmin and fill out the web forms.

Since the schema comes from your application model, you can't do that easily when you are using MongoMapper.
The easiest way to pre-fill data is to just write a quick script that imports your model and creates the nessecary data for you.
Doing it manually would mean that you have to type out your data in BSON format like MongoMapper would generate it - *including* a unique ID for each document.

In conclusion, I had a great time playing with MongoMapper and I can highly recommend giving it a try.
