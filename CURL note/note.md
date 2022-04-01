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

**--basic**

(HTTP) 告诉curl对远程主机使用HTTP Basic认证。这是默认的，这个选项通常是没有意义的，除非你用它来覆盖先前设置的选项，该选项设置了不同的认证方法（如 --ntlm, --digest, 或 --negotiate）。

与-u, --user一起使用。

Example:
```
 curl -u name:password --basic https://example.com
```
 另见 --proxy-basic.

 **--cacert <file>**

 (TLS) 告诉curl使用指定的证书文件来验证对方。该文件可以包含多个CA证书。证书必须是PEM格式。通常情况下，curl会使用一个默认的文件，所以这个选项通常用来改变这个默认文件。

 如果环境变量'CURL_CA_BUNDLE'被设置，curl会识别它，并使用给定的路径作为CA证书包的路径。这个选项覆盖了这个变量。

 windows版本的curl会自动寻找一个名为'curl-ca-bundle.crt'的CA证书文件，可以在curl.exe的同一目录下，也可以在当前工作目录下，或者在你的PATH中的任何文件夹下。

 如果curl是针对NSS SSL库构建的，那么NSS PEM PKCS#11模块（libnsspem.so）需要可用，以便该选项能够正常工作。

 (仅限iOS和macOS）如果curl是针对安全传输构建的，那么为了向后兼容其他SSL引擎，这个选项是支持的，但不应该被设置。如果不设置该选项，那么curl将使用系统和用户钥匙串中的证书来验证对等体，这是验证对等体证书链的首选方法。

(仅限Schannel）该选项在Windows 7或更高版本的libcurl 7.60或更高版本的Schannel中被支持。支持该选项是为了向后兼容其他SSL引擎；相反，建议使用Windows的根证书存储（Schannel的默认）。

如果这个选项被多次使用，将使用最后一个选项。

Example:
```
 curl --cacert CA-file.txt https://example.com
```

参见 --capath 和 -k, --insecure。

**--capath <dir\>**

(TLS) 告诉curl使用指定的证书目录来验证对方。可以提供多个路径，用": "隔开（例如 "path1:path2:path3"）。证书必须是PEM格式，如果curl是针对OpenSSL构建的，该目录必须已经用OpenSSL提供的c_rehash工具处理过。如果--cacert文件包含许多CA证书，使用--capath可以使OpenSSL驱动的curl比使用--cacert更有效地进行SSL连接。

如果设置了这个选项，默认的capath值将被忽略，如果它被多次使用，将使用最后一个。

Example:
```
 curl --capath /local/directory https://example.com
```

参见 --cacert 和 -k, --insecure。

**--cert-status**

(TLS) 告诉curl通过使用证书状态请求（又称OCSP装订）TLS扩展来验证服务器证书的状态。

如果该选项被启用，并且服务器发送了一个无效的（如过期）响应，如果该响应表明服务器证书已被撤销，或者根本没有收到任何响应，则验证失败。

这一点目前只在OpenSSL、GnuTLS和NSS后端实现。

Example:
```
 curl --cert-status https://example.com
```
另请参见 --pinnedpubkey。

**--cert-type <type\>**

(TLS) 告诉 curl 所提供的客户证书使用的是什么类型。PEM, DER, ENG 和 P12 是公认的类型。如果没有指定，则假定为PEM

如果这个选项被多次使用，将使用最后一个选项。

Example:
```
 curl --cert-type PEM --cert file https://example.com
```

参见-E, --cert, --key 和 --key-type。

**-E, --cert <certificate[:password]>**

(TLS) 告诉curl在用HTTPS、FTPS或其他基于SSL的协议获取文件时使用指定的客户证书文件。如果使用安全传输，证书必须是PKCS#12格式，如果使用任何其他引擎，必须是PEM格式。如果没有指定可选的密码，它将在终端上被查询到。请注意，这个选项假定有一个 "证书 "文件，它是私钥和客户证书的串联!参见 -E, --cert 和 --key 来单独指定它们。

如果curl是针对NSS SSL库构建的，那么这个选项可以告诉curl在环境变量SSL_DIR（或默认为/etc/pki/nssdb）定义的NSS数据库中使用证书的昵称。如果NSS的PEM PKCS#11模块（libnsspem.so）可用，那么可以加载PEM文件。如果你想使用当前目录下的文件，请在其前面加上"./"前缀，以避免与昵称混淆。如果昵称包含":"，需要在它前面加上"\"，这样它就不会被识别为密码分隔符。如果昵称包含"\"，它需要被转义为"\\"，这样它就不会被识别为一个转义字符。

如果curl是根据OpenSSL库构建的，并且pkcs11引擎是可用的，那么PKCS#11 URI（RFC 7512）就可以用来指定位于PKCS#11设备中的证书。一个以 "pkcs11: "开头的字符串将被解释为一个PKCS#11 URI。如果提供了一个PKCS#11 URI，那么--engine选项将被设置为 "pkcs11"，如果没有提供，--cert类型选项将被设置为 "ENG"。

(仅限iOS和macOS）如果curl是针对安全传输构建的，那么证书字符串可以是系统或用户钥匙串中的证书/私钥的名称，或者是PKCS#12编码的证书和私钥的路径。如果你想使用当前目录下的文件，请在其前面加上"./"前缀，以避免与昵称混淆。

(仅适用于Schannel)客户证书必须通过路径表达式指定到一个证书商店。(不支持加载PFX，你可以先把它导入到一个商店)。你可以使用"<store location>/<store name>/<thumbprint>"来引用系统证书库中的证书，例如，"CurrentUser\MY\934a7ac6f8a5d579285a74fa61e19f23ddfe8d7a"。缩略图通常是一个SHA-1十六进制字符串，你可以在证书细节中看到。支持以下存储位置。CurrentUser, LocalMachine, CurrentService, Services, CurrentUserGroupPolicy, LocalMachineGroupPolicy, LocalMachineEnterprise。

如果这个选项被多次使用，将使用最后一个选项。

Example:
```
 curl --cert certfile --key keyfile https://example.com
```
另请参见 --cert-type, --key 和 --key-type。

**--ciphers <list of ciphers\>**

(TLS) 指定在连接中使用哪些密码。密码列表必须指定有效的密码。在这个网址上阅读关于SSL密码列表的细节。
``` 
https://curl.se/docs/ssl-ciphers.html
```
如果这个选项被多次使用，将使用最后一个选项。

Example:
```
 curl --ciphers ECDHE-ECDSA-AES256-CCM8 https://example.com
```
参见-tlsv1.3。


as
acx
sas
asas

sasc

vcdcfd

sas

savcx
dgfg

gfdfs


dfgf
cdsa

sas
sa
as
fvdfas
asdsadas

sfvsddsasa

dhgfsassa
dsa
afad
sdf
as
fdaf
fasf
fsd
awd
dasddas