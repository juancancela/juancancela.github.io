---
layout: post
title: "Advanced Javascript: Typescript In Action"
date: 2017-4-06 16:54:46
author: Juan Carlos Cancela
categories: 
- blog
- javascript
- typescript
- coding
img: 2017-12-06-typescript-cover.png
thumb: ts.jpg
---

## Description

During a project I was working on a couple of months ago, I got the opportunity to work for my very first time with Typescript.


Typescript is a language that "basically formalizes a static type system that describes JavaScript's dynamic types, but it describes them at development time", as Anders Hejlsberg -creator of C# and Typescript itself- have said.


## Typescript in a nutshell

Its Javascript

- Superset of Javascript that compiles to JS
- Transpilable to plain Javascript
- Interoperates with existing code

...but better!

- Type annotations
- Interfaces
- Classes
- Generics
- Enums
- Async/Await
- Decorators
- ...more

Lets dig a bit more into some these points:

- The fundamental aspect to understand about Typescript is the fact that **it is a superset of Javascript**. The direct implication of this is that any current, plain javascript application is, indeed, a Typescript application. You dont need to change a single line of your application code to start using it, right away.

- Secondly, Typescript source code is transpilable to plain Javascript, that is, through a bundling tool like Browserify or Webpack -and using the appropiate loader- your Typescript source is transformed to javascript that any javascript engine can execute.

- One direct implication from previous points is that your newly Typescript code can easily interact with your existing Javascript one. **Typescript starts as Javascript, and finishes as Javascript as well**

## Typescript features

As mentioned before, Typescript provides Javascript with a powerful typing system. Thanks to that, it is possible to use features available on other languages, in Javascript.

Let's review some of these features and direct examples where its useful:

## Interfaces

An interface can be thought a contract that specifies inputs and outputs of a given component. That is, the operations that we can execute on any component that implements such interface.
Consider following example: We have a Repository interface that has four operations (create, read, delete, update) we could define an interface as follows: 

{% highlight javascript %}
interface Repository {
    create(instance);
    delete(id);
    read(id);
    update(id, instance);
}
{% endhighlight %}

Then, we might have a class that implements Repository interface, like this:

{% highlight javascript %}
class MySqlRepository implements Repository {
    create(instance){ /...(implementation)/ };
    delete(id) { /...(implementation)/ };
    read(id) { /...(implementation)/ };
    update(id, instance) { /...(implementation)/};
}
{% endhighlight %}

Now consider this: Suppose we have an application that uses this MySqlRepository implementation to connect to a MySql instance. For whathever reason, we need to change from MySql to PostgresSQL.
Having interfaces we could implement a new PostgresSqlRepository class like this:

{% highlight javascript %}
class PostgresSqlRepository implements Repository {
    create(instance){ /...(implementation)/ };
    delete(id) { /...(implementation)/ };
    read(id) { /...(implementation)/ };
    update(id, instance) { /...(implementation)/};
}
{% endhighlight %}

that implements Repository interface, and then, safely switch between available implementations. So, an interface is a mechanism to enforce a particular piece of code to provide a well defined list of operations that needs to be implemented.


## Type Inference

Typescript compiler provides a powerful type inference implementation, that allows compiler to infer lots of information about our Javascript code. Consider this case: 

{% highlight javascript %}
function add16(val){
    return 16 + val;
}
var v = add16("val"); // TS type inference mechanism will warn us that we are adding a number to a string

{% endhighlight %}

Typescript compiler's type inference mechanism is so powerful that makes sense to use it even if we are now going to code a single line of Typescript.

Typescript infers this using four simple rules: 


**Inference by Assignment**: 

Consider following example:

{% highlight javascript %}
type Adder = (a: number, b: number) => number;
let foo: Adder = (a, b) => a + b;

let foo2: Adder = (a, b) => {
    a = "hello";
    return a + b; //Error cannot assign string to number
}
{% endhighlight %}

As it can be seen, **a** is declared as a number, but then a string is assigned. Typescript will recognize this pattern, and let us know that looks like theres an error in code, since a variable of type number is being assigned with an string.


**Inference by Variable Definition**: 

{% highlight javascript %}
let foo = 123;
let bar = "Hello";
foo = bar; //Error: cannot assign string to number
{% endhighlight %}

in this case, foo is initially assigned value 123, then infers its a number, and bar with an string. When we try to assifn foo to bar, Typescript will let us know we are trying to assign a string to a number.


**Inference by Return**: 

Suppose we have a function addNumbers as follows:

{% highlight javascript %}
function addNumbers(a: number, b: number){
    return a + b;
}

var a = "";
a = addNumbers(1,2); // Error cannot assign a number to a string
{% endhighlight %}

Again, Typescript type inference mechanism will let us know that based on the return type of the addNumbers function, theres an error, since we are trying to assign it to a string value (**a**).


**Inference by Structure**: 

Finally, Typescrip is able to infer type of variables using structure:

{% highlight javascript %}
let foo = {
    a: 1,
    b: 2
}

foo.a = "hello"; //Cannot assign a string to a number
{% endhighlight %}

Property a of foo object was initially set to 1, then Typescript will output an error when we try to assign an string to it.

## Generics

Following Repository interface example, we can see that read function can return anything:

{% highlight javascript %}
interface Repository {
    create(instance);
    delete(id);
    read(id);
    update(id, instance);
}
{% endhighlight %}

Ideally, we would like to define the return type of the read function with a provided value. Kind of a parameter to the interface / class definition itself. One way to do it could be to set the return of read to a particular type, in example:

{% highlight javascript %}
interface PersonRepository {
    create(instance);
    delete(id);
    read(id):Person;
    update(id, instance);
}
{% endhighlight %}

Now, Typescript will check that the return type of read needs to be an instance of Person class. So, we will probably have an implementation as follows:

{% highlight javascript %}
class PersonPostgresSqlRepository implements PersonRepository {
    create(instance){ /...(implementation)/ };
    delete(id) { /...(implementation)/ };
    read(id): Person { /...(implementation)/ };
    update(id, instance) { /...(implementation)/};
}
{% endhighlight %}

PersonPostgresSqlRepository is a final class that implemented PersonRepository, and thus, it enforces read function to return an object of type Person.
One problem we will find with this approach is that we could eventually be duplicating lot of code. What if we have hundreds of objects like Person in our app? 
For such cases, generics are a handy abstraction that enables parametrization of interfaces and classes.
Consider following Reposutory interface using generics:

{% highlight javascript %}
interface Repository<T> {
    create(instance);
    delete(id);
    read(id):T;
    update(id, instance);
}
{% endhighlight %}

Now, read function returns type T, which we are able to specify. Now, we could have something like this:

{% highlight javascript %}
class PostgresSqlRepository<T> implements Repository<T> {
    create(instance){ /...(implementation)/ };
    delete(id) { /...(implementation)/ };
    read(id): T { /...(implementation)/ };
    update(id, instance) { /...(implementation)/};
}

let personRepository = new PostgresSqlRepository<Person>();
let departmentRrepository = new PostgresSqlRepository<Department>();
...
{% endhighlight %}

As you can see, you can think of generics as type placeholders.

# Code Examples

I have created a clean, Express Hello World app that you can use for any Typescript project. In this project, Webpack and Typescript are properly configured to transpile code to ES5 target: [repo](https://github.com/juancancela/ts-workshop-2017)

Also, theres a Pull Request with some example refactors to take a look: [refactors](https://github.com/juancancela/ts-workshop-2017/pull/1)