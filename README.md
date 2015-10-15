# p4-linux-server-configuration

# Overview
 - Domain: [https://ptutorials.mypassion.io/][1]
 - IP: 52.27.194.120
 - SSH port: 2200
 - To view the source code of the main application being hosted, click [here][4].

# Super Users
- root (no remote access)
- luiz (Remote access with udacity_key.rsa)
- grader (Remote access udacity_key.rsa)

# Apache
- Programming Tutorials is being hosted using apaches virtual host. 
- Main app directory: `/var/www/programming-tutorials`. 
- Main app config: `/etc/apache2/sites-available/programming-tutorials.conf`.
    - `programming-tutorials.wsgi` that is passed to apache.
    - `google-secret.json` required for Google OAuth athentication
    - `venv folder` containing 3rd party python libraries used by the application. 
    - `db_admin.py` to conveniently edit the database using SQLAlchemy
    - `app folder` containing the main application, including the `static folder`.
        - With the current configuration, any URL not matching the static alias `/static` will be processed by the WSGI application. This means files outside the static folder are hidden from the user.

# Bare Git Repo
- located in `/home/luiz/programming-tutorials`.
- contains post-receive hook

When the bare repo receives a push containing the production branch, the post-receive hook will move all project files to `/var/www/programming-tutorials`, run `~/scripts/setup.sh` then restart apache gracefully.

`~/scripts/setup.sh` does the following:

- Replaces the app id placeholders for Google, GitHub and Facebook with the valid app ids.
- Replaces postgresql connection URI with the correct username and password
- Copies `google-secret.json` to `/var/www/programming-tutorials`


# Firewall Config
 - Allows all outgoing connections
 - Only allow incoming connections for
	- SSH (port 2200)
	- HTTP (port 80)
	- NTP (port 123)
- If in doubt, check `sudo ufw status`

# PostgreSQL
- Database name: `p_tuts`
- Username: `p_tuts`
- User `p_tuts` is restricted to `p_tuts` database
- Remote connection is disabled. 
    - If in doubt, `cat /etc/postgresql/9.3/main/pg_hba.conf`

# Package Installation Summary
- Apache:
    - apache2
	- libapache2-mod-wsgi
- PostgreSQL:
	- postgresql
	- postgresql-contrib
- Git:
    - git
- Python:
	- python-dev 
	- python-setuptools
	- python-pip
	- python-virtualenv
	- Python libraries (project specific):
		- `/var/www/programming-tutorials/requirements.txt`

# References
- [How To Use Git Hooks To Automate Development and Deployment Tasks][2]
- [How To Deploy a Flask Application on an Ubuntu VPS][2]

[1]: https://ptutorials.mypassion.io/
[2]: https://www.digitalocean.com/community/tutorials/how-to-use-git-hooks-to-automate-development-and-deployment-tasks
[3]: https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
[4]: https://github.com/LuizGsa21/p3-programming-tutorials

