# content-gcpro-devops-engineer
Joseph Lowery, Matt Ulasien, Mattias Andersson

Triggering a CI/CD Pipeline with Google Cloud Build
Introduction
In this hands-on lab, we'll combine Google's Cloud Source Repositories, Cloud Run, and Cloud Build services to create and test a CI/CD pipeline. We are tasked with creating a CI/CD pipeline to automate deployment whenever a change is committed to the source code repo. We will host the repo on Cloud Source Repository and put the app into production with the serverless Cloud Run service. To automate the pipeline, we'll need to integrate Cloud Run with Cloud Build and establish a commit-based trigger for Cloud Build. Once the pipeline is complete, we'll perform several tests to confirm the automation is working properly.

Solution
To get started, log in to the Google Cloud Platform using the provided credentials.

Enable APIs
In the Google Cloud Platform, we need to enable the APIs by completing the following:

Click the main navigation icon (the three horizontal lines at the top left of the screen), and select Support, then Library.

Search Source then select Cloud Source Repositories API.

Select Enable.

With the API enabled, go back to the API Library by selecting the main navigation icon, and select Support, then Library.

Search build and select Cloud Build API.

Select Enable.

Go back to the library and search run, and select Cloud Run API.

Click Enable.

Create the Source Repository
With our APIs enabled, we can begin to create the cloud shell. To do so, open the cloud shell using the >_ button at the top of the screen and then complete the following:

Click Activate Cloud Shell then Continue.

Once in the shell, enter:

gcloud source repos create automate-pipeline
Clone the repo with the following command:

gcloud source repos clone automate-pipeline
Change the directory to the automate-pipeline repo:

cd automate-pipeline
Pull the app files from the GitHub repo into our repo:

git pull https://github.com/linuxacademy/content-gcpro-devops-engineer
Set up the necessary global configuration variables to use the git command:

git config --global user.name "YOUR NAME"
git config --global user.email "YOUR EMAIL"
Push the cloned repo into the recently created Cloud Source Repository:

git push origin master
Confirm the Cloud Source Repository has been created with the desired files by going to the Google Cloud Platform, select the main navigation menu, scroll to the Tools section, and select Source Repositories.

Select the automate-pipeline repository and view the content.

Under the Files section, select the labs folder, and drill down to triggering-cicd-pipeline, source-build, and templates.

Integrate Cloud Run into Cloud Build
Back in the Cloud Shell, we need to define our PROJECT_ID variable:

Run the following command to define the variable:

export PROJECT_ID=$(gcloud info --format='value(config.project)')
Enable the Cloud Run service account for Cloud Build by going to the main navigation menu, then under Tools select Cloud Build then Settings.

Next to Cloud Run, change Disabled to Enabled and select GRANT ACCESS TO ALL SERVICE ACCOUNTS.

Establish a Cloud Build Trigger
In the Cloud Build navigation, select Triggers and complete the following:

Select Create Trigger.

For Event choose, Push to a branch.

Set Source to the pipeline we created.

Under Build configuration, enter the path /labs/triggering-cicd-pipeline/source-build/cloudbuild.yaml.

Click Create.

Test Your Pipeline
Run the following to test the trigger:

To the right of our trigger, select Run trigger.

Click on the current build and make sure that all of the steps appear as green checkmarks.

Once the steps are confirmed, open the main navigation, and under COMPUTE select, Cloud Run.

Select cloud-run-deploy.Make a change to the source code and push it to the repository.

Click on the URL at the top of the page.

Edit the Pipeline
Let's make a few edits to our code to make sure that the URL animation is centered and repeats 3 times:

On the Cloud Run page, under the Editor section, drill down into the file folders: automate-pipeline > labs > triggering-cicd-pipeline > source-build > templates.

Select index.html.

Change the #main section to read as follows:

#main {margin: 1em auto; width: max-content}
To make the URl run the animation 3 times, change the URL to: "https://github.com/linuxacademy/content-gcpro-devops-engineer/blob/master/labs/triggering-cicd-pipeline/assets/AutomateAllTheThings-GIF-3.gif?raw=true".

Click Save.

Back in our Cloud Shell, enter:

git add .
git commit -m "update index.html"
git push origin master
Go back to the Cloud Build page by Cloud going to the main navigation menu, then under Tools select Cloud Build then Settings.

Select the new build and wait until all the steps are shown as green checkmarks.

Refresh the page with the URL we used earlier and see how our animation has updated.

Conclusion
