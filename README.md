# study-rails
railsの勉強

Railsはrubyで動くフレームワーク

rubyを更新する場合は、アプリのGemfileを変更する。

Gemfile
```
source 'https://rubygems.org'
git_source(:github) { |repo| "https://github.com/#{repo}.git" }

ruby '3.1.3'

# Bundle edge Rails instead: gem 'rails', github: 'rails/rails'
gem 'rails', '6.1.7.7'
# Use postgresql as the database for Active Record
gem 'pg', '>= 1.4'
・・・
```

まずはrailsを6.1.7.7のままrubyを3.3.13にする

Rails 6.1.7.7とRuby 3.3の組み合わせは正式な互換性が保証されていない

### ブランチ用意
```
git switch -c upgrade-ruby
```

Gemfile変更
```
ruby "3.3.13"
```

```
bundle list
```

#### 結果
```
Gems included by the bundle:
  * actioncable (6.1.7.7)
  * actionmailbox (6.1.7.7)
  * actionmailer (6.1.7.7)
  * actionpack (6.1.7.7)
・・・
```
### Ruby実行環境自体の更新

```
which ruby
```
#### 結果
```
/Users/kunii/.rbenv/shims/ruby
```
.rbenvが含まれるのでrbenvで管理中

```
brew update
brew upgrade rbenv ruby-build
rbenv install --list-all | grep '3\.3\.'

rbenv install 3.3.13
rbenv local 3.3.13
```

```
gem install bundler    # まずBundlerを準備
bundle update --ruby   # Gemfile.lockのRubyバージョンを更新
```

#### 結果
```
Bundle updated!
```
が出たなら以下は不要、出なかったら
```
bundle install # 依存Gemをインストール
```

### 検証
Ruby 3.3.13でRails 6.1.7.7の初期化とZeitwerkのチェック

バージョンの確認
```
bundle exec rails -v
```
#### 結果
```
Rails 6.1.7.7
```

Zeitwerkのチェック
```
bundle exec rails zeitwerk:check
```
#### 結果
```
・・・
Hold on, I am eager loading the application.
All is good!
```
良い結果

### アセットのコンパイルを確認

現在のアプリはWebpacker 5.4.4とSassC 2.4.0を使用しているため、次の確認が重要
```
RAILS_ENV=production bundle exec rails assets:precompile
```

#### 結果
```
・・・
Everything's up-to-date. Nothing to do
```
良い結果

```
git add .
git commit -m "Upgrade Ruby from 3.1.3 to 3.3.13"
```
