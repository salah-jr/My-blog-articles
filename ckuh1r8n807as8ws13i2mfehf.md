---
title: "Everything You Need To Know about the N+1 problem"
seoTitle: "How to deal with N+1 problem in PHP Laravel and what is eager loading"
seoDescription: "In this article, We will dig deep into the N+1 problem that could happen in most ORMs like Laravel Eloquent and solving it with eager loading."
datePublished: Thu Oct 07 2021 14:40:27 GMT+0000 (Coordinated Universal Time)
cuid: ckuh1r8n807as8ws13i2mfehf
slug: everything-you-need-to-know-about-the-n-plus-1-problem
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1633551974672/RAIrky-Tt.png

---

In this article, We will dig deep into one of the popular performance issues called the N+1 problem that could happen in most ORMs (Object Relational Mapper) tools. We will look over it and Discuss the available solutions. On top of that, I'll explain some of the data loading strategies like Lazy loading, Eager loading and Lazy eager loading.

\*The code in this article is written using PHP and Laravel syntax, but the concept is the same for any other language. Also, I will use the [Laravel-debugbar](https://github.com/barryvdh/laravel-debugbar) package to show you the backstage operations such as DB queries, request duration and memory usage. \*

## Lazy loading

Suppose you have a small gallery app with a database that contains two tables the albums table and the images table, these two tables have a one-to-many relationship (Each album has many images and the image is related to one album). Lazy loading means the loading data from the relationship will be on hold until we need it by accessing the property 'images'. Let's look at that practically

```plaintext
$albums = Album::get();
foreach($albums as $album) 
{
    /*
     * Loop through the albums and retrieving the images 
     * for each album in the database.
     */
     $images = $album->images;
     dump($images->toArray());
}
```

Here the lazy loading happens, in the First query we get all the albums from the database ( 1 query ) and inside the `foreach` we get the related images for every single album we have ( N queries ), imagine you have a thousand albums. Ouch! 😵 That will lead to executing a thousand queries to a database! Which will cause the famous N+1 problem. This screenshot shows us what happens when we execute this code.

![scrnli_10_6_2021_5-05-31 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633532682335/KLf6__iPD.png align="left")

The debug bar shows us that the previous code executed 1001 queries (N + 1) using 30MB of memory and taken almost 13 seconds to complete, Which gives rise to big trouble in your application.

\*\*So, What will happen if you have a million records?!\*\*🤔

The solution is simple, just use eager loading to solve the problem caused by lazy loading...

## Eager loading

As I said in lazy loading we can't access the lazy-loaded data for related models until we access the relationship as property `$album->images;`. But, In **Eager loading** you can load the data from the Eloquent relationship at the time you query the parent model in our case the parent is the `Album` model. Which fantastically prevent the N+1 problem 😋.

Before we dive into Laravel Eloquent and how it effortlessly deals with this problem in various ways, I will first show you how to build eager loading yourself using native **PHP's PDO class** to solve this problem.

#### With native PHP

* Getting all albums and list the IDs
    

```plaintext
$albums = $pdo->query("SELECT * FROM `albums`");
$ids = [];
foreach($albums as $album) {
    $ids[] = (int) $album['id'];
}
//setup list of ids for using later to get the related images
$albumIds = implode(',', $ids);
```

* Getting all images and list the IDs
    

```plaintext
$imageIds = [];
/*
* Retrieving all the images at once instead of retrieving 
* the images for each album
*/
$allImages = $pdo->query("SELECT * FROM `images` WHERE `album_id` IN ( {$albumIds} )");
foreach($allImages as $image) {
    $ImageIds[$image['album_id']]
}
```

* Dumping the data
    

```plaintext
//fetching data from the lists we make
foreach ($albums as $album) {
    var_dump($album);
    var_dump($imageIds[ $album['id'] ]);
}
```

In this example we will execute only two queries instead of 1001 queries, One to retrieve all of the albums and another to retrieve all the images for all of the albums by using of `IN` operator. The memory and request duration also will be decreased significantly.

\*\* In Laravel this is much easier thanks to modern ORMs like Eloquent \*\*

### Laravel Eager Loading solutions

**1\. Using**`with()` method

You can use the `with()` method to define which relationships should be eager loaded with the parent record by adding the relation name as a parameter

```plaintext
$albums = Album::with('images')->get();
foreach($albums as $album){
      // Print the album related images
      echo $album->images;
}
```

Let's see the debug bar results 😍,

![scrnli_10_6_2021_11-19-28 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633555108667/Ak9TKz7wB.png align="left")

And now Only **two queries** are executed instead of **1001** in lazy loading with memory usage of 25MB and request duration 578ms instead of 13s a huge difference right?

You can also eager load multiple relations by passing them as an array parameter. `$albums = Album::with(['images', 'creator'])->get();`

Using the nested eager loading to load a relationship from a relationship. `with('images.photographer')`

Or minimize the memory usage by only loading specific columns `with('images:title,size')` , Try that.

**2\. Using**`$with` property

Just define this property to your model class to always load the relations when the model (Album) is retrieved

```plaintext
//The relationships that should always be loaded
protected $with = ['images', 'creator'];
```

To except some relations from loading use the `without()` method.

```plaintext
$albums = Album::without('creator')->get();
```

If you have a lot of relations and you want to specify the relations you only want just use the `withOnly()` method to override the `$with` property in your model.

```plaintext
$albums = Album::withOnly('images')->get();
```

#### Catch the bugs earlier with `preventLazyLoading()` method

This method is quite different because It will throw an exception if there are any lazy-loaded data in your application, adding it into the `boot()` method at `AppServiceProvider` class, Will help you to perceive the N+1 problem faster and solve it by any of the two solutions explained above.

```plaintext
public function boot()
{
    Model::preventLazyLoading(! app()->isProduction());
}
```

*Here is the thrown exception:*

![scrnli_10_7_2021_12-31-07 AM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1633560016172/2zfZbVfJc.png align="left")

There is an amazing package called [Laravel-query-detector](https://github.com/beyondcode/laravel-query-detector), It does the same thing too, But by alerting the user with the N+1 problem instead of throwing an exception.

## Lazy-eager loading

This loading strategy combines both eager and lazy loading; loading of relationship data will be on hold until a specific logic happens

```plaintext
$albums = Album::get();
if(count($albums) < 100)
{
    $albums->load('images');
}
```

## Conclusion

The N+1 queries problem is a very common issue, knowing and understanding the cause of the issue is the most important solution for it. In this article, We learned how to detect the N+1 problem and how to deal with it in different ways. We wrote a PHP native solution in addition to learn some Laravel Eloquent tools that use eager loading to reduce the number of queries. We also threw light on some loading strategies and the difference between them.  

**Thanks for reading! If you liked this article and want more content like this, Subscribe to my newsletter and make sure to follow me on** [**Twitter**](https://twitter.com/Sala7JR) **and** [**Linkedin**](https://www.linkedin.com/in/salah96/) **👋**