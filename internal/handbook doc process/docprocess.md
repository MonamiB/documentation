# Documentation Process

In our case, the instructions are for working on the klpdotorg/documentation/handbook repository.

## Update

**Important:** Ensure that you create a branch and maintain all changes in it till the peer review is done. Merging the changes to master automatically triggers an RTD build and content is pushed to production, so it is important to commit only validated content.

1. Clone the git repo on your desktop. This is a one-time only process. Henceforth, you will need to checkout the latest version of the documentation in a new branch before you edit the content.
2. Navigate to handbook/source folder.
3. Open the relevant `.rst` file using a text editor and update as required.
4. If you have created a new file, using .rst extension is recommended, though RTD recognizes `.txt` and `.md` extensions as well. 
5. Update the `index.rst` file to reflect the new topic in the correct hierarchy.

## Preview

1. Create test build. 

    path to git folder\handbook> make clean
    path to git folder\handbook> make html

2. Navigate to the build folder and preview the `index.htm` on your browser.
3. Verify the content, format, cross-references, etc.
4. Commit the branch and notify the reviewer.

**Pro Tip:**

Ensure that build static and templates directories are listed in `.gitignore`.

## Review

Once the content has been through a round of peer review, merge the branch to master. This will automatically trigger an RTD build and the changes will be pushed to production.

## Publishing on RTD

The documentation is published on RTD automatically when changes are committed to master.

### Optional: RTD Setup for a New Project

1.	Login to RTD using the organization credentials.
2.	On the dashboard, click **Import**.
3.	Provide the URL to the git doc repo in the **Repo** field.
4.	Click **Create**.

This will publish the documentation in the absense of any errors in the `conf.py` or `Makefile` scripts.

To automate publishing each change committed to master in Github:

1.	In your documentation repository, go to **Settings > Webhooks & Services**.
2.	To enable RTD, select **ReadTheDocs** from the **Add Service dropdown**.

## Optional: Source Files Setup for a New Project

1. Create the source and build directories with all the relevant scripts and the configuration file. This is a one-time process.
    
    sphinx-quickstart

2. Follow the instructions onscreen and provide the requisite data.
