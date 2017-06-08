Self-Hosted Instance
=====================

To host a standalone ILP instance for your organization:

1. Get the source code from https://github.com/klpdotorg/dubdubdub.
2. Get all the DBs (TBD - list or link to source) that need to exist simultaneously.

    a. Fetch DB. (how? Any external links to postgres documentation?)
    b. Restore DB. (how?)
    c. Create corresponding Materialized Views.

3. Create an administrator username and password using the following command: ::

    python manage.py createsuperuser

.. note::
 
   For administration, only a **superuser** role can be created. All other user roles are used to classify the users contributing data to the platform.

.. todo::

    How to create CNAME or domain URL or URL redirect to this server?

    Any recommended nomenclature for the URL?
