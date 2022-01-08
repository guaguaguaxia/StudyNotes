# curl.1 the man page
## 名称
curl - transfer a URL
## 简介
curl [options / URLs]
## 描述
curl是一个从服务器传输数据或向服务器传输数据的工具。它支持这些协议：DICT, FILE, FTP, FTPS, GOPHER, GOPHERS, HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, MQTT, POP3, POP3S, RTMP, RTMPS, RTSP, SCP, SFTP, SMB, SMBS, SMTP, SMTPS, TELNET或TFTP。该命令被设计为无需用户干预即可工作。

curl提供了大量有用的技巧，如代理支持、用户认证、FTP上传、HTTP发布、SSL连接、cookies、文件传输恢复等。正如你将在下面看到的，这些功能的数量会让你头晕目眩。

curl由libcurl提供所有传输相关的功能。详情见libcurl(3)。

## Url

URL的语法与协议有关。你可以在[RFC 3986]("https://www.ietf.org/rfc/rfc3986.txt")中找到详细的描述。

你可以通过在大括号内写出部分集，并在URL中加引号来指定多个URL或URL的部分，如：
```
  "http://site.{one,two,three}.com"
```

或者你可以通过使用[]来获得字母数字系列的序列，如：
```
  "ftp://ftp.example.com/file[1-100].txt"
  "ftp://ftp.example.com/file[001-100].txt" (前面带有0的)
  "ftp://ftp.example.com/file[a-z].txt"
```

不支持嵌套的序列，但你可以使用几个彼此相邻的序列：
```
  "http://example.com/archive[1996-1999]/vol[1-4]/part{a,b,c}.html"
```

你可以在命令行中指定任何数量的URL。它们将按照指定的顺序依次被取出。你可以在命令行上以任何顺序混合指定命令行选项和URL。

你可以为范围指定一个步骤计数器，以获得每N个数字或字母：
```
  "http://example.com/file[1-100:10].txt"
  "http://example.com/file[a-z:2].txt"
```

当使用[]或{}序列从命令行提示符中调用时，你可能必须将完整的URL放在双引号内，以避免Shell对其进行干扰。这也适用于其他被视为特殊的字符，例如"&"、"？"和 "*"。

在URL中提供IPv6区域索引，并加上转义的百分比符号和接口名称。如:
```
  "http://[fe80::3%25eth0]/"
```

如果你指定的URL没有 **协议://前缀**，curl会尝试猜测你可能想要的协议。它将默认为HTTP，但会根据经常使用的主机名前缀尝试其他协议。例如，对于以 "ftp "开头的主机名，curl会假设你想使用FTP。

curl会尽力使用你传递给它的URL。它并不试图把它作为一个语法正确的URL来验证，但它接受的内容是相当自由的。

curl将尝试为多个文件传输重新使用连接，因此从同一服务器获取许多文件时不会进行多次连接/握手。这可以提高速度。当然，这只适用于在一个命令行中指定的文件，不能在分开的curl调用中使用。

## 输出

如果没有被告知，curl会把收到的数据写到stdout。可以用-o, --output或-O, --remote-name选项来指示它将数据保存到本地文件中。如果curl在命令行中给出了多个要传输的URL，它同样需要多个选项来决定将它们保存在哪里。

curl不会解析或以其他方式"理解"它得到的或写成输出的内容。它不做任何编码或解码，除非用专门的命令行选项明确要求。

## 协议

curl支持许多协议，或者用URL术语来说：方案。你的特定构建可能不支持所有这些协议。

**DICT**
让你使用在线字典查询单词。

**FILE**
curl不支持远程访问file:// URL，但当在Microsoft Windows上运行时，使用本地UNC方法就可以了。

**FTP(S)**
curl supports the File Transfer Protocol with a lot of tweaks and levers. With or without using TLS.

**GOPHER(S)**
检索文件。

**HTTP(S)**
curl支持HTTP，有许多选项和变化。它可以表述HTTP的0.9、1.0、1.1、2和3版本，取决于构建选项和正确的命令行选项。

**IMAP(S)**
使用邮件阅读协议，curl可以为你 "下载 "邮件。无论是否使用TLS。

**LDAP(S)**
curl可以为你做目录查询，无论是否有TLS。

**MQTT**
curl支持MQTT版本3。通过MQTT下载等于 "订阅 "一个主题，而上传/发布等于在一个主题上 "发布"。不支持通过TLS的MQTT（目前）。

**POP3(S)**
从pop3服务器下载意味着得到一个邮件。无论是否使用TLS。

**RTMP(S)**
实时信息传输协议主要用于服务器流媒体，curl可以下载它。

**RTSP**
curl支持RTSP 1.0下载。

**SCP**
curl支持SSH第二版scp传输。

