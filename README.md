# wikijs-openshift

After following this guide, should be able to deploy a wiki.js site on openshift. It should be routed to a openshift-generated host.

## Requirements
Openshift CLI
Docker

## Step 1: Openshift - MongoDB
Deploy MongoDB on Openshift Web Console (in catalog). Set connection username and password (Recommend set yourself). ***Database name: wiki***

Go to Applications/Services and check the Cluster IP of Mongodb. (Need to use in config.yml)

## Step 2: Docker - Dockerfile
Use the dockerfile in /docker directory.

Replace the email address you want for admin account of the wiki site.

## Step 3: Docker - your-config.yml
This file is used to configure the wiki.

Things need to edit includes:
* title
* host (only if the route is known, or else just leave it and follow the steps)
* db (use the cluser IP)
* git connection (I think can be private repo, not tested)
** If auth type is ssh, need to add the privatekey file using dockerfile

## Step 4: Create Docker Image
Open a docker hub account

In terminal, go to /docker and run:
* docker build ./ (Build the docker image)
* docker images (Check the image)
** Lookup the image you just built and note the image id
* docker tag [INSERT-IMAGE-ID] yourdockerhubaccountname/reponame:repotag
** repotag is for different version of the repo, important (recommand: 'intial' or 'first')
* docker push yourdockerhubaccountname/reponame:repotag

Now the image should be on docker hub

## Step 5: New app in Openshift
In terminal, assume you have logged in and already in the project, run
* oc new-app yourdockerhubaccountname/reponame:repotag
* oc expose svc/wikijs-test

Go to Openshift Web Console and check the routed hostname, copy it to your-config.yml host. No need to add port.

After that run step 4 again, with different repotag. Then go to step 6.

## Step 6: Re-deploy the wiki
(I can't find a legit way to push image to ImageStream. The guide cant help so need to re-deploy in this way)
Delete wiki deployment, either on web console or using openshift cli.
In terminal, run again 
* oc new-app yourdockerhubaccountname/reponame:repotag (This time the updated one, so should be different tag)
 
 Wait for it to deploy and should be ready after deployment
Use the admin email address in dockerfile as user and admin123 as password to login.




