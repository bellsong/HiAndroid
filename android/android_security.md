[安全](http://blog.csdn.net/androidsecurity/article/details/8833710)


MD5 CRC32 SHA1

对应到文件，假设 SHA-1 是没有漏洞的算法的话，那就不存在两个不一样的文件会有一样的 SHA-1 值，所以只要 SHA-1 值一样，那么基本上文件也是一样的~

在实际生活中，软件可以算出一个 SHA-1 的数值，如果他被人恶意篡改， SHA-1 值就会被改变。

于是用户只要对比下 SHA-1 值是不是和官方提供的一样就能确保文件的正确性~

这种做法被广泛的利用与互联网的各个地方用以确保安全

，比如网站的证书签名、文件校对、开源软件的代码仓库管理~

move to SHA-256

Consider using safer alternatives, such as SHA-256, or SHA-3.