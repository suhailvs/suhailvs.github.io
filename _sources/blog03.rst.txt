=======================
Publish pages in Github
=======================


1) Make a fresh clone
---------------------
::
    
    cd ..
    git clone https://github.com/danielfpedro/simple-modal.git simple-modal-webpage

2) Create a gh-pages branch
---------------------------
::

    cd simple-modal-webpage
    git checkout --orphan gh-pages
    git rm -rf . 

3) Add content and push
-----------------------
::

    echo ""My GitHub Page"" > index.html
    git add index.html
    git commit -a -m ""First pages commit""
    git push origin gh-pages

After your push to the `gh-pages` branch, your Project Page will be available at `danielfpedro.github.io/simple-modal`

4) Pull the branch
------------------

pull the branch `gh-pages` in original repo(`simple-modal`)

::

    cd ..
    rm -r simple-modal-webpage
    cd simple-modal
    git fetch   


5) Use New branch `gh-pages`
----------------------------

now you have 1 more branch `gh-pages`. use it to commit changes for github webpage

::

    git checkout gh-pages
    <make changes to documentation>
    git add .
    git commit -m ""documentation improvements""
    git push -u origin gh-pages
    