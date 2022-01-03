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
