---
layout: post
title: "ruby 中的钩子方法"
date: 2015-05-20 21:56:59 +0800
comments: true
categories: 
---

以前从未对Ruby中的钩子方法有一个很深刻的认识，只是知道在使用的时候该去怎么引用，从看到了[这篇](http://www.sitepoint.com/rubys-important-hook-methods/)文章之后我开始有了一个新的认识，也意识到自己对Ruby中的钩子方法的认识只是停留在一个很片面的层次，今天正好抽空总结一下。

##什么是钩子方法
下面这段是对原文的翻译：
钩子方法提供当应用程序在运行时扩展应用程序的行为，假设有这样一种场景，当子类继承父类时父类能够接受到通知，或者是优雅地处理一个对象上的不可调用的方法而不是让编译器抛出异常。所有这些情况都是引用钩子方法的实例，但是他们的引用又不仅限于此，不同的框架或者库使用不同的钩子方法来实现他们的功能。

这篇文章中我们将讨论下面列表中的钩子方法：

* included
* extended
* prepended
* inherited
* method_missing

##included
Ruby提供了一种方法来使用定义在模块中的方法（模块在其他语言中被称作mixins）,模块的概念非常的简单，它是一种可以在其它地方使用的代码的集合。
例如我想写一个方法，当它被调用时能够返回一个静态的字符串，我们现在将这个方法叫做name,你可能在其它的地方同样也想使用这个那么方法，对于这样的场景，我们最后新建一个moudle,所以我们有了下面的代码：

```
moudle Person
  def name 
    puts "My name is Person"
  end
end
```
同时我们还有这样的一个类想要使用上面的moudle，我们可能会这样做

```
class User
  include Person
end
```
现在我们来执行上边的代码，看看我们会得到什么

```
User.new.name
=> My name is Person
```
但是现在我们对moudle稍微做些修改，看看这其中的included钩子方法是如何被调用的

```
moudle Person
  def self.included(base)
    puts "#{base} included #{self}"
  end
  
  def name 
    puts "My name is Person"
  end
end
```
现在我们再来执行上边的代码，看看我们会得到什么

```
User.new.name
=> User included Person
=> My name is Person
```
是的，正如我们所见，当我们在调用moudle中的方法的时候base返回的时包含改moudle得类名，当我们在调用moudle中的方法的时候会触发这里的included钩子方法。

##extended
如果你经常写Ruby程序的话，有时候你可能会使用extend来引用一个moudle，使用这种方式跟使用include有很大的不同，当使用extend来引用moudle的时候，其实是将moudle中的方法作为类方法类引入，而使用include是作为实例方法来引入，这也就是为什么我们看到上边会使用User.new.name来调用name方法
下面同样是来自于原文的一个例子：

```
module Person
  def name
    "My name is Person"
  end
end

class User
  extend Person
end

puts User.name # => My name is Person

u1 = User.new
u2 = User.new

u1.extend Person

puts u1.name # => My name is Person
puts u2.name # => undefined method `name' for #<User:0x007fb8aaa2ab38> (NoMethodError)
```
跟included类似，和extend对应的也有一个钩子方法extended.
当一个模块被其他模块或者类执行了extend操作的时候，就会触发该方法的调用。

##prepended
另一种使用moudle内部方法的方式是prepend.这种方式被定义在ruby2.0中，它跟include和extend有很大的不同，使用include或者extend引入的方法可以在base类中重新覆盖，但是如果使用的是prepend方法引入moudle中的方法，那么它会覆盖base类中的同名方法，这样正好和include、extend相反。
下面是原文中的一个例子：

```
module Person
  def name
    "My name belongs to Person"
  end
end

class User
  include Person
  def name
    "My name belongs to User"
  end
end

puts User.new.name 
=> My name belongs to User
```
再来看一下使用prepend的方式

```
module Person
  def name
    "My name belongs to Person"
  end
end

class User
  prepend Person
  def name
    "My name belongs to User"
  end
end

puts User.new.name 
=> My name belongs to Person
```
于prepend对应的也有一个钩子方法prepended方法

```
module Person
  def self.prepended(base)
    puts "#{self} prepended to #{base}"
  end

  def name
    "My name belongs to Person"
  end
end
```
当上边一段代码执行时，会看到下面的结果

```
Person prepended to User
My name belongs to Person
```
##inherited
继承是面向对象中一个非常重要的概念，Ruby是一个面向对象的编程语言，提供了从base/parent继承一个子类的方法，下面是一个例子：

```
class Person
  def name
     "My name is Person"
  end
end
 
class User < Person
end
 
puts User.new.name # => My name is Person
```
上边是一个非常简单地继承，你可能会很好奇，是什么时候会得到通知，当一个类继承另外一个类时，Ruby提供了一个叫做inherited的钩子方法，下面是一个例子：

```
class Person
  def self.inherited(child_class)
    puts "#{child_class} inherits #{self}"
  end
 
  def name
    "My name is Person"
  end
end
 
class User < Person
end
 
puts User.new.name
```
执行上边这段代码会得到下面的输出

```
User inherits Person
My name is Person
```
##method_missing
method_missing 可能是Ruby中使用最广的钩子。当我们试图去访问一个对象上不存在的方法时则会调用这个钩子方法。下面是一个简单地例子

```
class Person
  def method_missing(sym, *args)
     "#{sym} not defined on #{self}"
  end

  def name
    "My name is Person"
  end
end

p = Person.new

puts p.name     # => My name is Person
puts p.address  # => address not defined on #<Person:0x007fb2bb022fe0>
```
method_missing 接收两个参数：被调用的方法名和传递给该方法的参数。
首先Ruby会寻找我们试图调用的方法，如果方法没找到则会寻找 method_missing 方法。

*声明：本文只是对原文做了部分的翻译，想看访问原文请看文章开始的链接。*


