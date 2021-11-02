# vscode中go开发环境配置

## 安装插件

首先`go`插件必须要安装上，安装完成后打开go文件，会提示安装`gopls`，但是此时不一定可以安装，因为是国内环境，懂得都懂。

## 安装gopls和go-outline

正确的安装方式是，首先配置go的代理：
```
go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```

然后就可以简单的使用`go install`进行命令进行安装啦：
```
go install golang.org/x/tools/gopls@latest
go install github.com/ramya-rao-a/go-outline@latest
```

最后，重新打开vscode,是不是发现智能提示已经有了。

## 配置调试工具

在打开的go文件中按`F5`调试，会提示安装dlv-dap工具，点击`install`进行安装，安装完成后，再次按`F5`会有一行错误提示：
```
go.mod file not found in current directory or any parent directory; see 'go help modules'
```

此时，在终端中输入：
```
go env -w GO111MODULE=auto
```

然后再次按`F5`启动调试，发现程序可以正常运行了！