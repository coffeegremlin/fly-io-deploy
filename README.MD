# [Fly.io](https://fly.io/) Deployment Guide

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

5. Now we have ti add the flyctl to our path. It will prompt you with two commands you need to run one after the other.

<img src="https://i.imgur.com/w9Yevka.jpg">

6. Now we have to sign in with the our account using: `flyctl auth login`
7. You'll see these screen and follow the prompts. If you'd like to use [Privacy.com](https://privacy.com/) to make a virtual card with a $0 limit, that is a nice way to get these extra features.

<img src="https://i.imgur.com/He1Z27L.jpg">
<img src="https://i.imgur.com/B5c4S1F.jpg">

8. And now you're all set!

### We have an account! We can now deploy our sites!

###  Minor tweaks for preparation:
1. In your settings.py add this to your INSTALLED_APPS: `'whitenoise.runserver_nostatic',`
2. Then at the bottom of settings.py add in: `STATIC_ROOT = BASE_DIR / "staticfiles"
STATICFILES_STORAGE = "whitenoise.storage.CompressedManifestStaticFilesStorage"`
3. After opening your Django project, if you don't have your requirements set run: `pip3 freeze > requirements.txt` and then run the command `python3 -m pip install -r requirements.txt`
4. We now need to make a procfile by running `touch Procfile`, and adding `web: gunicorn YourAppNameHere:app`. Replace your app name in the line.

#### Deploying a Python App

1. Open the link to the fly docs on [Python](https://fly.io/docs/getting-started/python/) apps.
2. from your terminal run `flyctl launch`
3. Add an app name, choose the closest server to you, and select `Y` when prompted to set up your database. You'll select the sincle shared cpu free tier. WRITE DOWN THE CREDENTIALS AS YOU ONLY SEE THEM HERE ONCE.
   <img src="https://i.imgur.com/57OglZg.jpg">
4. Fly will generate a `fly.toml` file for us and inside there we now have a default configuration. Before we finish we need to tell fly where to look for our environment variables with: `flyctl secrets import < ./yourappname/.env`
5. To deploy to fly when you're ready, run `flyctl deploy` which will look for our `fly.toml` file, start deploying and will return you to your regular command line when it's done. (This might take awhile.)
   <img src="https://i.imgur.com/C9XG15j.jpg">
6. If the deploy command fails try one more time using `flyctl deploy --local-only` which will use your local docker daemon for the deploy.
7. To view our app and make sure everything worked correctly, we need to run `flyctl status` which gives us all the juicy details. And for errors we can run `flyctl logs`
8.  To open our app from the command line run `flyctl open`


### Fly.io is totally free! But if you really like it, you can upgrade for some bells and whistles. We will NOT need to do so during this cohort.