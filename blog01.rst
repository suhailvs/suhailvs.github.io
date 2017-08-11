====================================
A minimal Django file upload example
====================================

1. Create a django project
--------------------------

Run startproject::

    $ django-admin.py startproject sample

now a folder(**sample**) is created::
	
	sample/
	  manage.py
	  sample/
	    __init__.py
	    settings.py
	    urls.py
	    wsgi.py	

2. create an app
----------------

Create an app::

	python manage.py startapp uploader

Now a folder(`uploader`) with these files are created::

	uploader/
	  __init__.py
	  admin.py
	  app.py
	  models.py
	  tests.py
	  views.py
	  migrations/
	    __init__.py

3. Update settings.py
---------------------

On `settings.py` add `'uploader'` to `INSTALLED_APPS` and add `MEDIA_ROOT` and `MEDIA_URL`, ie::

	INSTALLED_APPS = (
		...<other apps>...
		'uploader',
	)

	MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
	MEDIA_URL = '/media/'

4. Update urls.py
-----------------

in `urls.py` add::

	...<other imports>...
	from django.conf import settings
	from django.conf.urls.static import static
	from uploader import views as uploader_views
	
	urlpatterns = [
		...<other url patterns>...
		url(r'^upload/$', uploader_views.home, name='imageupload'),
	]+ static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)


5. Update models
----------------

update `models.py`::

	from django.db import models
	from django.forms import ModelForm

	class Upload(models.Model):
		pic = models.FileField(upload_to="images/")    
		upload_date=models.DateTimeField(auto_now_add =True)

	# FileUpload form class.
	class UploadForm(ModelForm):
		class Meta:
			model = Upload
			fields = ('pic',)


6. Update views.py
------------------

update `views.py`::

	from django.shortcuts import render
	from uploader.models import UploadForm,Upload
	from django.http import HttpResponseRedirect
	from django.core.urlresolvers import reverse
	# Create your views here.
	def home(request):
		if request.method=="POST":
			img = UploadForm(request.POST, request.FILES)		
			if img.is_valid():
				img.save()	
				return HttpResponseRedirect(reverse('imageupload'))
		else:
			img=UploadForm()
		images=Upload.objects.all()
		return render(request,'home.html',{'form':img,'images':images})


7. create templates
-------------------

Create a folder **templates** in folder **uploader**, then create a file **home.html**, ie **sample/uploader/templates/home.html**::

	<div style="padding:40px;margin:40px;border:1px solid #ccc">
		<h1>picture</h1>
		<form action="#" method="post" enctype="multipart/form-data">
			{% csrf_token %} {{form}} 
			<input type="submit" value="Upload" />
		</form>
		{% for img in images %}
			{{forloop.counter}}.<a href="{{ img.pic.url }}">{{ img.pic.name }}</a>
			({{img.upload_date}})<hr />
		{% endfor %}
	</div>


8. Syncronize database
----------------------

Syncronize database and runserver::

	>> python manage.py makemigrations
	>> python manage.py migrate
	>> python manage.py runserver

	visit <http://localhost.com:8000/upload>