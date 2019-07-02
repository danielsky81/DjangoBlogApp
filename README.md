# Django Blog Application Project

[![Build Status](https://travis-ci.org/danielsky81/DjangoBlogApp.svg?branch=master)](https://travis-ci.org/danielsky81/DjangoBlogApp)

## Intial Steps

1. Setting the virtual environment and activating it.
1. Installing the Django 1.11.17 (*further update to 1.11.21 due to vurnability)
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

1. Create views in the `views.py` and add new urls to `urls.py` file inside the `blog_app` folder to redirect users to the posts app. Then we need to import all views and create urlpatterns for them inside the `urls.py` file in the `posts` folder.

## Templates creation

1. Create a `template` folder in the root directory.
1. Add a `base.html` template that's going to hold our navigation bar and allow us to navigate through the rest of the project.
1. Add `custom.css` file to `css` folder inside `static`
1. We need to create additional pages and import django forms form bootstrap library as follows `pip install django-forms-bootstrap`. We need to add this to the `INSTALLED_APPS` inside the `settings.py` file like so `django_forms_bootstrap`
1. create our super user now that all of our views and templates have been created `./manage.py createsuperuser`

## Deployement to heroku

1. We need to remove the `SECRET_KEY` from the `settings.py` file and import it from the local environment instead, so we need to make sure that the `env.py` exists before importing it like so

```py
if os.path.exists('env.py'):
    import env

SECRET_KEY = os.environ.get('SECRET_KEY')
```

1. Create an `env.py` file in the root directory

```py
import os

os.environ.setdefault("SECRET_KEY", "!7o4yf&4t+#l@(sflou_yq+@a-%o#82xat1cg-q94($qc1@mtr")
```

1. Let's create a heroku app by login in Heroku and clicking on NEW button visible in the dashboard `django-blog-appplication`.
1. We're going to use a scalable SQL database called PostgreSQL, rather than the sqlite database that Django uses on our local machine. So to provision this, click on the resources tab, scroll down to add-ons and just start typing Postgres. Then select Heroku Postgres, select the Hobby Dev - Free plan, and click on the provision button.
1. If we now go to our settings tab and click on the reveal config files button, we can see we have a database URL there that's been created. While we're here, let's put our SECRET_KEY variable in. So this is the environment variable for Heroku, effectively the same as our env.py file that we created.
1. Now that we've done that, we need to add a couple more libraries to our project `pip install dj-database-url psycopg2-binary`. And what these libraries allow us to do is to connect to a database using a URL, rather than the standard database driver that Django uses.
1. Now sice we have new requirement let's update it `pip freeze > requirements.txt`
1. Now we just need to update our `settings.py` file so that it uses our new database URL.

```py
import dj_database_url

DATABASES = {
    'default': dj_database_url.parse(os.environ.get('DATABASE_URL'))
}
```

1. But when we're testing it locally, we don't have that database URL set. So we need to add that into our `env.py` file as well.

```py
os.environ.setdefault("DATABASE_URL", "postgres://ehpujhnsuretho:1cbfeb035cdbca6676fda04f6fd15dbf76fee153e8995231ea97c115a1ad17f8@ec2-54-246-84-100.eu-west-1.compute.amazonaws.com:5432/d9350caj6n1eft")
```

1. Once connected we should be able to connect to our database `./manage.py makemigrations` and `./manage.py migrate`
1. To complete our deployment of our code to Heroku we need to add some more libraries. The first one that we need is called `Whitenoise`. This allows us to host our static files, such as CSS and JavaScript, correctly on Heroku. `pip install whitenoise`, update `requirements.txt` and add it to the `settings.py` under the `MIDDLEWARE` like so `'whitenoise.middleware.WhiteNoiseMiddleware',` Then we need to add `STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')` to the `settings.py` file as this just gives a collection point for our static files. When we deploy to Heroku, it's going to collect those into that location and deploy them, then, to our project.
1. What we also just want to do is make some changes to our database setting here. And this is just to keep our continuous integration testing with Travis working. We're not going to, as we said, upload our env.py file, so Travis isn't going to know what our database URL is. So let's add an if statement here that says if database URL is in our environment, then we use the URL.

```py
if "DATABASE_URL" in os.environ:
    DATABASES = {
        'default': dj_database_url.parse(os.environ.get('DATABASE_URL'))
    }
else:
    print("Postgres URL not found, using sqlite instead")
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
        }
    }
```
1. Now let's just make sure that our env.py file is in our `.gitignore` file because we do not want those secret keys to be uploaded to GitHub.
1. And finally, we just need to create a `Procfile` and add `web: gunicorn blog.wsgi:application`. We know that this is how Heroku runs our applications. So we just need to install the gunicorn library as well `pip install gunicorn` and save in requirements `pip freeze > requirements.txt`
1. So now we're just about ready to start our deployment, but we've got one more setting that we just need to change in our `settings.py` file `ALLOWED_HOSTS = [os.environ.get("C9_HOSTNAME"), '127.0.0.1', 'django-blog-appplication.herokuapp.com']` We need to do this, otherwise, when we do deploy to Heroku, we'll get a forbidden error when we try to run it because it's not in the allowed hosts.