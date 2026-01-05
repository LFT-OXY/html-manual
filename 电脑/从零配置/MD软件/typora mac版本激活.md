首先去官网选择mac版本下载安装 typora下载

然后打开typora包内容找到

/Applications/Typora.app/Contents/Resources/TypeMark/ 

编辑器打开上面文件夹，这里我拉到vscode

找到page-dist/static/js/Licen..如下图 

![image.png](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250410233319248.png)




输入 `hasActivated="true"==e.hasActivated`进行搜索

将它改为  `hasActivated="true"=="true"`

重新打开typora看到成功激活！！！

四，关闭软件每次启动时的已激活弹窗
继续在安装路径下resources\page-dist\license.html，找到



依旧ctrl+F 定位到：

</body></html>

替换为 

</body><script>window.οnlοad=function(){setTimeout(()=>{window.close();},5);}</script></html>

五，去除软件左下角“未激活”提示
按照安装路径，找到 resources\locales\zh-Hans.lproj\Panel.json 



文件中查找："UNREGISTERED":"未激活"，

替换为："UNREGISTERED":" "

最后，重新打开Typora，手动关掉激活窗口，之后就不会再出现。
