# Chapter2 - 關於Rails
## RESTful
REST是一種軟體架構風格，符合REST理念的系統就稱做RESTful

而REST是由動詞(verbs)、名詞(nouns)、內容形態(content types)組成的，比方說我們要訪問google首頁，那麼這個行為就是：

| verb | nouns | content types |
| ------------ | ----------- | ----------- |
| get       |  www.google.com |  html |

RESTful把所有的行為都切割成這三個部分，而Ruby on Rails則是遵循RESTful的框架之一，所以在rails內的動作就遵循這個風格。所以在rails內我們使用許多HTTP Request(GET、POST、PUT、DELETE..等)，伺服器（名詞）判斷你送出的Request（動詞）送出回應（內容形態）。




## Routing
Routing的概念就像是總機小姐，當你打電話進一間大公司時，都會有總機小姐詢問你要辦理什麼業務，然後幫你轉接給業務承辦人員，然後再由承辦人員完成你申請的業務。Routing也是做一樣的事，當你想要到某個頁面（辦理業務）時，Routing就會幫你找到負責這個業務的承辦人員（Controller Action）然後業務人員就會幫你辦完業務，給你你想要的頁面（View），大功告成。
Example:

```ruby
#指定動作跟controller action
get 'products/:id' => 'catalog#view'

#自動按照rails內建的RESTful路徑判斷controller action
resources :products

#member & collection
resources :products do
#會產生products/:id/short
 	member do
		get 'short'
		post 'toggle'
	end
#會產生products/sold
	collection do
		get 'sold'
	end
end
```


## Rack

> Rack provides a minimal interface between webservers supporting Ruby and Ruby frameworks.

簡單來講rack就是讓ruby以及ruby框架跟server互動的橋樑，所有與server的互動都會經過rack。
所以確切的說，我們寫的所有ruby code都是在跟rack互動，rack會負責再幫我們跟server互動，扮演秘書的角色。
對webserver的開發者來說，世界上有千百種framwork，每新增一個framwork就樣讓server支援它會是一件很恐怖的工作，所以就需要一個共同的規範`rack SPEC`，只要framwork都遵守著rack spec，server就只需要處理rack送來的訊號就可以了。而這個SPEC其實就是一個帶environment參數的call method，由rack跟server拿這個enviroment應該要回傳的資料，並回串一個由三個主要區塊「HTTP狀態」、「HTTP Headers」、「HTTP內容」組成的字串。

而所謂的Rack middleware就是指在Rack 和 Application 中間的application，我們的每個request其實都是經過很多層的middleware才傳到Rails App中，所以有些不需要給rails處理的工作就可以在middleware中做，對效能有幫助。

TODO: Learn what is `rails Metal`


參考資料：
http://webcache.googleusercontent.com/search?q=cache:eBrDs9kEBksJ:wp.xdite.net/%3Fp%3D1557+&cd=1&hl=zh-TW&ct=clnk&gl=tw


http://www.whatcodecraves.com/articles/2012/07/23/ruby-on-rack


http://rack.github.io/


http://guides.rubyonrails.org/rails_on_rack.html#resources


middleware應用：http://railscasts.com/episodes/151-rack-middleware

## Rake (打錯了，應該是要打Rack，沒差～留著當筆記...囧rz)
rake可以讓每個script之間可以有清楚的相依性，且能夠只執行程式需要執行的部分，作用大概就跟管家很像，當我們要管家去掃地時，不用告訴管家你要先去拿掃把、然後走去庭院、然後開始掃地，只要單純地說「去掃地」就好了，而在這當中「拿掃把」、「走去庭院」、「開始掃地」都是不同的script，管家（rack）會依序執行它。而且他也會記得他掃過的區域，不會每次掃地都一直重新掃一樣的地方。
例如我們可以寫一個`dev.rake`的檔案，並用`rake dev:build`的方式執行下面這段script中的build task

```ruby
namespace :dev do
	desc "Rebuild system"
	task :build => ["tmp:clear", "log:clear", "db:drop", "db:create", "db:migrate"]
	task :rebuild => [ "dev:build", "db:seed" ]
end
```
rake就會按照順序執行完後面的一系列rake script。


## Bundler
Bundler是管理gem的工具，可以偵測gem的升級、相依性等，並能確保在不同的環境下都使用同樣的Gem。
我們可以用Gemfile管理各種Gem，`bundle install`就會幫我們安裝正確的Gem版本。

```
source 'https://rubygems.org'
gem 'rails', '4.0.2'
gem 'sass-rails', '~> 4.0.0'
gem 'uglifier', '>= 1.3.0'
gem 'coffee-rails', '~> 4.0.0'
```


## Unobtrusive Javascript

通常javascript的指令都是在使用者產生某種行為後運行，比方說onclick就會寫成這樣：

`example.html`
```html
<a id="test" herf="#" onclick="alert('hello world')">Click Me</a>
```

但是這樣就會造成view裡面有太多的指令，等到我們想要修改的時候就會變得很複雜
Unobtrusive Javascript的概念就是把藏在html裡面的script全部分離出來，改用listener接收使用者的行為，以剛剛的例子來看就會是這樣：

`example.html`
```html
<a id="test">Click Me</a>
```
`example.js`
```javascript
$("#test").onclick(function(){
	alert("hello world");
});
```

## I18n
I18n就是internationalization，主要的意思是讓程式可以根據使用者不同的地區顯示不同的結果，最常見於多國語系的網站中，在Rails中有一個`en.yml`就是記錄語系的檔案，我們也可以自己新增`zh-TW`的語系：

config/locales/en.yml
```yaml
en:
  hello: "Hello world"
zh-TW:
  hello: "哈羅"
```

如此一來當使用者使用en語系時`t("hello")`就會輸出`Hello world`，切換為zh-TW語系時`t("hello")`就會輸出成`哈羅`，很方便。

