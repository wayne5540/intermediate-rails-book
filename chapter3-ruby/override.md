# override


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
