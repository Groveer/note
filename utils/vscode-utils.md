# VSCodium和Code-OSS使用vscode扩展

1. 找到product.json文件：
```
debian: /usr/share/codium/resources/app/product.json
archlinux: /usr/lib/code/product.json
```
2. 修改"extensionsGallery"字段：
```json
"extensionsGallery":{
    "serviceUrl":"https://marketplace.visualstudio.com/_apis/public/gallery",
    "cacheUrl":"https://vscode.blob.core.windows.net/gallery/index",
    "itemUrl": "https://marketplace.visualstudio.com/items"
}
```
3. 重启code即可

# vscode使用gitee同步配置

1. 在网页上登陆gitee后，点击右上角“发布代码片段”
2. 随便填写信息，如“vscode clound sync”，点击发布
3. 发布后跳转的浏览器地址最后一段就是gistID,若遗忘，则在个人主页后添加`/codes`就可以进入代码片段管理页面了
4. 点击头像，进入设置页面，找到私人令牌，创建私人令牌
5. 安全起见，建议只勾选`user_info`和`gists`权限
6. 提交后会弹出消息框，务必记录该私人令牌，因为它只会出现一次，如我的就是：
```
ae82d00e89e159112b1bacce8dd3c8a8
```
7. 打开vscode，按下`Ctrl+Shift+P`，输入`settings.json`打开设置，并填入以下内容：
```
"gitee.gist":"将双引号里面的内容替换成前面获取到的gistID",
"gitee.access_token":"将双引号里面的内容替换成你的私人令牌"
```
8. 配置好后，可以使用`upload setting`命令上传设置，`download setting`命令下载配置

