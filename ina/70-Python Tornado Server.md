# Python Tornado Server

Kali ini akan kita buat sebuah server menggunakan Python dengan library Tornado. Server ini akan memiliki fitur sebagai berikut :
1. Menggunakan port yang dapat di-custom oleh user
2. Dapat menggunakan koneksi SSL maupun non SSL
3. Memiliki end point websocket, baik fitur SSL (wss) maupun biasa (ws)
4. Memiliki end point http biasa
5. Memiliki broadcast websocket end point yang dapat diset intervalnya
6. Support semua file statik (html, css, javascript, apapun), dengan path resolving otomatis
7. Daemon task

Langkahnya adalah sebagai berikut :
1. Install library tornado untuk python2 dengan `sudo pip2 install tornado`
2. Buat folder **www** untuk isian file html nya. Untuk folder ini dapat diisi apapun.
3. Generate local SSL via console anda dnegan perintah `openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout myserver.crt.key -out myserver.crt.pem` atau dapat juga via online ssl generator misal : http://www.selfsignedcertificate.com/
4. Buat skrip pythonnya misal dengan nama **tornado1.py** dengan isian sebagai berikut :
```
#!/usr/bin/python2
'''
Simple http/https and websocket server with python tornado
'''

# main tornado lib
import tornado.ioloop, tornado.web, tornado.websocket

# for threading
import threading, Queue

# json
import json

#import RPi.GPIO as GPIO
import sys,time,random

# unique id
import uuid

'''
web server setting
'''
SERVER_SETTING = {
	'ROOT_PATH': './www/',
	'PORT': 8080,
	'DEBUG_MODE': True
}

# bind tornado logging
LOG_ERROR = tornado.log.app_log.error
LOG_INFO = tornado.log.app_log.info
LOG_WARNING = tornado.log.app_log.warning

# create the queue object
QUEUE_WEBSOCKET_MSG = Queue.Queue()

# websocket connected client
WS_CONNECTED_CLIENTS = []

# ws periodic job
WS_PERIODIC_JOB = None
WS_BROADCAST_MSG = ""

'''
	get last error msg
'''
def GetLastErrorMsg():
	return sys.exc_info()[1]

'''
	web api dynamic handler using http
'''
class WebApiHandler(tornado.web.RequestHandler):
	# make it asynchronous job
	@tornado.web.asynchronous
	
	# on get request
	def get(self):
		try:
			LOG_INFO("Get arguments")
			LOG_INFO(self.request.arguments)
		except:
			pass
		
		#always flush and finish it
		self.flush()
		self.finish()

'''
	websocket handler
'''
class ServerWebSocketHandler(tornado.websocket.WebSocketHandler):	
	
	def check_origin(self, origin):
		return True
		
	'''
		when client connect via websocket
	'''
	def open(self,*args, **kwargs):
		self.stream.set_nodelay(True)
		self.id = uuid.uuid4().hex
		WS_CONNECTED_CLIENTS.append(self)
		
		LOG_INFO("Websocket connection established for client %s" % (self.id))
		
		if not WS_PERIODIC_JOB.is_running():
			WS_PERIODIC_JOB.start()
			LOG_INFO("Websocket starting periodic job")
		else:
			LOG_INFO("Websocket periodic job already running")

	'''
		when server receive message from server
	'''
	def on_message(self, message):
		try:
			LOG_INFO("Websocket get message from %s : %s" % (self.id, message))
			QUEUE_WEBSOCKET_MSG.put(message)
		except:
			pass
			LOG_ERROR("Websocket error : %s" % GetLastErrorMsg())
		
	'''
		when client close connection
	'''
	def on_close(self):
		WS_CONNECTED_CLIENTS.remove(self)
		LOG_INFO("Websocket connection lost")
	
	'''
		broadcast message
	'''
	def broadcast(self, pkg, all_but=None):
		try:
			for c in WS_CONNECTED_CLIENTS:
				#if c.join_completed and c != all_but:
				if c != all_but:
					c.write_message(pkg)
		except:
			pass
			LOG_ERROR("Websocket broadcast error : %s" % GetLastErrorMsg())

'''
	broadcast global message over websocket
'''
def ws_broadcast():
		try:
			WS_BROADCAST_MSG = str(time.time())
			for c in WS_CONNECTED_CLIENTS:
				#if c.join_completed and c != all_but:
				#if c != all_but:
				#c.write_message("%s,%s" % (c.id, WS_BROADCAST_MSG))
				#c.write_message(json.dumps({'id':c.id, 'msg':WS_BROADCAST_MSG}))
				c.write_message("{\"id\":\"%s\",\"msg\":%f}" % (c.id, time.time()))
		except:
			pass
			LOG_ERROR("Websocket broadcast error : %s" % GetLastErrorMsg())
			
'''
	application handler for tornado
	this will handle all the path below this folder automatically
'''
tornado_web_application = tornado.web.Application(
	[		
		# dynamic content handler
		(r"/api",WebApiHandler),
		
		# websocket handler
		(r"/ws",ServerWebSocketHandler),
		
		# for static content. this is global handler, put it after all definitive handler
		(r"/(.*)", tornado.web.StaticFileHandler, 
			{
				'path': SERVER_SETTING['ROOT_PATH'], 
				'default_filename': 'index.html'
			}
		)
	],
	debug = SERVER_SETTING['DEBUG_MODE'])
	
'''
	start a process to be threading in daemonize mode
	ref:
	- https://stackoverflow.com/questions/30913201/pass-keyword-arguments-to-target-function-in-python-threading-thread
	- https://gist.github.com/sunng87/5152427
'''
def start_daemon(f=None, args=None):
	if not f is None:
		if not args is None:
			t = threading.Thread(target=f, kwargs=args)
		else:
			t = threading.Thread(target=f)
		t.setDaemon(True)
		t.start()
		
'''
	simple task
'''
def task_alive_beats():
	while True:
		try:
			LOG_INFO(
				"Alive beat at %f %s>>" % 
				(
					time.time(), 
					"="*(int(round(time.time() % 60)))
				)
			)
			time.sleep(1)
		except:
			pass
			LOG_ERROR("Alive beat error : %s" % GetLastErrorMsg())

'''
	the main loop app
'''
def main_app_loop():
	try:
		# enable pretty logging
		tornado.log.enable_pretty_logging()
		
		# ssl option is on
		ssl = False
		if len(sys.argv) == 2:
			ssl = (sys.argv[1] == 'ssl')
		LOG_INFO("SSL option status = %s" % ssl)
		
		# create http server with ssl-certs
		if ssl:
			http_server = tornado.httpserver.HTTPServer(
				tornado_web_application, 
				ssl_options={
					"certfile": "myserver.crt.pem",
					"keyfile": "myserver.crt.key",
				}
			)
		else:
			http_server = tornado.httpserver.HTTPServer(
				tornado_web_application
			)
		
		# bind to port
		#tornado_web_application.listen(SERVER_SETTING['PORT'])
		http_server.listen(SERVER_SETTING['PORT'])
		
		global WS_PERIODIC_JOB
		WS_PERIODIC_JOB = tornado.ioloop.PeriodicCallback(ws_broadcast, 100)
		
		# start a daemonize task
		start_daemon(task_alive_beats)
		
		# start ioloop
		LOG_INFO("Server loop io started at port %d" % (SERVER_SETTING['PORT']))
		tornado.ioloop.IOLoop.instance().start()
		
	except:
		pass
		print('\n')
		LOG_INFO("Bye bye...")
		
'''
main program start here
'''
if __name__ == "__main__":
	main_app_loop()
```
5. Jalankan filenya dengan `python2 tornado1.py` atau versi ssl nya dengan `python2 tornado1.py ssl`. Pastikan anda sudah menyesuaikan nama file ssl dan path root nya.
6. Akses server nya dengan `http://ip_raspi:8080` atau `https://ip_raspi:8080`
7. Untuk websocket dapat diakses via `ws://ip_raspi:8080/ws` atau `wss://ip_raspi:8080/ws`. Saya sarankan menggunakan ssl kalau websocket akan diakses dari device lainnya, karena terkadang burst data yang cepat dari websocket dianggap sebagai bomb
8. Contoh untuk akses websocket dapat dilihat pada file html berikut ini :
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
    </head>
    <body>
        <a href="javascript:WebSocketTest()">Run WebSocket</a>
        <div id="messages" style="height:200px;background:black;color:white;"></div>
		
		<script type="text/javascript">
        var messageContainer = document.getElementById("messages");
		//console.log(messageContainer);
        function WebSocketTest() {
            if ("WebSocket" in window) {
                messageContainer.innerHTML = "WebSocket is supported by your Browser!";
                //secure version
				//var ws = new WebSocket("wss://192.168.1.25:8080/ws");
				
				//unsecure version
				var ws = new WebSocket("ws://192.168.1.25:8080/ws");
				
                ws.onopen = function() {
                    ws.send("Message to send");
                };
                ws.onmessage = function (evt) { 
                    var received_msg = evt.data;
                    messageContainer.innerHTML = "Message is received...";
                };
                ws.onclose = function() { 
                    messageContainer.innerHTML = "Connection is closed...";
                };
            } else {
                messageContainer.innerHTML = "WebSocket NOT supported by your Browser!";
            }
        }
        </script>
    </body>
</html>
```
9. Server akan secara periodik per 100ms mengirimkan data ke semua websocket client yang terkoneksi ke server. Jadi ya sangat cepat

Referensi :
- https://gist.github.com/sunng87/5152427
- http://www.codestance.com/tutorials-archive/python-tornado-web-server-with-websockets-part-i-441
- https://github.com/hiroakis/tornado-websocket-example
- http://www.giantflyingsaucer.com/blog/?p=4586
- https://stackoverflow.com/questions/27757506/broadcasting-a-message-using-tornado
- https://stackoverflow.com/questions/18531849/python-tornado-send-message-to-all-connections
- http://www.tornadoweb.org/en/stable/ioloop.html
- https://stackoverflow.com/questions/534839/how-to-create-a-guid-uuid-in-python
- https://stackoverflow.com/questions/20636145/tornado-ssl-certs
