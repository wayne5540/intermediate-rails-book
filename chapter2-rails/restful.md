# RESTful

## REST

REST是一種軟體架構風格，符合REST理念的系統就稱做RESTful

而REST是由動詞(verbs)、名詞(nouns)、內容形態(content types)組成的，比方說我們要訪問google首頁，那麼這個行為就是：

| verb | nouns | content types |
| ------------ | ----------- | ----------- |
| get       |  www.google.com |  html |

## RESTful

RESTful把所有的行為都切割成這三個部分，而Ruby on Rails則是遵循RESTful的框架之一，所以在rails內的動作就遵循這個風格。所以在rails內我們使用許多HTTP Request(GET、POST、PUT、DELETE..等)，伺服器（名詞）判斷你送出的Request（動詞）送出回應（內容形態）。




