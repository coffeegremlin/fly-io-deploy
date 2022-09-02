# [Fly.io](https://fly.io/) Deployment Guide

## Introduction: What is Fly.io?

Fly.io is a fantastic deployment method for web developement for when you need to deploy a project quickly. But also need a back end. We're switching to fly.io due to Heroku ending their free tier in November of 2022.

## Setup: Creating an account

1. Go to [The Fly.io signup page](https://fly.io/app/sign-up).

<img src="">


2. Creating an account/Signing Up:
	- Click on either the Github or the Email signup
	- Check the email you provided to log in and verify your email.
	-  Once you've opened the email verification link you'll come to the "Dashboard" page (image below).

<img src="">


1. Finally we can start deploying out first Project! Click on "Deploy your app".

<img src="">

## Now install flyctl

1. You will need to install the flyctl, so click on the link to take you there.
2. If you're on macOS run the command:  `brew install flyctl`
3. If you're on windows running a WSL or on Linux run: `curl -L https://fly.io/install.sh | sh`
4. A successful install looks like this:

<img src="">

5. Now we have ti add the flyctl to our path. It will prompt you with two commands you need to run.

<img src="">

6. Now we have to sign in with the our account using: `flyctl auth login`
7. You'll see these screen and follow the prompts. If you'd like to use [Privacy.com](https://privacy.com/) to make a virtual card with a $0 limit, that is a nice way to get these extra features.
8. And you're set!

### We have an account! We can now deploy our sites!

#### Deploying a Python App

1. Open the link to the fly docs on [Python](https://fly.io/docs/getting-started/python/) apps.
2. After


### Fly.io is totally free! But if you really like it, you can upgrade for some bells and whistles. We will NOT need to do so during this cohort.