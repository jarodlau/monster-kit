
```
[CGI设置]
server.modules += ("mod_cgi")
cgi.assign                  = ( ".py" => "D:/python25/python.exe")
server_root = "C:/LightTPD"
alias.url = ( "/cgi-bin" => server_root + "/cgi-bin" )

[CGI例子]
import datetime,os

print 'Content-type: text/html\n\n'
print '<H1>Hello!</H1>'
print 'Server time:', datetime.datetime.now()
print '<BR>'
print 'Client IP:',os.environ['REMOTE_ADDR']

[FastCGI设置]
就算设置了也没用，python在windows下的fastcgi库找了半天没找到，官方提供的python-fastcgi需要编译，机器上没有VC,使用cygwin运行configure然后用python setup.py build -c mingw32来build结果最后一步时不行，十分郁闷
alias.url += ( "/fcgi-bin" => server_root + "/fcgi-bin" )
fastcgi.server = ( ".fcgi" =>
	( "localhost" =>
		(
#			"socket" => "fcgi.sock",
			"bin-path" => "D:/python25/python.exe",
			"port" => 8888,
			"min-procs" => 2
		)
	)
)

```