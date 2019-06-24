# Django Blog Application Project

[![Build Status](https://travis-ci.org/danielsky81/DjangoBlogApp.svg?branch=master)](https://travis-ci.org/danielsky81/DjangoBlogApp)

## Intial Steps

1. Setting the virtual environment and activating it.
1. Installing the Django 1.11.17
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

## Adding apps

1. Create an app called `posts` and `templates` directory inside of it.
1. Add `media` and `static` folders
1. Update `settings.py` file to tell Django that we have an app called posts.
1. Now that we've done that, we want to be able to serve out the templates. So we need to tell Django where the templates directory is.  So inside `TEMPLATES` setting we include `'DIRS': [os.path.join(BASE_DIR, 'templates')]`
1. just to make sure that we can serve out our media files properly, we need to add a new context_processor under options like so `django.template.context_processors.media`
1. To serve out our `media` directory let's create a media URL `MEDIA_URL = '/media/'`
1. And it's worth remembering that this is just how Django refers to the URL. It's not actually referring to a specific directory at the moment. To get it to do that, we need to add the media root setting, which we'll go ahead and do now. `MEDIA_ROOT = os.path.join(BASE_DIR, 'media')`. So now that we've done that, we need to do exactly the same for our static URL. Again, this is just a label. To actually get it to connect to the right directory and serve the right directory out, we need to use `STATICFILES_DIRS = (os.path.join(BASE_DIR, 'static'), )`

So now we've set up our new app and our project to serve out templates, media files, and static files.

## Create Models, Views And URLs

1. First of all, from `django.utils`, we want to import timezone because we're going to be doing some work with time and date.
1. We add Django forms library by creating a `forms.py` file. And the fields that we're going to have on our form are user editable fields on a blog post: the title, the content, the published date, our image, and also our tags.
1. Now just to make sure that our posts are accessible through the admin backend. We import our Post class and we're going to register our Post class with our admin site `admin.site.register(Post)`.
1. Because we're using image field, we need to add a library called `pillow` via the CLI `pip install pillow`. Once pillow is installed, we need to add it to the requirements for our project `pip freeze > requirements.txt`.
1. Now we need to do migrations for `posts` via terminal `./manage.py makemigrations` and update this migration to our database `./manage.py migrate`.


Now that we've created our models and our forms, then we can go ahead and start creating our views. After that, we'll add our URLs and our templates, which will tie our whole project together and make it a fully working project.

