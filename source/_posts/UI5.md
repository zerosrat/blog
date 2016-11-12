title: UI5 知识分享
date: 2016-04-11 22:56:56
tags: ui5
categories: Front End
---

## 前言

### 参考

> 官方指南 - [Demo Kit](https://sapui5.hana.ondemand.com/#docs/guide/99ac68a5b1c3416ab5c84c99fefa250d.html)

### What is UI5?

UI5是SAP的一个企业网页前端框架，基于jQuery，有很多可以直接使用的控件。UI5又分为 [OpenUI5](http://openui5.org/) 和 SAPUI5，其中前者是开源的，可以在 [github](https://github.com/SAP/openui5) 获取。
<!-- more -->
### Why UI5?

今年1月到SAP实习，到现在主要是做前端开发，对UI5也有了一定的了解，可以为大家带来一些分享。做企业级网站的话，UI5或许是个不错的选择。

***

## 开发环境搭建

### Eclipse + SAPUI5 Tools
<img src="http://7xoxnz.com1.z0.glb.clouddn.com/ui5_eclipse.png" width="100" alt="图片名称"/>

搭建步骤直接从官方文档拖过来：

> Required Software for Installing and Running SAPUI5 Tools
> To install and run SAPUI5 tools, the following software has to be installed:
> 
> - Java Runtime environment: JRE version, as supported by the used Eclipse release, in 32- or 64-Bit. For example, for Eclipse Mars, JRE 1.7 is recommended but > JRE 1.8 is supported as well.> 
> 
> - Operating system: Windows XP, Vista, 7 (32- or 64-Bit), 8/8.1
> 
> - Browser: Not relevant, except for Internet Explorer 9.0 or higher for embedded application preview
> 
> Installation Process for SAPUI5 Tools
> To install SAPUI5 tools, proceed as follows:
> 
> 1. Launch your Eclipse workbench. Open the installation wizard by choosing **Help>Install New Software**.
> 
> 2. In the Work with field of the installation wizard, specify the target directory of the package.
> 
> 3. To add the new installation directory, choose Add and then choose Archive to specify the location. Enter a name for your local software site.
> 
> 4. Select all features for the UI development toolkit for HTML5 and choose Next.
> 
> 5. Review the feature groups to be installed and choose Next.
> 
> 6. Accept the terms of the license agreement and choose Finish to initiate the installation of selected feature groups.
> 
> 7. In the Certificates dialog confirm the certificates from Eclipse.org and SAP with OK.
> 
> 8. To apply the changes of the installation procedure, restart the Eclipse workbench.
> 
> 9. To check, whether the installation has been successful, proceed as follows:
> 
> For application development open Eclipse and choose ** File>New>Other...>SAPUI5 Application Development>Application Project**. If the installation has been successful, the New Application Project wizard opens.

### Intellij IDEA + OpenUI5

<img src="http://7xoxnz.com1.z0.glb.clouddn.com/ui5_idea.gif" width="100" alt="图片名称"/>

> [参考国外友人的分享](http://scn.sap.com/community/developer-center/front-end/blog/2014/09/22/configuring-jetbrains-webstorm-for-ui5-development)

### SAP Web IDE

<img src="http://7xoxnz.com1.z0.glb.clouddn.com/ui5_webide.png" width="100" height="100" alt="图片名称"/>

最后一种方式最简单，只需要一个HCP账号就行了：

1. [登录HCP](https://account.hanatrial.ondemand.com/) (没有账号的话可以先注册)

2. 点击侧边栏的 **Subscriptions**

3. 点击 **webide**

4. 点击 **Application URL**

***

## Hello World

```html
<!DOCTYPE html>
<html>
<head>
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta charset="utf-8">
	<title>Hello World App</title>
	<script src="https://openui5.hana.ondemand.com/resources/sap-ui-core.js"
		id="sap-ui-bootstrap"
		data-sap-ui-theme="sap_bluecrystal"
		data-sap-ui-libs="sap.m">
	</script>
	<script type="text/javascript">
		sap.ui.getCore().attachInit(function () {
			// create a mobile app and display page initially
			var app = new sap.m.App("myApp", {
				initialPage: "page"
			});
			// create the page
			var page = new sap.m.Page("page", {
				title : "Hello World",
				showNavButton : false,
				content : new sap.m.Text({
					text : "Hello World"
				})
			});
			
			app.addPage(page);
			// place the app into the HTML document
			app.placeAt("content");
		});
	</script>
</head>
<body class="sapUiBody" id="content">
</body>
</html>
```

***

## Data Binding

### What is data binding

UI5使用了一种方式，将数据源和实体绑定，数据改变是相互影响的。数据源就是Model，而实体一般就是control或是view。

### 简单例子

sample.controller.js
```javascript
onInit : function(){
	var data = {helloText : "hello"};
	var model = new sap.ui.model.json.JSONModel(data);
	this.getView().getModel(model, "defaultModel");
}
```

sample.view.xml
```
<Text text="{defaultModel>/helloText}" />
```

## Navigation and Routing

### What is navigation and routing

基于uri的mapping来进入不同页面的一种机制，徒说无益，看看下面的例子。

### 简单例子

webapp/manifest.json
```
{
   ...
   "sap.ui5": {
      ...
      "routing": {
         "config": {
            "routerClass": "sap.m.routing.Router",
            "viewType": "XML",
            "viewPath": "sap.ui.demo.nav.view",
            "controlId": "app",
            "controlAggregation": "pages",
            "transition": "slide",
            "bypassed": {
               "target": "notFound"
            }
         },
         "routes": [{
            "pattern": "",
            "name": "appHome",
            "target": "home"
         }],
         "targets": {
            "home": {
               "viewName": "Home",
               "viewLevel" : 1
            },
            "notFound": {
               "viewName": "NotFound",
               "transition": "show"
            }
         }
      }
   }
}
```

webapp/Component.js
```
init: function () {
    // call the init function of the parent
    UIComponent.prototype.init.apply(this, arguments);

    // create the views based on the url/hash
    this.getRouter().initialize();
}
```