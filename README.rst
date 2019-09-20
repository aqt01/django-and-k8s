django-k8s
==========

django-k8s

.. image:: https://img.shields.io/badge/built%20with-Cookiecutter%20Django-ff69b4.svg
     :target: https://github.com/pydanny/cookiecutter-django/
     :alt: Built with Cookiecutter Django
.. image:: https://img.shields.io/badge/code%20style-black-000000.svg
     :target: https://github.com/ambv/black
     :alt: Black code style


:License: MIT


Django + K8s Instructions
-------------------------


Docker compose
^^^^^^^^^^^^^^


For local `127.0.0.1:80`::

    $ pocker-compose -f prod_local.yml up


For production with `domain` https validation:: 

    $ docker-compose -f production.yml up


Kubernetes
^^^^^^^^^^

Start minikube::

    $ minikube start --memory 8192 --cpus 4


Enable ingress internally:: 

    $ minikube addons enable ingress


Start django stack locally on minikube::

    $ kubectl apply  -f django_k8s_local.yml


Kompose
^^^^^^

Just for demostration::

    $ kompose convert -f prod_local.yml -o django_for_k8s.yml


GCP 
^^^

Create the IP::

    $  gcloud compute addresses create ingress-prod --global
    

Create the namespace (optional)::
    
    $ kubectl create namespace k8s     


Create django kubernetes deployments::

    $ kubectl apply -f django_k8s.yml --namespace k8s
    

Create certificates for domain (please edit the current domain)::

    $ kubectl apply -f django_k8s_certificate.yml --namespace k8s


Verify the creation of the resources::

    $ kubectl get pods --namespace k8s

    $ kubectl get services --namespace k8s

    $ kubectl get ingress --namespace k8s


Check the status of the certificate. You should wait some secs until it provision the ssl correctly ::

    $ kubectl describe managedcertificates --namespace k8


Settings
--------

Moved to settings_.

.. _settings: http://cookiecutter-django.readthedocs.io/en/latest/settings.html


Basic Commands
--------------

Setting Up Your Users
^^^^^^^^^^^^^^^^^^^^^

* To create a **normal user account**, just go to Sign Up and fill out the form. Once you submit it, you'll see a "Verify Your E-mail Address" page. Go to your console to see a simulated email verification message. Copy the link into your browser. Now the user's email should be verified and ready to go.

* To create an **superuser account**, use this command::

    $ python manage.py createsuperuser

For convenience, you can keep your normal user logged in on Chrome and your superuser logged in on Firefox (or similar), so that you can see how the site behaves for both kinds of users.

Type checks
^^^^^^^^^^^

Running type checks with mypy:

::

  $ mypy django_k8s

Test coverage
^^^^^^^^^^^^^

To run the tests, check your test coverage, and generate an HTML coverage report::

    $ coverage run -m pytest
    $ coverage html
    $ open htmlcov/index.html

Running tests with py.test
~~~~~~~~~~~~~~~~~~~~~~~~~~

::

  $ pytest

Live reloading and Sass CSS compilation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Moved to `Live reloading and SASS compilation`_.

.. _`Live reloading and SASS compilation`: http://cookiecutter-django.readthedocs.io/en/latest/live-reloading-and-sass-compilation.html



Celery
^^^^^^

This app comes with Celery.

To run a celery worker:

.. code-block:: bash

    cd django_k8s
    celery -A config.celery_app worker -l info

Please note: For Celery's import magic to work, it is important *where* the celery commands are run. If you are in the same folder with *manage.py*, you should be right.




Email Server
^^^^^^^^^^^^

In development, it is often nice to be able to see emails that are being sent from your application. For that reason local SMTP server `MailHog`_ with a web interface is available as docker container.

Container mailhog will start automatically when you will run all docker containers.
Please check `cookiecutter-django Docker documentation`_ for more details how to start all containers.

With MailHog running, to view messages that are sent by your application, open your browser and go to ``http://127.0.0.1:8025``

.. _mailhog: https://github.com/mailhog/MailHog



Deployment
----------

The following details how to deploy this application.



Docker
^^^^^^

See detailed `cookiecutter-django Docker documentation`_.

.. _`cookiecutter-django Docker documentation`: http://cookiecutter-django.readthedocs.io/en/latest/deployment-with-docker.html



Custom Bootstrap Compilation
^^^^^^

The generated CSS is set up with automatic Bootstrap recompilation with variables of your choice.
Bootstrap v4 is installed using npm and customised by tweaking your variables in ``static/sass/custom_bootstrap_vars``.

You can find a list of available variables `in the bootstrap source`_, or get explanations on them in the `Bootstrap docs`_.


Bootstrap's javascript as well as its dependencies is concatenated into a single file: ``static/js/vendors.js``.


.. _in the bootstrap source: https://github.com/twbs/bootstrap/blob/v4-dev/scss/_variables.scss
.. _Bootstrap docs: https://getbootstrap.com/docs/4.1/getting-started/theming/


