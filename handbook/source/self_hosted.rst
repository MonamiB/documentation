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

To get `http://olp.ind.in/map/` working, including the domain name registration and django web application deployment step, follow these steps.
* Populate and deploy the DISE data for the state before populating the KLP data.

1. Create a fresh `klpdise_olap` db.
2. Clone the `dise_dashboard` repo.
3. Run the migrations.
4. Checkout the `olp` branch.
5. Get all the DISE data from http://schoolreportcards.in/SRC-New/.
6. Run the import scripts:
 - `python manage.py import_basic --year 15-16 csvs/BasicData.csv`
- `python manage.py import_facilities --year 15-16 csvs/FacilityData.csv`
- `python manage.py import_general --year 15-16 csvs/GeneralData.csv`
- `python manage.py import_rte --year 15-16 csvs/RTEData.csv`
- `python manage.py import_teachercount --year 15-16 csvs/TeachersData.csv`
- `python manage.py import_totalenrolment --year 15-16 csvs/TotalEnrolmentData.csv`
- `./misc/update-aggregation.sh`
7. Deploy the app. (We need to point all `dise.*` endpoints on dubdubdub to this new URL).

* Setup dubdubdub

1. Clone the `dubdubdub` repo and checkout the `olp` branch.
2. Restore all of the following `dbs`:
``` klpdise_new
 libinfra
 library
 libreading
 pratham_mysore
 reading_report
 sikshana
 electrep_new
 klp-coord
 spatial
 ang_infra
 apmdm
 dbstatus
 dise_all
 klpsys
 dubdubdub
```
3. Create the materialized views. `psql -h localhost -U klp -d dubdubdub -f sql/materialized_views.sql`
4. Make sure `'mvw_dise_info_olap'` has data. This should have been populated in the `materialized_views.sql` script using the `klpdise_olap` db we populated when we were setting up DISE.
5. Might need to do `alter sequence boundary_id_seq restart with 11042` before next step of boundary creation. Just make sure the `sequence` value in the `boundary_id_seq` table is the next value after the id value of the latest boundary in the boundary table.
6. Since the `School` table has `managed=False`, we need to manually pass in the Primary key id. Fetch the id of the latest existing `School` in the db, add 1 to it, and use that value in the script mentioned in the next point. (Currently, script has `63929` as the starting school_id hardcoded. Because `School.objects.latest('id').id`)
7. Make sure to open up and run everything inside `schools/migrations/0023_auto_20170630_0642.py`.
8. Import CSV sheet received from Odisha using: https://github.com/klpdotorg/dubdubdub/blob/olp/apps/schools/management/commands/populateOLP.py
9. The script also spits out another .sql file which has the sql command that needs to be run for each school in order to get their lat, long in the db.
10. That sql file needs to be run on the klp-coord db on the inst_coord table.
 - `psql klp-coord < logs/school_sql_2017-06-20-1497931881.sql`
11. We need to refresh materialized views after this exercise is done.
12. Then we have to run a `python manage.py calculate_boundary_centroids` script which spits out another .sql file that we need to run to get all the boundary borders in the db.
13. That .sql file needs to be run on the klp-coord db on the boundary_coord table.
 - `psql klp-coord < logs/boundaries_sql_2017-06-20-1497941378.sql`
14. Refresh materialized views after that.
15. We have the new schools and their information (including DISE) in the db now.
16. Deploy dubdubdub.

NOTE: Depending on the urls that you choose to host both the `dise_dashboard` app as well as `dubdubdub` app above, please refer to the PR `https://github.com/klpdotorg/dubdubdub/pull/701` and see where all you’ll have to make changes to get this to work properly.
