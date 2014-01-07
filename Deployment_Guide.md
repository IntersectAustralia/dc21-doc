# Deployment Guide

Below are links to a set of pages describing various aspects of deployment. Please read the following information before proceeding.

## Important Info About Deployment
The approach to DC21 deployment is to run a series of commands on a local or 'deployment' machine that deploy the system to the target production server. This is common practice in Rails deployment, as it helps make deployments safe and repeatable.

In all the below instructions, we refer to two machines:
### deploy
The _deploy_ machine is a machine where you're running commands to deploy the application, this could be
* your desktop machine
* a VM running on your desktop machine
* a VM running in the cloud (e.g. Nectar)
* a VM running at your institution

If you want to follow our instructions precisely, you will need a deployment machine that:
* is a clean el6 server with the standard "Server" packages.
* has EPEL yum repository is installed.

Using a VM in the cloud or at your institution is recommended, as this can be kept for future redeployments and will save you (and your colleagues) from having to set it up again to do future deployments.


### server
The _server_ machine is the production server where the web application will run.

### Usernames and passwords
As you proceed through the instructions you will create and set passwords for a number of accounts (both Linux and Postgres). Make sure you keep track the users you have created and their passwords.

## First Time Build
You should read the [release notes](README.md#version-documentation) before installing.
* [Deployment Guide : First Time Server Build](Deployment_Guide_-_First_Time_Server_Build.md) - instructions for building a new instance of the DC21 web application.
* [Setting Up OAI](Setting_Up_OAI.md) - following on from above, instructions for setting up the jOAI component to provide the OAI-PMH feed of published collections.
* [Setting Up SSL](Setting_Up_SSL.md) - instructions for configuring your instance to run over https - optional but recommended.
* [Setting up Redis and Resque](Setting_up_redis_and_resque.md) - instructions for setting up and configuring the background workers.

## Updating
You should read the [release notes](README.md#version-documentation) before installing.
* [Deployment Guide : Deploying A New Version](Deployment_Guide_-_Deploying_A_New_Version.md) - instructions for updating an installed instance with a new version of the web application.

## Smoke Tests
* [Smoke Tests](Smoke_tests.md) - a minimum set of test that we recommend be run after installation and/or updates to check that all parts of the infrastructure are functioning correctly.

## Miscellaneous
* [Deploying To Multiple Environments](Deploying_to_multiple_environments.md) - some tips on how Capistrano supports running multiple environments (e.g. test and production).
