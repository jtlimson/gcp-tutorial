# Start a google App Engine
On the Google Cloud Platform menu, click Activate Cloud Shell (). If a dialog box appears, click Start Cloud Shell.

Clone the source code repository for a sample application called guestbook:
git clone https://github.com/GoogleCloudPlatform/appengine-guestbook-python

Navigate to the source directory:

`cd appengine-guestbook-python`

List the contents of the directory:

`ls -l`

View the app.yaml file and note its structure:

`cat app.yaml`

YAML is a templating language. YAML files are used for configuration of many Google Cloud Platform services, although the valid objects and specific properties vary with the service. This file is an App Engine YAML file with handlers: and libraries:. A Cloud Deployment Manager YAML file, for example, would have different objects.

Run the application using the built-in App Engine development server.

`dev_appserver.py ./app.yaml`

The App Engine development server is now running the guestbook application in the local Cloud Shell. It is using other development tools, including a local simulation of Datastore.

In Cloud Shell, click `Web preview > Preview on port 8080` to preview the application.
To access the Web Preview icon, you may need to collapse the Navigation Menu pane.




Try the application. Make a few entries in Guestbook, and click Sign Guestbook after each entry.
```
Using the Google Cloud Platform Console, verify that the app is not deployed. In the GCP Console, on the Navigation Menu () menu, click App Engine > Dashboard. Notice that no resources are deployed. The App Engine development environment is local.
To end the test, return to Cloud Shell and press Ctrl+C to abort the App Engine development server.
```
Deploy the Guestbook application to App Engine

Ensure that you are at the Cloud Shell command prompt.

Deploy the application to App Engine using this command:

gcloud app deploy ./index.yaml ./app.yaml

If prompted for a region, enter the number corresponding to the region that Qwiklabs or your instructor assigned you to. Type Y to continue.

To view the startup of the application, in the GCP Console, on the Navigation Menu () menu, click App Engine > Dashboard.

You may see messages about "creating your first app". Keep refreshing the page periodically until the application is deployed.

View the application on the Internet. The URL for your application is:
https://PROJECT_ID.appspot.com/

where PROJECT_ID represents your Google Cloud Platform project name. This URL is listed in two places:

The output of the deploy command: Deployed service [default] to `[https://PROJECT_ID.appspot.com]`

The upper-right pane of the App Engine Dashboard

Copy and paste the URL into a new browser window.

You may see an INTERNAL SERVER ERROR. If you read to the bottom of the page, you will see that the error is caused because the Datastore Index is not yet ready. This is a transient error. It takes some time for Datastore to prepare and begin serving the Index for guestbook. After a few minutes, you will be able to refresh the page and see the guestbook application interface.

Result:



Congratulations! You created your first application using App Engine, including exercising the local development environment and deploying it. It is now available on the internet for all users.
