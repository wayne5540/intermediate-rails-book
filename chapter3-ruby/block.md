# block


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

