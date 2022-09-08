# [Fly.io](https://fly.io/) Django Deployment Guide

## Introduction: What is Fly.io?

Fly.io is a fantastic deployment method for web developement for when you need to deploy a project quickly. But also need a back end. We're switching to fly.io due to Heroku ending their free tier in November of 2022.

## Setup: Creating an account

1. Go to [The Fly.io signup page](https://fly.io/app/sign-up).

<img src="https://i.imgur.com/SrYoCzq.jpg">


2. Creating an account/Signing Up:
	- Click on either the Github or the Email signup
	- Check the email you provided to log in and verify your email.
	-  Once you've opened the email verification link you'll come to the "Dashboard" page (image below).

<img src="https://i.imgur.com/xO2m3fm.jpg">


1. Finally we can start deploying out first Project! Click on "Deploy your app".

## Now install flyctl

1. You will need to install the flyctl, so click on the link to take you there.
2. If you're on macOS run the command:  `brew install flyctl`
3. If you're on windows running a WSL or on Linux run: `curl -L https://fly.io/install.sh | sh`
4. A successful install looks like this:

<img src="https://i.imgur.com/cteatzv.jpg">

5. We might have to add the flyctl to our path. It will prompt you with two commands you need to run one after the other. If you're on mac, add it to your bash profile, and if you're on windows it will be your `.zprofile` by running: `code ~/.zprofile`

<img src="https://i.imgur.com/w9Yevka.jpg">

### If your terminal looks like this, you're fine.
<img src="https://i.imgur.com/bRmpb3y.png">

6. Now we have to sign in with the our account using: `flyctl auth login`
7. You'll see these screen and follow the prompts. If you'd like to use [Privacy.com](https://privacy.com/) to make a virtual card with a $0 limit, that is a nice way to get these extra features.

<img src="https://i.imgur.com/He1Z27L.jpg">
<img src="https://i.imgur.com/B5c4S1F.jpg">

8. And now you're all set!

### We have an account! We can now deploy our sites!

###  Minor tweaks for preparation:
1. We need to add [whitenoise](http://whitenoise.evans.io/en/stable/django.html) to deal with our static files. So run `pip install whitenoise`
2.  So in your settings.py add this to your INSTALLED_APPS: `'whitenoise.runserver_nostatic',`
3. Then at the bottom of settings.py add in: `STATIC_ROOT = BASE_DIR / "staticfiles"
STATICFILES_STORAGE = "whitenoise.storage.CompressedManifestStaticFilesStorage"`

<img src="https://i.imgur.com/esvOKS8.jpg">
<img src="https://i.imgur.com/lqMnLNQ.jpg">

3. Now run `python3 manage.py collectstatic` to make sure everything is working.
4. After opening your Django project, if you don't have your requirements set run: `pip3 freeze > requirements.txt` and then run the command `python3 -m pip install -r requirements.txt`
5. We now need to make a procfile by running `touch Procfile`, and adding `web: gunicorn YourAppNameHere.wsgi` or if that give you errors during deployment `web: gunicorn YourAppNameHere:app`. Replace your app name in the line.

#### Deploying a Python App

1. Open the link to the fly docs on [Python](https://fly.io/docs/getting-started/python/) apps.
2. from your terminal run `flyctl launch` . And if this gives you an error you can run with the latest flyio docker image using : `flyctl launch --image flyio/hellofly:latest` or change the command to match the dockerimage in your app.
3. Add an app name, choose the closest server to you, and select `Y` when prompted to set up your database. You'll select the single shared cpu free tier. WRITE DOWN THE CREDENTIALS AS YOU ONLY SEE THEM HERE ONCE.
   <img src="https://i.imgur.com/57OglZg.jpg">
4. We also need to add the fly.io recomended db requirement using `pip install dj-database-url`
5. Fly will generate a `fly.toml` file for us and inside there we now have a default configuration. Before we finish we need to tell fly where to look for our environment variables with the secrets set command: 
### vvv REPLACE THESE FAKE VALUES vvv
   `flyctl secrets set KEY_NAME=somekindofvalue ANOTHER_KEY_NAME=someothervalue`
   
6. We now need to add in our database that fly made for us using those credentials. So run: `flyctl secrets set DATABASE_URL="postgres://postgres:SECRETPASSWORD@yourappname.internal:5432/yourappname"`
7.  We need to make sure our settings.py has 
   
   `ALLOWED_HOSTS = ['yourappname.fly.dev', '127.0.0.1']`

8.  Finally last step we need to do is check out fly.toml file and Dockerfile. In the Dockerfile you may need to change the last CMD line from: 
   
   `CMD ["gunicorn", "--bind", ":8080", "--workers", "2", "demo.wsgi"]`
   
   to:
   
   `CMD ["gunicorn", "--bind", ":8080", "--workers", "2", "yourappname.wsgi"]`

## fly.toml
   <img src="https://i.imgur.com/IzRC3Vg.jpg">

## Dockerfile
   <img src="https://i.imgur.com/EwxZ8oK.jpg">

9. If you recieve the error `failed ruinning usr/bin/python manage.py makemigrations: exit status 1`, try either changing in fly.toml: 
 
`release_command = "python manage.py migrate` to `python3 manage.py migrate`

10.  To deploy to fly when you're ready, run `flyctl deploy` which will look for our `fly.toml` file, start deploying and will return you to your regular command line when it's done. (This might take awhile.)
   <img src="https://i.imgur.com/C9XG15j.jpg">
11.  If the deploy command fails try one more time using `flyctl deploy --local-only` which will use your local docker daemon for the deploy.
12. To view our app and make sure everything worked correctly, we need to run `flyctl status` which gives us all the juicy details. And for errors we can run `flyctl logs`
13. To open our app from the command line run `flyctl open`


### Fly.io is totally free! But if you really like it, you can upgrade for some bells and whistles. We will NOT need to do so during this cohort.