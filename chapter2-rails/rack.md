# Rack

> Rack provides a minimal interface between webservers supporting Ruby and Ruby frameworks.

簡單來講rack就是讓ruby以及ruby框架跟server互動的橋樑，所有與server的互動都會經過rack。
所以確切的說，我們寫的所有ruby code都是在跟rack互動，rack會負責再幫我們跟server互動，扮演秘書的角色。


對webserver的開發者來說，世界上有千百種framwork，每新增一個framwork就樣讓server支援它會是一件很恐怖的工作，所以就需要一個共同的規範`rack SPEC`，只要framwork都遵守著rack spec，server就只需要處理rack送來的訊號就可以了。而這個SPEC其實就是一個帶environment參數的call method，由rack跟server拿這個enviroment應該要回傳的資料，並回串一個由三個主要區塊「HTTP狀態」、「HTTP Headers」、「HTTP內容」組成的字串。


而所謂的Rack middleware就是指在Rack 和 Application 中間的application，我們的每個request其實都是經過很多層的middleware才傳到Rails App中，所以有些不需要給rails處理的工作就可以在middleware中做，對效能有幫助。

TODO: Learn what is `rails Metal`


參考資料：
* http://webcache.googleusercontent.com/search?q=cache:eBrDs9kEBksJ:wp.xdite.net/%3Fp%3D1557+&cd=1&hl=zh-TW&ct=clnk&gl=tw
* http://www.whatcodecraves.com/articles/2012/07/23/ruby-on-rack
* http://rack.github.io/
* http://guides.rubyonrails.org/rails_on_rack.html#resources
* middleware應用：http://railscasts.com/episodes/151-rack-middleware
