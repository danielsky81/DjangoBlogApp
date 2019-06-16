# Django Blog Application Project

[![Build Status](https://travis-ci.org/danielsky81/DjangoBlogApp.svg?branch=master)](https://travis-ci.org/danielsky81/DjangoBlogApp)

## Intial Steps

1. Setting the virtual environment and activating it.
1. Installing the Dlango 1.11.17
1. Saving requirements
1. Creating new django project called `blog_app`
1. Running Django migrate
1. Starting development server at `http://127.0.0.1:8000/`
1. Git

## Travis

1. Go to the profile image and click on it to be redirected to all available Git repositiories. Select the required one and make it available to synchronize it with Travis.
1. Click on the repository name to copy a markup code. Then click on the icon saying 'build unknown' and copy the url with a `markdown` selection. Paste the code inside the README.md file to allow us to continually keep track of whether our project is passing the tests or not.
1. Create a new file called `.travis.yml` in the root directory to allows to integrate with `Travis`

```yml
dist: xenial   # required for Python >= 3.7
language: python
python:
    - "3.7"
# command to install dependencies
install:
    - pip3 install -r requirements.txt
script:
    - SECRET_KEY="whatever" ./manage.py test
```