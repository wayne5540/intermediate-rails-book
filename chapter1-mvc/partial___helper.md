# 什麼東西應該放在 partial / 什麼東西應該放在 helper

partial負責處理大片的html code，或是之後要利用ajax render出來的片段。

helper則負責處理跟邏輯判斷有關的東西，比方判斷文章發佈狀態或是讀取變數的值等等。

topics_helper.rb
```ruby
module TopicsHelper
#需要邏輯判斷的工作留給helper
def render_published_topic_name_path(topic)
    link_to(topic.name, topic_path(topic))  if topic.is_published?
end
```
_topic_info.html.erb
```erb
#partial只需要負責html的部分
<table>
  <thead>
    <tr>
      <td>Topic name</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><%= render_published_topic_name_path(topic) %></td>
    </tr>
  </tbody>
<table>
```

