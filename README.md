# mdSDS  
markdown Static Document System | markdown 静态文档系统  

[Demo](https://doc.clz.me/)  

## 使用方法  
0. 依赖安装（如已安装请跳过）  
[下载安装node LTS版本](https://nodejs.org/)  
  
1. 下载项目  
```shell
$ git clone https://github.com/coloz/mdSDS.git
```
  
2. 将markdown文档放置在docs目录中  
推荐以 <数字标号>-<文件名>的方式命名文档和文件夹  
这样创建出的目录，将以<数字标号>进行排序    
  
3. 运行目录创建脚本  
```shell
$ cd mdSDS
$ node process.js
```
  
4. 将项目部署到服务器或pages服务  


## 目录创建  
mdSDS支持三种文档访问方式：  

### 常规方式  
运行目录创建脚本  
```shell
$ node process.js
```

该方式创建出的URL：  
首页.md  >>>  https://doc.clz.me/shou-ye  
001-文件夹/001-文档.md  >>>  https://doc.clz.me/wen-dang  

### markdown方式  
运行目录创建脚本  
```shell
$ node process.js --md
```

该方式创建出的URL：  
000-首页.md  >>>  https://doc.clz.me/首页.md  
001-文件夹/001-文档.md  >>>  https://doc.clz.me/001-文件夹/001-文档.md  

### 自定义URL  
1. 在项目根目录下添加**custom.json**文件，添加 **文件名：自定义URL**  
**custom.json**
```json
{
    "首页": "home",
    "文档": "document",
}
```
2. 运行目录创建脚本  
```shell
$ node process.js
```

该方式创建出的URL：  
000-首页.md  >>>  https://doc.clz.me/home  
001-文件夹/001-文档.md  >>>  https://doc.clz.me/document  

##  静态服务器配置  
### Apache
在 .htaccess 文件中添加一个重写规则， 代码如下：
```
RewriteEngine On
    # If an existing asset or directory is requested go to it as it is
    RewriteCond %{DOCUMENT_ROOT}%{REQUEST_URI} -f [OR]
    RewriteCond %{DOCUMENT_ROOT}%{REQUEST_URI} -d
    RewriteRule ^ - [L]
    # If the requested resource doesn't exist, use index.html
RewriteRule ^ /index.html
```
### NGinx
使用 try_files 指向 index.html
```
try_files $uri $uri/ /index.html;
```
### IIS
往 web.config 中添加一条重写规则，类似于这里：
```
<system.webServer>
  <rewrite>
    <rules>
      <rule name="Angular Routes" stopProcessing="true">
        <match url=".*" />
        <conditions logicalGrouping="MatchAll">
          <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
          <add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" />
        </conditions>
        <action type="Rewrite" url="/index.html" />
      </rule>
    </rules>
  </rewrite>
</system.webServer>
```  
### Github Pages服务  
直接部署即可  

## 灵感来自amwiki
对比[amwiki](https://github.com/TevinLi/amWiki)，主要做了两点优化：  
1. 自动为markdown文档生成右侧目录，方便文档查阅；  
2. 自动将md中文文件名转换为拼音短链接，方便分享和记录链接，同时也提供了自定义短链接和类似amwiki的XXX.md文件名访问方式；  

只需复制粘贴一下，就可以无缝的迁移amwiki文档到这个文档系统。  
再次感谢amwiki，过去一年多时间，我们公司文档都是使用的amwiki构建，非常方便，因为作者太久不维护了，所以逼着我自己开发了一个。  
