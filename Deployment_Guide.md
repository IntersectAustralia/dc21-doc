# Deployment Guide

Below are links to a set of pages describing various aspects of deployment. Please read the following information before proceeding.

## Important Info About Deployment
As of version 2.0, the approach to DC21 deployment is to run a series of commands on the target production server to update the server locally. This removes the need for a _deployment_ machine.

The _server_ machine is the production server where the web application will run.

### Usernames and passwords
As you proceed through the instructions you will create and set passwords for a number of accounts (both Linux and Postgres). Make sure you keep track the users you have created and their passwords.

## First Time Build
Before installing, you should read the [release notes](README.md#version-documentation) for the version you are installing.
* [Deployment Guide : First Time Server Build](Deployment_Guide_-_First_Time_Server_Build.md) - instructions for building a new instance of the DC21 web application.

## Updating
Before installing, you should read the [release notes](README.md#version-documentation) for the version you are installing.
* [Deployment Guide : Deploying A New Version](Deployment_Guide_-_Deploying_A_New_Version.md) - instructions for updating an installed instance with a new version of the web application.

## Smoke Tests
* [Smoke Tests](Smoke_tests.md) - a minimum set of test that we recommend be run after installation and/or updates to check that all parts of the infrastructure are functioning correctly.

## Miscellaneous
* [Deploying To Multiple Environments](Deploying_to_multiple_environments.md) - some tips on how Capistrano supports running multiple environments (e.g. test and production).
