#GOOGLE DRIVE UPLOADER
1.	Install *python2, python2-pip, python2-setuptools, python2-gdata* : `sudo pacman –S python2 python2-pip python2-setuptools`
2.	Install python2-gdata with : `sudo pacman -S python2-gdata`
3.	Install python-magic : `sudo pip2 install python-magic`
4.	Make python file, for example **gdrive.py** and write this line :
	```
	#!/usr/bin/python2 -u
	#
	# Google Docs file upload tool
	# (c)oded 2012 http://planzero.org/
	#
	# Requirements:
	# gdata >= 2.0.15  http://code.google.com/p/gdata-python-client/
	# magic >= 0.1     https://github.com/ahupp/python-magic (Note: this is NOT the
	#                  same as the python-magic Ubuntu package, which is older!)
	#
	
	# Imports
	from __future__ import division
	import sys, time, os.path, magic, atom.data, gdata.client, gdata.docs.client, gdata.docs.data
	
	# Settings
	# Enter your username and password here
	username = ' '
	password = ' '
	
	# Check arguments
	if len(sys.argv) < 2:
	    sys.exit('Usage: ' + sys.argv[0] + ' <filename> [collection]')
	
	# Heads up
	print 'Google Docs Upload Tool - v0.1 - http://planzero.org/'
	
	# Set the filename and collection
	filename = sys.argv[1]
	collection = sys.argv[2] if len(sys.argv) >= 3 else None
	
	# Open the file to be uploaded
	try:
	    fh = open(filename)
	except IOError, e:
	    sys.exit('ERROR: Unable to open ' + filename + ': ' + e[1])
	
	# Get file size and type
	file_size = os.path.getsize(fh.name)
	file_type = magic.Magic(mime=True).from_file(fh.name)
	
	# Create a Google Docs client
	docsclient = gdata.docs.client.DocsClient(source='planzero-gupload-v0.1')
	
	# Log into Google Docs
	print 'o Logging in...',
	try:
	    docsclient.ClientLogin(username, password, docsclient.source);
	except (gdata.client.BadAuthentication, gdata.client.Error), e:
	    sys.exit('ERROR: ' + str(e))
	except:
	    sys.exit('ERROR: Unable to login')
	print 'success!'
	
	# The default root collection URI
	uri = 'https://docs.google.com/feeds/upload/create-session/default/private/full'
	
	# If a collection name was set
	if collection:
	
	    # Get a list of all available resources (GetAllResources() requires >= gdata-2.0.15)
	    print 'o Fetching collection ID...',
	    try:
	        resources = docsclient.GetAllResources(uri='https://docs.google.com/feeds/default/private/full/-/folder?title=' + collection + '&title-exact=true')
	    except:
	        sys.exit('ERROR: Unable to retrieve resources')
	
	    # If no matching resources were found
	    if not resources:
	        sys.exit('ERROR: The collection "' + collection + '" was not found.')
	
	    # Set the collection URI
	    uri = resources[0].get_resumable_create_media_link().href
	    print 'Success!'
	
	# Make sure Google doesn't try to do any conversion on the upload (e.g. convert images to documents)
	uri += '?convert=false'
	
	# Create an uploader and upload the file
	# Hint: it should be possible to use UploadChunk() to allow display of upload statistics for large uploads
	t1 = time.time()
	print 'o Uploading file...',
	uploader = gdata.client.ResumableUploader(docsclient, fh, file_type, file_size, chunk_size=1048576, desired_class=gdata.data.GDEntry)
	new_entry = uploader.UploadFile(uri, entry=gdata.data.GDEntry(title=atom.data.Title(text=os.path.basename(fh.name))))
	print 'Success!'
	print 'Uploaded', '{0:.2f}'.format(file_size / 1024 / 1024) + ' MiB in ' + str(round(time.time() - t1, 2)) + ' seconds'
	```

5.	Add executable flag to the file and run it with : `./gdrive.py <nama file lokal> <nama folder GDrive>`

Referensi :
-	http://planzero.org/blog/2012/04/13/uploading_any_file_to_google_docs_with_python
-	http://jeremyblythe.blogspot.com/2012/06/motion-google-drive-uploader-and.html
