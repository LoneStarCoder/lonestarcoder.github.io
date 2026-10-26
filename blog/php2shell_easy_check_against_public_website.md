# Here is a very quick and easy way to check which version of PHP a public website is presenting.

```
curl.exe -k -I -L https://www.example.com/
```
HTTP/1.1 200 OK

Date: Mon, 26 Oct 2026 15:57:00 GMT

Content-Type: text/html; charset=UTF-8

Connection: keep-alive

x-powered-by: PHP/7.4.33
