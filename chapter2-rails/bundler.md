# Bundler

Bundler是管理gem的工具，可以偵測gem的升級、相依性等，並能確保在不同的環境下都使用同樣的Gem。
我們可以用Gemfile管理各種Gem，`bundle install`就會幫我們安裝正確的Gem版本。

```
source 'https://rubygems.org'
gem 'rails', '4.0.2'
gem 'sass-rails', '~> 4.0.0'
gem 'uglifier', '>= 1.3.0'
gem 'coffee-rails', '~> 4.0.0'
```
