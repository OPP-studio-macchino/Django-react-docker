## Useage

### frontend フォルダを作成

mkdir frontend
cd frontend

### react 環境構築

mkdir templates static
cd static
mkdir frontend css img
cd ..
mkdir src
cd src
mkdir components
cd ..
sudo apt-get update
curl -sL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install nodejs
npm init -y
npm i webpack webpack-cli --save-dev

#### package.json を開き下記を追記

"scripts": {
"dev": "webpack --mode development --entry ./src/index.js --output-path ./static/frontend",
"build": "webpack --mode production --entry ./src/index.js --output-path ./static/frontend",
},

npm i babel-loader @babel/core @babel/preset-env @babel/preset-react --save-dev
npm i react react-dom --save-dev

### react のバージョンが 17.0.1 であるのに対し、

### material-ui/core@4.11.0がサポートしている react のバージョンは 16.8.0。（＝最新の React プロジェクトに対 応していない。）

### なので、依存関係を解決できずエラーになって、インストールができないことに。。

### ここで、npm install のあとに、オプションで、--save --legacy-peer-deps を追加。

npm install --save --legacy-peer-deps @material-ui/core
npm install -g n
npm install @babel/plugin-proposal-class-properties --legacy-peer-deps
npm install react-router-dom --legacy-peer-deps
npm install @material-ui/icons --legacy-peer-deps
touch babel.config.json

### 下記を babel.config.json をコピペ

#################################
{
"presets": [
[
"@babel/preset-env",
{
"targets": {
"node": "18"
}
}
],
"@babel/preset-react"
],
"plugins": ["@babel/plugin-proposal-class-properties"]
}
#################################

touch webpack.config.js

### 下記を webpack.config.js へコピペ

#################################
const path = require("path");
const webpack = require("webpack");

module.exports = {
entry: "./src/index.js",
output: {
path: path.resolve(\_\_dirname, "./static/frontend"),
filename: "[name].js",
},
module: {
rules: [
{
test: /\.js$/,
exclude: /node_modules/,
use: {
loader: "babel-loader",
},
},
],
},
optimization: {
minimize: true,
},
plugins: [
new webpack.DefinePlugin({
'process.env.NODE_ENV' : JSON.stringify('production')
})
]
}
#################################

touch src/index.js
mkdir templates/frontend
touch templates/frontend/index.html

### templates/frontend/index.html に下記をコピペ

#################################

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  {% load static %}
  <!-- cssを追加する場合 -->
  <link rel="stylesheet" href="{% static "css/index.css" %}">
</head>
<body>
  <div id="main">
      <div id="app"></div>
  </div>

  <script src="{% static 'frontend/main.js' %}"></script>
</body>
</html>
#################################

## views.py

#################################
def index(request, \*args, \*\*kwargs):
return render(request, 'frontend/index.html')
#################################

touch urls.py
#################################
from django.urls import path
from .views import index

urlpatterns = [
path('', index)
]
#################################

### 各種ルーティング作業後

touch src/components/App.js
