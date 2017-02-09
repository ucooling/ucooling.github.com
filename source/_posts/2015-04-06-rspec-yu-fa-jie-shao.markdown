---
layout: post
title: "Rspec 语法介绍"
date: 2015-04-06 23:08:16 +0800
comments: true
categories: 
---
##关键字
* describe 和 context

以上两个关键字是帮助你组织分类的，都是可以任意套叠的.
使用一个字符串作为他们的参数，以及使用一个block来定义其上下文的范围。
写model的spec或者其他的unit test时，可以传一个Ruby的类作为describe的第一个参数，例如

```
#测试类
describe Users do
	#测试方法
	context '#test' do
		it 'should ...' do
			......
		end
	end
end
```
* let

```
let(:name) { expression }
```
let方法简单地使用后面的block创建memoized attributes.换句话说就是为后边的测试准备数据，定义的属性可以在多个例子中使用，但是不能跨多个实例调用。跟before里地代码类似，但是比before里测代码效果更好。
memozied的意思是let后面关联的block只执行一次，然后会缓存改变量，提高执行效率，有需要时才会加载，相较于before执行速度会更快。

```
let(:github) { 'ucooling.github.io' }
```
* before

let和before(:each)的区别，let不会自动初始化变量，而before(:each)会自动初始化变量，如果我期中有些测试用例不需要初始化这些变量，建议使用let，这样会节省一些时间和资源。before(:all)同样

```
class Thing
	def tests
		@tests ||= []
	end
end
describe Thing do
	before(:each) do
		@thing = Thing.new
	end
	describe "initialized in before(:each)" do
		it "has 0 tests" do
			@thing.should have(0).tests
		end
		it "can get accept new tests" do
			@thing.tests << Object.new
		end
		it "does not share state across examples" do
			@thing.should have(0).tests
		end
		it "does not have 1 count" do
			@thing.shou*ld_not have(1).tests
		end
	end
end
```
* expect

expect用来改变一个值或者抛出一个异常。

```
expect { 
BlogPost.create :title => “Hello” 
}.to change {BlogPost.count}.by(1) 
```
希望在expect块里做完之后，BlogPost.count的值要改为1