**SFTP**
curl支持通过SSH第2版完成的SFTP（草案5）。

**SMB(S)**
curl支持SMB版本1的上传和下载。

**SMTP(S)**
将内容上传到SMTP服务器意味着发送电子邮件。无论是否使用TLS。

**TELNET**
告诉curl获取一个telnet URL，开始一个交互式会话，它在stdin上发送它所读取的内容，并输出服务器发送给它的内容。

**TFTP**
curl可以进行TFTP下载和上传。

## 进度表
curl在操作过程中通常会显示一个进度表，显示传输的数据量、传输速度和估计剩余时间等。进度表显示的是字节数，速度的单位是字节/秒。后缀（k、M、G、T、P）是基于1024的。例如，1k是1024字节。1M是1048576字节。

curl默认将这些数据显示在终端，所以如果你调用curl做一个操作，并且它即将将数据写到终端，它就会禁用进度表，否则就会把进度表和响应数据的输出搞乱。

如果你想要一个HTTP POST或PUT请求的进度表，你需要使用shell重定向（>）、-o、--output或类似的方法，将响应输出重定向到一个文件。

这不适用于FTP上传，因为该操作不会向终端输出任何响应数据。

如果你喜欢一个进度 "条 "而不是普通的仪表，-#, --progress-bar是你的好选择。你还可以用-s, --silent选项完全禁用进度表。

## 选项

选项以一个或两个破折号开始。许多选项需要在其旁边加上一个附加值。

短的 "单破折号"形式的选项，例如-d，可以在它和它的值之间使用或不使用空格，但建议使用空格作为分隔符。长的 "双破折号 "形式，例如-d，--data，需要在它和它的值之间有一个空格。

不需要任何附加值的短版选项可以紧挨着使用，比如你可以一次性指定所有选项-O、-L和-v为-OLv。

一般来说，所有的布尔选项都可以用--option来启用，也可以用--no-option来禁用。也就是说，你使用相同的选项名称，但前缀为 "no-"。然而，在这个列表中，我们大多只列出和显示它们的--option版本。


**--abstract-unix-socket <path>**

(HTTP）通过一个抽象的Unix域套接字连接，而不是使用网络。注意：netstat显示抽象套接字的路径以'@'为前缀，然而<path>参数不应该有这个前导字符。

Example:

```
 curl --abstract-unix-socket socketpath https://example.com
```

See also --unix-socket. Added in 7.53.0.


**--alt-svc <file name\>**

[HTTP Alternative Services 介绍]("https://imququ.com/post/http-alt-svc.html")

(HTTPS) 该选项在curl中启用alt-svc解析器。如果文件名指向一个现有的alt-svc缓存文件，将被使用。在完成传输后，如果缓存被修改过，将再次保存到该文件名。

指定一个""文件名（零长度），以避免加载/保存，使curl只是处理内存中的缓存。

如果这个选项被多次使用，curl将从所有的文件中加载内容，但最后一个文件将被用于保存。

Example:
```
 curl --alt-svc svc.txt https://example.com
```

See also --resolve and --connect-to. Added in 7.64.1.  

**--anyauth**


(HTTP) 告诉curl自己找出认证方法，并使用远程站点声称支持的最安全的方法。这是通过首先做一个请求并检查响应头文件来完成的，因此可能会引起额外的网络往返。这是用来代替设置一个特定的认证方法的，你可以用--basic, --digest, --ntlm, 和 --negotiate.来做。

如果你从stdin上传，不建议使用--anyauth，因为它可能要求数据被发送两次，然后客户端必须能够回退。如果从stdin上传时出现这种需要，上传操作会失败。

与-u, --user一起使用。

Example:

```
 curl --anyauth --user me:pwd https://example.com
```

另见 --proxy-anyauth, --basic 和 --digest。

**-a, --append**

(FTP SFTP)当在上传中使用时，这将使curl追加到目标文件中，而不是覆盖它。如果远程文件不存在，它将被创建。注意，这个标志会被一些SFTP服务器（包括OpenSSH）所忽略。

Example:
```
 curl --upload-file local --append ftp://example.com/
```

另见-r, -range和-C, --continue-at。

**--aws-sigv4 <provider1[:provider2[:region[:service]]]>**

在传输中使用AWS V4签名认证。

提供者参数是一个字符串，在创建外发认证头时被算法使用。

区域参数是一个字符串，当区域名称在端点中被省略时，它指向资源集合的一个地理区域（区域-代码）。

服务参数是一个字符串，当服务名称在端点中被省略时，它指向一个由云提供的函数（服务代码）。

Example:
```
 curl --aws-sigv4 "aws:amz:east-2:es" --user "key:secret" https://example.com
```

参见 --basic 和 -u, --user。