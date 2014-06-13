# Routing

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
