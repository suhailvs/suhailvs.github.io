## Deployment

run **make html** on master branch, 

	$ make html

then checkout to **gh-pages** branch, then remove files excpet **_build** folder, then copy items in folder **_build/html** to root folder

**Rename folders: `_static --> static`,`_images --> images`::

	$ mv _static static
	$ mv _images images

**now rename every occurrence of `src="_images/` with `src="images/`:**

    $ find ./ -type f -readable -writable -exec sed -i 's/src="_images/src="images/g' {} \;

    $ find ./ -type f -readable -writable -exec sed -i 's/href="_static/href="static/g' {} \;