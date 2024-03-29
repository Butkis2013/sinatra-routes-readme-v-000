# Routes in Sinatra

## Overview 

In this lesson, we'll explore routes and how they direct users to different parts of your website

## Objectives

1. Define routes as the part of an application that connect HTTP requests to a specific method in your application code
2. Construct a route by matching an HTTP method/verb, like `GET` or `POST` with a URL-matching pattern
3. Explain the syntax of routes as Ruby methods that are called with arguments and blocks

## What are Routes?

Every time a person clicks on a link, types in a URL, or submits a form, they are making HTTP requests to a specific URL on your application. Routes are the part of an application that connect HTTP requests to a specific method in your application code built to handle responding to such a request (that part of code is called a Controller Action).

Let's say we have a web application that helps doctors track the medications they've prescribed. The doctor would need an easy way to see all the medicines she's prescribed.

In the application's navigation is a link "All Medicines". When the user clicks on that link, it directs the browser via the link's `HREF` attribute to the URL "/medicines" on the domain. The URL "/medicines" must trigger the method in your application built to load the appropriate medicines and send an HTML response with that list of medicines.

Also in the application's navigation is a link "All Patients". When the user clicks on that link, it directs the browser via the link's `HREF` attribute to the URL "/patients" on the domain. The URL "/patients" must trigger the method built to load the appropriate patients and send an HTML response with a list of patients.

These mappings are called Routes.

Mapping `http://localhost:9393/medicines` to a Sinatra Action for a response:

![Sinatra Route Example 1](https://dl.dropboxusercontent.com/s/unlxkqbg841b1xh/2015-09-15%20at%209.46%20PM.png)

Mapping `http://localhost:9393/patients` to a Sinatra Action for a response:

![Sinatra Route Example 2](https://dl.dropboxusercontent.com/s/t3mgmc0qwr9hfsi/2015-09-15%20at%209.48%20PM.png)

Users interact with our application by requesting specific URLs via HTTP. URLs are the interface to a web application.

How does our application know what to show a user based on the web request they sent? Through the routes we program in our application. Being able to draw a route in a web application to respond to an HTTP request is the absolute first step in building anything on the web.

In Sinatra, a route is constructed by pairing an HTTP method/verb, like `GET` or `POST` with a URL-matching pattern, i.e. a string that matches what users type in to their browser when they want to visit our webpage. We'll see an example in just a moment.

## How Do Routes Work?

Routes match the web request sent by a client to some code in our application that tells the app what data and templates to send back to the client.

Let's go into more detail on our two routes, `/medicines` and `/patients`, from the example application above.

When our doctor types into the browser, `http://localhost:9393/medicines`, our application gets a request: `GET /medicines`.

Our Sinatra application will match this request to a route that is defined in a controller.

The matching route defined in the controller would look like this:

```ruby
get '/medicines' do
	# some code to get all the medicines
	# some code to render the correct HTML page
 end
```

Once the request has been matched to the route in the controller, called the **controller action**, then it executes the code inside of the controller action's block, as shown below:

```ruby
# medicines_controller.rb

get '/medicines' do
  @medicines = Medicine.all

  erb :'medicines/index.html.erb'
end
```

**Advanced:** You might be wondering what this line does for us: `erb :'/medicines/index.html.erb`. We'll learn much more about ERB soon. For now, all you need to know is that that line of code identifies a file that contains a combination of HTML and Ruby code and sends it back to the client to be rendered in the browser. 

Let's run through this specific scenario.

1. The HTTP request verb, `GET` matches the `get` method in our controller. The `/medicines` path in the HTTP request matches the `/medicines` path in our controller method. Yay! We've successfully matched the request to a controller action!

2. Once our app has found its match, it will run the code in the block that accompanies the route. In this case, the block will query the `Medicine` table for all of its medicines. The collection of `Medicine` instances that such a query returns is stored in the variable `@medicines`.

3. Finally, the `@medicines` collection of objects is rendered via the `index.html.erb` file: `views/medicines/index.html.erb`. This file is the HTML (+ some Ruby code––more on that later) that we want our users to see when they request to see all of the medicines. The HTML returned from the template is sent by the controller action as a response to the user.

4. If no matching route for the web request is found, our application will respond with a 404 informing the user's browser that our application cannot find a match for that request URL.

### Breaking Down Route Definition

Now that we know how routes work, let's take a closer look at their syntax. 

Sinatra is what is known as a Domain Specific Language, or DSL. A DSL is a specialized, situation-specific language. Sinatra was built expressly for the purpose of creating web applications with Ruby. It is written with Ruby and the code we'll be using to build our Sinatra apps is Ruby codes. 

The route below:

```ruby
get '/medicines' do 
	# some code to get the medicines and render the correct HTML file
end
```

is actually a plain old-fashioned Ruby method that is getting called with an argument and a block. 

Here, the `get` method is called with an argument of `'/medicines'` and is being invoked with a block (the code between the `do`/`end` keywords. 

Here's another way to write it that might look familiar:

```ruby
get('/medicines') { some code }
```

The `get` , or the `post`, or `delete` methods for that matter, will be invoked if Sinatra matches the HTTP method (`GET`, `POST`, etc) *and* the URL, in this case `'/medicines'`, to a route defined in the controller. 

**Advanced:** If you're curious, check out the Sinatra source code, [especially that code that defines the `#get`, `#post`, `#patch` and `#delete` methods](https://github.com/sinatra/sinatra/blob/master/lib/sinatra/base.rb#L1367). 

### Resources

- [Routes in Sinatra](http://www.sinatrarb.com/intro.html#Routes)

<a href='https://learn.co/lessons/sinatra-routes-readme' data-visibility='hidden'>View this lesson on Learn.co</a>
