# GMAIL SENDER
1.	Install *python2, python2-pip, python2-setuptools, python2-gdata* : `sudo pacman –S python2 python2-pip python2-setuptools python2-gdata `
2.	Install gdata python dengan : `sudo pip2 install gdata`
3.	Install python-magic : `sudo pip2 install python-magic`
4.	Buat sebuah file python misal gmail.py dan isikan script berikut :
	```
	#!/usr/bin/python2
	
	"""
	GMAIL SENDER
	by
	Rodrigo Coutinho (http://kutuma.blogspot.com/2007/08/sending-emails-via-gmail-with-python.html)
	Added some code by EX4 (ekapujiw2002@gmail.com)
	
	Change log :
	2014/04/08:
		- Added getopt for parameter
	"""
	
	#the lib we used
	import smtplib
	from email.MIMEMultipart import MIMEMultipart
	from email.MIMEBase import MIMEBase
	from email.MIMEText import MIMEText
	from email import Encoders
	import os, sys, getopt
	
	#enter your gmail account and password here
	gmail_user = " "
	gmail_pwd = " "
	
	#the mail sender function
	def mail(to, subject, text, attach):
		#start mail head
		msg = MIMEMultipart()
	
		msg['From'] = gmail_user
		msg['To'] = to
		msg['Subject'] = subject
	
		msg.attach(MIMEText(text))
	
		#attachment
		try:
			if os.path.isfile(attach) and (attach!=""):
				part = MIMEBase('application', 'octet-stream')
				part.set_payload(open(attach, 'rb').read())
				Encoders.encode_base64(part)
				part.add_header('Content-Disposition','attachment; filename="%s"' % os.path.basename(attach))
				msg.attach(part)
		except IOError as e:
			sys.exit("Cannot open file attachment!!!")
			
		#send the email
		mailServer = smtplib.SMTP("smtp.gmail.com", 587)
		mailServer.ehlo()
		mailServer.starttls()
		mailServer.ehlo()
		mailServer.login(gmail_user, gmail_pwd)
		mailServer.sendmail(gmail_user, to, msg.as_string())
		# Should be mailServer.quit(), but that crashes...
		mailServer.close()
		
	#check param and send
	def check_param_and_mail(sys_arg):	
		
		#check param
		print "Checking parameter..."
		if len(sys_arg)<3:
			sys.exit('Invalid parameter.\r\nUsage : '+sys.argv[0] + ' -t <mail destination> -s <subject> -m <message> [-a <attachment>]')
			
		#parse param
		print "Parsing parameter..."
		try:
			opts, args = getopt.getopt(sys_arg,"ht:s:m:a:",["help=","to=","subject","message=","attachment"])
		except getopt.GetoptError:
			sys.exit('Parsing parameter fail.\r\nUsage : '+sys.argv[0] + ' -t <mail destination> -s <subject> -m <message> [-a <attachment>]')
		
		#get the param
		mail_to = ""
		mail_subjet = ""
		mail_message = ""
		mail_attachment = ""
		
		for opt, arg in opts:
			if opt in ("-h", "--help"):
				sys.exit('Usage : '+sys.argv[0] + ' -t <mail destination> -s <subject> -m <message> [-a <attachment>]')
			elif opt in ("-t", "--to"):
				mail_to = arg
			elif opt in ("-s", "--subject"):
				mail_subjet = arg
			elif opt in ("-m", "--message"):
				mail_message = arg
			elif opt in ("-a", "--attachment"):
				mail_attachment = arg
		
		#send the email	
		print "Sending email..."
		try:
			mail(mail_to,mail_subjet,mail_message,mail_attachment)
			print "Success"
		except:
			sys.exit("Fail at : "+sys.exc_info()[0])
	   
	# main program
	if __name__=="__main__":
		check_param_and_mail(sys.argv[1:])
	```

5.	Jalankan scriptnya dengan : `./gmail.py –t ke@xxx.com –s “subject email” –m “isi pesan email” –a [nama file attachment]`
	File attachment optional

Referensi:
- http://kutuma.blogspot.com/2007/08/sending-emails-via-gmail-with-python.html
- http://jeremyblythe.blogspot.com/2012/06/motion-google-drive-uploader-and.html
