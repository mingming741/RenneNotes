# cgi
Common gateway Interface(cgi)，是为了使得web服务可以执行console application的一种操作，不仅限于apache的server，而是可以贯通到所有的能提供web server的服务结构中去。 通常情况下, 一次请求对应一个CGI 脚本的执行，生成一个HTML。

简而言之，一个 HTTP POST 请求，从客户端经由 标准输入 发送数据到一个CGI 程序 。同时携带其他数据, 例如 URL paths, HTTP header 数据, 被转换为进程的环境变量。

# apache cgi
这里有一个教程链接：https://httpd.apache.org/docs/2.4/howto/cgi.html


