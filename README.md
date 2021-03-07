> pingoswoole

<br>

<p align="center" >
  PingoSwoole
</p>

<p align="center">高性能 • 轻量级 • 命令行</p>

 

## Pingoswoole PHP 是什么

基于 Swoole 开发的高性能 PHP 框架，从 `2017` 年开始经过多年发展收获了很多中小型团队的支持，框架版本也经历了多个版本的迭代：

- `V1.*`: 基于 Swoole 的常驻内存型 PHP 高性能框架
- `V2.0`: 基于 Swoole 的 FastCGI、常驻内存、协程三模 PHP 高性能框架
- `V2.1`: 基于 Swoole 4.4+ 单线程协程 PHP 框架 
- `V2.2`: 基于 Swoole 4.4+ 单线程协程 PHP 微服务框架 

## 与传统 MVC 框架比较

传统框架大部分都是在 HTTP 领域开发，而 Mix 能开发 HTTP、WebSocket、TCP、UDP、RPC 几乎全部互联网领域。

在命令开发方面 Mix 也有更多的封装，填充代码即可开发一个功能完备的命令行程序。

在性能方面采用常驻内存+协程，拥有传统框架无法比拟的性能。 

## 微服务
 

## 框架定位

在其他 Swoole 框架都定位于大中型企业的时候，Mix 决定推动这项技术的普及，我们定位于众多的中小型企业、创业型公司，我们将 Swoole 的复杂度封装起来，用简单的编码方式呈现给用户，让更多的中级程序员也可打造高并发系统，让 Swoole 不再只是高级程序员的专利。
 
 
## 环境要求

* Linux
* PHP >= 7.3
* Swoole >= 4.4.4 (websocket >= 4.4.15)

## 快速开始

推荐使用 [composer](https://www.phpcomposer.com/) 安装。

```
composer create-project --prefer-dist pingo/swooleapp app ~2.0
```

启动服务器：

接下来启动 HTTP 服务器。

```
$> php bin/pingswoole server:start -d
```

如果一切顺利，运行到最后你将看到如下的输出：

```
            (_)                                   | |     
    _ __  _ _ __   __ _ _____      _____   ___ | | ___ 
    | '_ \| | '_ \ / _` / __\ \ /\ / / _ \ / _ \| |/ _ \
    | |_) | | | | | (_| \__ \ V  V / (_) | (_) | |  __/
    | .__/|_|_| |_|\__, |___/ \_/\_/ \___/ \___/|_|\___|
    | |             __/ |                               
    |_|            |___/        
                  
    -------------------------------------------------------
listen ip                        0.0.0.0   
listen port                      9501   
swoole version                   4.6.2   
php version                      7.3.26   
pingoswoole version              2.0.1   
tmp dir                          /www/swooleapp/runtime/tmp/   
log dir                          /www/swooleapp/runtime/log/ 
```

访问测试 (新开一个终端)：

```
$> curl http://127.0.0.1:9501/
Hello, World!
```

## License

Apache License Version 2.0, http://www.apache.org/licenses/
