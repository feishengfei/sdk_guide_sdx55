# 简介

本文档使用[gitbook](https://docs.gitbook.com/)维护。

### 安装

```
sudo apt-get install nodejs npm calibre
npm config set strict-ssl false
sudo npm install -g gitbook-cli
```

### 运行web服务器

```
zhouX@twserver-ThinkCentre-M920t-N000:~/sdk_guide$ ls
_book  en  LANGS.md  sdk_guide.md  zh-hans
zhouX@twserver-ThinkCentre-M920t-N000:~/sdk_guide$ gitbook serve
Live reload server started on port: 35729
Press CTRL+C to quit ...

info: parsing multilingual book, with 2 languages 
info: 7 plugins are installed 
info: loading plugin "livereload"... OK 
info: loading plugin "highlight"... OK 
info: loading plugin "search"... OK 
info: loading plugin "lunr"... OK 
info: loading plugin "sharing"... OK 
info: loading plugin "fontsettings"... OK 
info: loading plugin "theme-default"... OK 
info: found 2 asset files 
info:  
info: generating language "zh-hans" 
info: found 7 pages 
info: found 0 asset files 
info:  
info: generating language "en" 
info: found 1 pages 
info: found 0 asset files 
info: >> generation finished with success in 0.5s ! 

Starting server ...
Serving book on http://localhost:4000
```

之后可以在浏览器中输入http://localhost:4000进行浏览。

### 生成pdf

```
frankzhou@jupiter:~/sdk_guide$ ls
en  LANGS.md  zh-hans
frankzhou@jupiter:~/sdk_guide$ gitbook pdf
info: parsing multilingual book, with 2 languages 
info: 7 plugins are installed 
info: 6 explicitly listed 
info: loading plugin "highlight"... OK 
info: loading plugin "search"... OK 
info: loading plugin "lunr"... OK 
info: loading plugin "sharing"... OK 
info: loading plugin "fontsettings"... OK 
info: loading plugin "theme-default"... OK 
info: found 1 asset files 
info:  
info: generating language "zh-hans" 
info: found 7 pages 
info: found 0 asset files 
info:  
info: generating language "en" 
info: found 1 pages 
info: found 0 asset files 
info: >> generation finished with success in 7.5s ! 
info: >> 2 file(s) generated 
frankzhou@jupiter:~/sdk_guide$ ls
book_en.pdf  book_zh-hans.pdf  en  LANGS.md  zh-hans
frankzhou@jupiter:~/sdk_guide$ 
```
