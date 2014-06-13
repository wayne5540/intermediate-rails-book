# Chapter3 - 關於Ruby



## Ruby syntax
不知道有什麼特別要說明的地方...Orz


## String / Array / Hash
在Ruby裡面，String / Array / Hash都是類別
```
"".class #=> String
"".class.class # => Class
[].class #=> Array
[].class.class # => Class
{}.class #=> Hash
{}.class.class # => Class
```

因此我們可以呼叫各別的class method，例如：

* String：字串
```ruby
"hello world".capitalize # => "Hello World"
```
* Array：陣列
```ruby
[ "a", "b" ] << [ "c", "d", "e" ] # => [ "a", "b", "c", "d", "e" ]
```
* Hash：由key-value鍵組成的檔案形態
```ruby
{:name => "hello"}.empty?   #=> false
{}.empty?   #=> true
```

可再參考 http://www.ruby-doc.org/ 獲得更多資訊。

## map
請參考[[Rails 高級新手系列] 關於MVC-什麼東西應該放在Model](http://waynechu.logdown.com/posts/200744-about-mvc-model)內的`collect(&:id)`教學

## lambda

要解釋lambda必須先解釋Proc
Proc就是把block存成實例的方式，呼叫前加上`&`就可以轉回block：
```ruby
p = Proc.new { |x| puts x * 2 }   # 也可以寫成 p = proc { |x| puts x * 2 }
# => #<Proc:0x007ffb8c250488@(irb):66>   存成Proc的實例
# 要用時加上&
[1,2,3].each(&p)
# => 2
# => 4
# => 6
p.class
# => Proc
```

lambda基本上也是Proc的實例，只是它的形態叫做lambda
```ruby
lam = lambda { |x| puts x * 2 }
# => #<Proc:0x007ffb8c230408@(irb):5 (lambda)>  也是存成Proc的實例，但是存成lambda的形態
[1,2,3].each(&lam)
# => 2
# => 4
# => 6
lam.class
# => Proc
```

lambda和proc的主要差別有兩個

1. lambda會check參數數量，proc不會

```ruby
# lambda
lam = lambda { |x| puts x }
lam.call(2) #=> 2
lam.call(2,3) #=> ArgumentError: wrong number of arguments (2 for 1)

# proc
proc = Proc.new { |x| puts x }
proc.call(2) #=> 2
proc.call(2,3) #=> 2    proc忽略了其他參數，只傳第一個
```

2. `return`的處理不同，lambda的`return`只會跳出lambda，proc的`return`會跳出整個method


```ruby
def lambda_test
  lam = lambda { return }
  lam.call
  puts "Haha! after lam call, i survived!"
end
def proc_test
  p = Proc.new { return }
  p.call
  puts "after proc call, i survived!"
end

lambda_test # => "Haha! after lam call, i survived!"
proc_test # => #沒東西，因為在執行到puts前就跳出proc_test這個method了
```

參考資料：
http://augustl.com/blog/2008/procs_blocks_and_anonymous_functions/

http://awaxman11.github.io/blog/2013/08/05/what-is-the-difference-between-a-block/


## self
self指的就是我們物件本身
```ruby
class Foo
  def self.foo
  	"class method"
  end
  def foo
 	 "instance method"
  end
  def foobar
  	self.foo
  end
end

#class呼叫自己，self現在是Foo class
Foo.foo # => "class method"

#Foo class 的 object
f = Foo.new  # => #<Foo:0x007fd103969840>
f.foo # => "instance method"
# foobar method 裡面的 self 就是 f 這個實例
f.foobar # => "instance method"
```


## block

Block是可以暫存一段ruby code的地方，用大括號`{}`表示
舉例說明：

```ruby
def block_test
  puts "test start"
  yield
  puts "test done"
end

block_test{ puts "block working here!" }
# => "test start"
# => "block working here!"
# => "test done"
```

我們將程式存在block中並呼叫`block_test`方法，如此一來block的內容就會替代掉`yield`。

此外常見的`do....end`也是block的表示方法：
```ruby
@people.each do |person|
	puts person.name
end
```



## instance method / class method
class method就是給class層級呼叫的method
instance method則是給class的實例呼叫的method

```ruby
class Test
	# 在class內定義method時加上self代表要直接取用class
  def self.class_method
  	"class method"
  end

  def instance_method
 	 "instance method"
  end
end

#class的呼叫
Test.class_method # => "class method"
Test.instance_method # => NoMethodError: undefined method `instance_method' for Test:Class

#class實例的呼叫
test = Test.new  # => #<Test:0x007fd103969840>
test.class_method # => undefined method `class_method' for #<Test:0x007fd103969840>
test.instance_method # => "instance method"
```


## instance variable / class variable

簡單來說，class variable跟著class走，instance variable跟著實例走
class variable跟instance variable都可以被繼承
但只要是在class底下的任何實例改變了class變數，其他同個class的實例的class變數也會被改變
class variable例子：

```ruby
class Earth
	# class variable用@@定義
  @@pollution = "low"
end

class HumanBorn < Earth
	@@pollution = "high"
end
# 這時Earth的變數：
# @@pollution => "high"  #因為是class variable所以被HumanBorn更改了
```

但若class底下的實例是改變實例變數，那麼就只會在實例中被改變，不會影響到其他實例
instance variable例子：

```ruby
class Kid
	@age = 1
  def grow_up
  	@age = @age +1
  end

  def show_age
  	@age
  end
end

wayne = Kid.new
wayne.show_age # => 1
wayne.grow_up
wayne.show_age # => 2

jason = Kid.new
# wayne.grow_up改變的是wayne這個實例內的instance variable，所以不影響jason這個實例
jason.show_age # => 1
```


## Mixin / Extend / Inheritance

喔喔喔喔！Mixin ＆ Extend可以參考之前寫過的筆記：
https://logdown.com/account/posts/200503-ruby-class-and-module

另外補充Inheritance：
繼承的意思就是繼承者擁有被繼承者的特性
所以子類別可以呼叫父類別的方法

```ruby
class Parent
  @last_name = "james"
  def laugh
  	"heeeeeeeeha!"
  end
  def last_name
  	@last_name
  end
end

class Child < Parent
end

dad = Parent.new
son = Child.new
#Child可以呼叫Parent的method
papa.laugh # => "heeeeeeeeha!"
son.laugh # => "heeeeeeeeha!"
#Child也繼承了Parent的實例變數
son.last_name # => "james"
```


## override

ruby class內的method都是可以被更改的，例如我們可以更改class Array內的to_s方法：

```ruby
# 原本的to_s method
[1, 2, 3].to_s #=> [1, 2, 3]

# 重新'打開'Array這個class
class Array
  def to_s
    "hahaha! you can't convert to string right now, uh?"
  end
end
# 再試一次to_s method
[1, 2, 3].to_s #=> "hahaha! you can't convert to string right now, uh?"
```
如此一來我們就可以靈活運用各種method了。
