# Unobtrusive Javascript
通常javascript的指令都是在使用者產生某種行為後運行，比方說onclick就會寫成這樣：


```html
<a id="test" herf="#" onclick="alert('hello world')">Click Me</a>
```

但是這樣就會造成view裡面有太多的指令，等到我們想要修改的時候就會變得很複雜
Unobtrusive Javascript的概念就是把藏在html裡面的script全部分離出來，改用listener接收使用者的行為，以剛剛的例子來看就會是這樣：


```html
<a id="test">Click Me</a>
<script type="text/javascript">
    $("#test").onclick(function(){
    	alert("hello world");
    });
</script>
```
