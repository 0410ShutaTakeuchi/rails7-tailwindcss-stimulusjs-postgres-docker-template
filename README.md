# Rails7 x TailwindCSS x Stimulus.js x PostgreSQLのDockerテンプレート

```
バックエンド：　　Ruby on Rails 7
フロントエンド：　TailwindCSS & Stimulus.js
データベース：　　PostgreSQL
```

## 構築方法

### 1. テンプレートリポジトリを取得
「YOUR_REPOSITORY_NAME」を作成するアプリ名に指定する
```bash
$ mkdir YOUR_REPOSITORY_NAME
$ cd YOUR_REPOSITORY_NAME
$ git clone git@github.com:0410ShutaTakeuchi/rails7-tailwindcss-stimulusjs-postgres-docker-template.git .
$ rm -rf .git/
```

### 2. Railsを作成
```bash
$ docker-compose run web rails new . --force --database=postgresql --css tailwind
```

### 3. データベースを作成
##### 1. `database.yml`を編集
1. 以下のコードに書き換える  
2. 「APP_NAME_development」をデータベース名に書き換える

  
###### ※test用のDBは作成しないようにコメントアウトしている
```bash
default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: <%= ENV["POSTGRES_USER"] %>
  password: <%= ENV["POSTGRES_PASSWORD"] %>
  host: db

development:
  <<: *default
  database: APP_NAME_development

# test:
#   <<: *default
#   database: APP_NAME_test

production:
  <<: *default
  url: <%= ENV['DATABASE_URL'] %>
```

##### 2. 作成＆マイグレーションのコマンドを実行
```
$ docker-compose run --rm web rails db:create
$ docker-compose run --rm web rails db:migrate
```

### 4. `Procfile.dev`を編集
以下のコードに書き換える
```
web: bin/rails server -p 3000 -b "0.0.0.0"
css: bin/rails tailwindcss:watch
```

### 5. アプリを起動
```
$ docker-compose up
```