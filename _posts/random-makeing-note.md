---
title: 如何用 Electron 制作一个抽号的程序
date: 2023-10-01 18:55:46
tags: Electron
categories: Programming
---
电子包写的随机抽号
<!-- more -->
{% btn https://github.com/acidec/random, 代码 %}

## 技术栈选择
抽号机事给普通人用的，他们对于这些大概一窍不通，所以不能写纯 TUI 的。
要易用，有图形界面，像个正常软件一样，界面可以简单不能简陋。
我直接想到了电子包这个梗，简单易用出图形界面，会一点前端就行。
~~用电子包写软件，我们都有光明的未来~~
## 开始

### 先决条件
首先你得有`nodejs`、可用的网络（别用残废的China Mainland局域网）、`git`

{% btn https://nodejs.org/en, nodejs %}
&
{% btn https://mirrors.bfsu.edu.cn/github-release/git-for-windows/git/LatestRelease/, git %}


### 配置Git
```powershell
git config --global user.name 名字 //可以随便填
git config --global user.email 邮箱 //要能用的，要是就看看代码也可以随便填
git config --global http.sslVerify false
```

### 创建项目
```powershell
mkdir random && cd random
npm init
```
然后他开始问问题，实话回答就行。

### 启动脚本
最标准的，参考文档的启动脚本，创建一个``main.js``，填入以下内容
```JavaScript
const { app, BrowserWindow } = require('electron')

const createWindow = () => {
  const win = new BrowserWindow({
    width: 1600,
    height: 1000,
    title:"抽号机"
  })

  win.loadFile('index.html')
}

app.whenReady().then(() => {
  createWindow()
})

app.on('window-all-closed', () => {
    if (process.platform !== 'darwin') app.quit()
  })

```
其意为启动一个1600x1200的窗口，标题为"抽号机"，渲染"index.html"
### 渲染的HTML
名字在``main.js``里写了叫``index.html``
```html
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
		<link rel="stylesheet" type="text/css" href="css/bootstrap.css" />
	</head>
<body>
	
	<div class="container-fluid"></div>
	<div class="container-fluid">
		<h1><strong>这是什么？抽号机喵~</strong></h1>
		<hr style="border-top:1px dotted #ccc;"/>
		<div class="container-fluid">
			<h1>1/54，保留6号</h1>
			<button type="button" class="btn btn-info btn-lg" onclick="rollNumber();">抽！</button>
			<br>
		</div>
		<div class="container-fluid">
			<h2>结果：</h2>
			<div id="result"></div>
		</div>
	</div>
<script src="script.js"></script>	
</body>	
</html>
```
班级有54个人，6号休学去写曲子了，把他留着。
引入了``BootStrap CSS``，去下载：
{% btn https://getbootstrap.com/docs/3.4/getting-started/, BootStrap CSS %}
下载之后把三样东西都放到``random``文件夹下。
~~按钮变得好看起来~~

引入的外部js代码在``script.js``里。
### 逻辑部分
需要用代码实现预定范围的随机数生成，并将其返回显示。
``index.html``里写了代码在``script.js``里。
```JavaScript
function rollNumber(){
	var set = [];
	while(set.length < 1){
		var r = Math.floor(Math.random() * 53) + 1;
		if(set.indexOf(r) === -1) set.push(r);
	}
 
	displayResult(set);
}

function displayResult(set){
	set.join(", ");
	
	document.getElementById('result').innerHTML="<center><h1 class='text-primary'>"+set+"</h1></center>";
}
```
从1-54号中抽一个并返回。

## 跑
```powershell
npm run start
```
启动一个Electron窗口，就是这个。呐，跑起来了。

## 结构

这是我放在 Github 上的代码的目录结构。
建议在获取之后运行下面代码更新 npm 包

```npx npm-check-updates -u```

* .gitignore -> 忽略一些构建生成和node modules，要不这几百M放Github上每次还得更新

* forge.config.js -> 打包工具的依赖

* index.html -> 网页

* main.js -> 启动脚本

* package(-lock).json -> npm的购物清单

* script.js -> JavaScript代码

* namelist.json -> new feature的名单，已打码（new feature暂时还未实现、、

* css & fonts & js -> Bootstrap CSS
