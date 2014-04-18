#MAKE TWITTER BOT
1. Install **python, python-pip** : `sudo pacman -S python python-pip python-setup-tools`
2. Install **python twython package** : `sudo pip install twython`
3. Register *app* you're gonna build to `https://apps.twitter.com/app/new`
4. Note the **API Key** and **API Secret** from tab **API Keys** :
	> API key	tZFstqoPos9EzZCXWbThR5IIs

	> API secret	LqdArL2Laevzbt9xtHDofVeAZemNoVPjN0PIWTeTZz5pgsokjx

5. Also note **access token** and **secret** :
	> Access token			597422837-DI74enAEqJRgJ3D8nOqKOxj7wSQLWd20fJbnRQUG
	
	> Access token secret		Eyfm2GYNLMFcBKLNqcwmUE94oICYSSviRr1RaEP2b0N9T

6. Make *script python* and save as *twitbot.py* for example :

	```
	#!/usr/bin/env python
	import sys
	from twython import Twython
	API_KEY = 'tZFstqoPos9EzZCXWbThR5IIs'
	API_SECRET = 'LqdArL2Laevzbt9xtHDofVeAZemNoVPjN0PIWTeTZz5pgsokjx'
	ACCESS_TOKEN = '597422837-DI74enAEqJRgJ3D8nOqKOxj7wSQLWd20fJbnRQUG'
	ACCESS_SECRET = 'Eyfm2GYNLMFcBKLNqcwmUE94oICYSSviRr1RaEP2b0N9T'
	
	api = Twython(API_KEY,API_SECRET,ACCESS_TOKEN,ACCESS_SECRET) 
	
	api.update_status(status=sys.argv[1][:140])
	```

7. Add *flag executable* to the script : `chmod +x twitbot.py`
8. Test with : `python twitbot.py 'My 1st tweet from RaspberryPi'`
	You should see your tweet
9. Now, let's do some *upload tweet* with photo :)

	```
	#!/usr/bin/env python
	
	# lib
	import sys
	from twython import Twython
	
	# key app
	API_KEY = 'tZFstqoPos9EzZCXWbThR5IIs'
	API_SECRET = 'LqdArL2Laevzbt9xtHDofVeAZemNoVPjN0PIWTeTZz5pgsokjx'
	ACCESS_TOKEN = '597422837-DI74enAEqJRgJ3D8nOqKOxj7wSQLWd20fJbnRQUG'
	ACCESS_SECRET = 'Eyfm2GYNLMFcBKLNqcwmUE94oICYSSviRr1RaEP2b0N9T'
	
	# akses api
	api = Twython(API_KEY,API_SECRET,ACCESS_TOKEN,ACCESS_SECRET) 
	
	# akses poto
	photo = open('logo.jpeg','rb')
	
	# send tweet dg pic
	api.update_status_with_media(media=photo, status=sys.argv[1][:140])
	```

	Congratulation, you just made a twitter bot from the raspi
