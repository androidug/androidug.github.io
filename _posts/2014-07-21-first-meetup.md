---
layout: post
title: First meetup
description: "Our first meetup!"
category: meetup
tags: [meetup, google-io, orm]
image:
  feature: Google-IO-14.jpg
---

21 July 2014 marked our first usergroup meetup. After having discussed starting an Android usergroup since the beginning of the year, we decided to just do it.

We decided to start small and tried to keep it fairly informal, so after a quick introduction we had 2 topics to discuss:

* Recent developments in the Android world (By Matt)
* Sugar ORM (By Ross)

## Recent developments in the Android world

As Google IO happened recently, we mostly focused on announcements from there. The keynote is well worth watching: [Google I/O 14 Keynote](https://www.youtube.com/watch?v=wtLJPvx7-ys)

### We also briefly discussed:

* [Android L](http://developer.android.com/preview/index.html)
* [Android Wear](http://developer.android.com/wear/index.html)
* [Android TV](http://developer.android.com/tv/index.html)
* [Android Auto](http://developer.android.com/auto/index.html)
* [Material design](https://developer.android.com/preview/material/index.html)

And as everybody that attended works on Discovery Vitality applications, this was quite a big one for us:

* [Android Fit](https://developers.google.com/fit/)

## Sugar ORM

Ross gave us a quick introduction to [Sugar ORM](http://satyan.github.io/sugar/)

Sugar ORM is a very simple ORM for Android that allows you to interact with SQLite in a simple way with minimal boilerplate.

Sugar CRM focuses on two things and nothing more:

* Database creation
* Simple APIs to manipulate domain objects

### Setup
Sugar ORM requires very little setup. 

All you need to do is, specify SugarApp as your application class in AndroidManifest.xml. You do that by changing the android:name attribute of the application tag.

There's also some optional meta tags to configure it. In your AndroidManifest.xml:

{% highlight xml %}
<application android:label="@string/app_name" android:icon="@drawable/icon"     android:name="com.orm.SugarApp">
    .
    .
    <meta-data android:name="DATABASE" android:value="sugar_example.db" />
    <meta-data android:name="VERSION" android:value="2" />
    <meta-data android:name="QUERY_LOG" android:value="true" />
    <meta-data android:name="DOMAIN_PACKAGE_NAME" android:value="com.example" />
    .
    .
</application>
{% endhighlight %}

### Entities

You simply extend SugarRecord for all the classes that you need persisted. That's it:

{% highlight java %}
public class Book extends SugarRecord<Book> {
  String title;
  String edition;

  public Book(){}

  public Book(String title, String edition){
    this.title = title;
    this.edition = edition;
  }
}
{% endhighlight %}

### APIs
Performing CRUD operations are very simple. Functions like save(), delete() and findById(..) are provided to make the work easy.

**Save Entity:**
{% highlight java %}
Book book = new Book(ctx, "Title here", "2nd edition")
book.save();
{% endhighlight %}

**Load Entity:**
{% highlight java %}
Book book = Book.findById(Book.class, 1);
{% endhighlight %}

**Update Entity:**
{% highlight java %}
Book book = Book.findById(Book.class, 1);
book.title = "updated title here"; // modify the values
book.edition = "3rd edition";
book.save(); // updates the previous entry with new values.
{% endhighlight %}

**Delete Entity:**
{% highlight java %}
Book book = Book.findById(Book.class, 1);
book.delete();
{% endhighlight %}

**Bulk Operations:**
{% highlight java %}
List<Book> books = Book.listAll(Book.class);
Book.deleteAll(Book.class);
{% endhighlight %}