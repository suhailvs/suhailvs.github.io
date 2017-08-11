================================
Mail setup on django using Gmail
================================

Create an Application specific password
---------------------------------------

+ Visit your [Google Account security page](https://www.google.com/settings/security).

+ In the `2-Step Verification` box, click `Settings` (if there is no settings link, you may want to create a new one. you can skip step 3 & 4).

+ Click the tab for `App-specific passwords`.

+ Click `Manage your application specific passwords`.

+ Under the `Application-specific passwords` section, enter a descriptive name for the application you want to authorize, such as ""Django gmail"" then click `Generate application-specific password` button.

+ note down the password. for example: `smbumqjiurmqrywn` ![password][1]

Then update `settings.py`:
--------------------------

::

    EMAIL_HOST = 'smtp.gmail.com'
    EMAIL_HOST_USER = 'your-username@gmail.com'
    EMAIL_HOST_PASSWORD = 'Application spectific password(for eg: smbumqjiurmqrywn)'
    EMAIL_PORT = 587
    EMAIL_USE_TLS = True


You can use the shell to test it:
---------------------------------
::

    python manage.py shell
    >>> from django.core.mail import send_mail
    >>> send_mail('Test', 'This is a test', 'your@email.com', ['toemail@email.com'],
         fail_silently=False)


  [1]: http://i.stack.imgur.com/fNxSx.png"