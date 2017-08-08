## Deployment

run **make html** on source branch, 

	$ make html

then checkout to **master** branch, then remove files excpet **_build** folder, then copy items in folder **_build/html** to root folder

**Rename folders: `_static --> static`,`_images --> images`::

	$ mv _static static

**now rename every occurrence of `src="_static/` with `src="static/`:**

    $ find ./ -type f -readable -writable -exec sed -i 's/href="_static/href="static/g' {} \;