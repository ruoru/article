# Go

## Go install

go package download: https://golang.org/dl/

| 操作系统 | 包名                           |
| -------- | ------------------------------ |
| Windows  | go1.4.windows-amd64.msi        |
| Linux    | go1.4.linux-amd64.tar.gz       |
| Mac      | go1.4.darwin-amd64-osx10.8.pkg |
| FreeBSD  | go1.4.freebsd-amd64.tar.gz     |

- UNIX/Linux, 和 FreeBSD 安装
  以下介绍了在 UNIX/Linux/Mac OS X, 和 FreeBSD 系统下使用源码安装方法：

1. 下载源码包：go1.4.linux-amd64.tar.gz。
2. 将下载的源码包解压至 /usr/local 目录。

```shell
tar -C /usr/local -xzf go1.4.linux-amd64.tar.gz
```

3. 将 /usr/local/go/bin 目录添加至 PATH 环境变量：
   export PATH=\$PATH:/usr/local/go/bin

- Mac OS X

1. 下载源码包：
2. MAC 系统下你可以使用 .pkg 结尾的安装包直接双击来完成安装，安装目录在 /usr/local/go/ 下。
3. 创建 Path 文件

```shell
vi ~/.bashrc

//插入
export GOPATH=$HOME/gopath
export PATH=$PATH:$HOME/go/bin:$GOPATH/bin

bash .bashrc  //重启生效
```

## go 本地查看文档（以浏览器方式）

```
godoc -http=:[port]
http:localhost:[port]
```
