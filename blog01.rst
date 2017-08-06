====================================
A minimal Django file upload example
====================================

1. Create a django project
--------------------------

**Create a django project: django-admin.py startproject sample**

now a folder(**sample**) is created::
	
	sample/
		manage.py
		sample/
			__init__.py
			settings.py
			urls.py
			wsgi.py	

2. Update settings.py and urls.py
---------------------------------
**On `settings.py` add:**::
	
	MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
	MEDIA_URL = '/media/'

**`urls.py` add:**::

	...<other imports>...
	from django.conf import settings
	from django.conf.urls.static import static
	
	urlpatterns = patterns('',
		url(r'^upload/$', 'uploader.views.home', name='imageupload'),
		...<other url patterns>...
	)+ static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)

3. create an app
----------------

**Create an app: `python manage.py startapp uploader`**

**Now a folder(`uploader`) with these files are created:**
::

	uploader/
		__init__.py
		models.py
		admin.py
		tests.py
		views.py			
		
4. Update settings.py
---------------------

**On `setting.py` -> `INSTALLED_APPS` add `'uploader',`, ie:**
::

	INSTALLED_APPS = (
		...
		'uploader',
		...
	)

5. Update models
----------------

**update `models.py`**
::

	from django.db import models
	from django.forms import ModelForm

	class Upload(models.Model):
	    pic = models.ImageField(""Image"", upload_to=""images/"")    
	    upload_date=models.DateTimeField(auto_now_add =True)

	# FileUpload form class.
	class UploadForm(ModelForm):
		class Meta:
			model = Upload


6. Update views.py
------------------

**update `views.py`**
::

	from django.shortcuts import render
	from uploader.models import UploadForm,Upload
	from django.http import HttpResponseRedirect
	from django.core.urlresolvers import reverse
	# Create your views here.
	def home(request):
		if request.method==""POST"":
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

**Create a folder `templates` and create a file `home.html`:**
::

	<div style=""padding:40px;margin:40px;border:1px solid #ccc"">
		<h1>picture</h1>
		<form action=""#"" method=""post"" enctype=""multipart/form-data"">
			{% csrf_token %} {{form}} 
			<input type=""submit"" value=""Upload"" />
		</form>
		{% for img in images %}
			{{forloop.counter}}.<a href=""{{ img.pic.url }}"">{{ img.pic.name }}</a>
			({{img.upload_date}})<hr />
		{% endfor %}
	</div>


Final Project tree::

	sample/
		manage.py
		sample/
			__init__.py
			settings.py
			urls.py
			wsgi.py				
		uploader/
			__init__.py
			models.py
			views.py			
			templates/
				home.html 


8. Sync db 
----------

**Syncronize database and runserver**
::

	>> python manage.py syncdb
	>> python manage.py runserver

	visit <http://localhost.com:8000>"