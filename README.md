# Purpose
Running long processes from django is hard.  There are many service based solutions, but those aren't always wanted.  

This lib spawns a new process from a django session.  Typically used to run manage.py commands.  If you need communication back to django, I use memcached to excahnge data.

posix_spawn() is ideal to use over fork() because it doesn't copy your database/network/file sockets to the new process.  Running fork() inside django can cause major issues if you don't clean up correctly.

## Usage

I assume your django app name is: **website**

1. Inside your django app, create a libs dir: $(BASE)/website/libs
2. copy django_posix_spawn into libs: $(BASE)/website/libs/django_posix_spawn
3. Run a command in the background:

		from website.libs.django_posix_spawn import posix_spawn, run_manage
		
			#same as running: Echo 9
		posix_spawn( '/usr/bin/echo', ['9']) 
		
			#same as runing: /usr/bin/python manage.py migrate
		run_manage( 'migrate' )
		

## Tip of the hat

This code was taken from: https://github.com/projectcalico/python-posix-spawn
I made modifications to:
 * Run in python3
 * Prevent zombie child processes
 * Fit into django smoothly
 * Compile C code without needing a custom setup/install script
 * Handle arguments in a user friendly way
