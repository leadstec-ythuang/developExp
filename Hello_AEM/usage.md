[TOC]
# AEM使用

## 
### 在网页AEM设置默认配置导入本地代码
1. 网页page配置默认数据
2. 在crx/de中找到页面节点数据（ex：content/oasis/us/page)复制粘贴到项目component下的cq:template下
3. 使用AEM Sync插件从AEM导入到本地项目代码component下的cq:template
4. 在crx/de中
  + create packages
  + edit -> filters,找到dam里的assets静态资源 -> done
  + build完package包下载到本地解压
  + 找到相应的文件夹复制到本地项目相应文件夹

  ## HTL文档
  ### HTL全局对象
  #### 可枚举对象
  + properties 当前资源的属性列表
  + pageProperties 当前页面的页面属性列表
  + inheritedPageProperties 当前页面的继承页面属性列表
  
  ### HTL JavaScript Use-API
  JavaScript Use-API允许HTL文件访问使用JavaScript编写的帮助程序代码