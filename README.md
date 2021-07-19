# Snyk Starter Open Source Workshop

Snyk is an open source security platform designed to help software-driven businesses enhance developer security. Snyk's dependency scanner makes it the only solution that seamlessly and proactively finds, prioritizes and fixes vulnerabilities and license violations in open source dependencies

Snyk's GitHub integration allows developers to easily onboard their GitHub repositories to scan and continuously monitor them for open source security and license risks. This integration also enables Snyk's Automated Fix Pull Requests and adds Snyk checks on every Pull Request.

In this **hands-on** demo we will achieve the follow

* Step 1 - Fork the highly vulnerable Goof Application
* Step 2 - Configure GitHub Integration
* Step 3 - Find vulnerabilities
* Step 4 - Fix using a Pull Request
* Step 5 - Test using the CLI (test and monitor)
* Step 6 - Failing using Exit Codes
* Step 7 - Viewing Dashboard Reports
* Step 8 - IDE integration VS Code

## Prerequisites

* public GitHub account - http://github.com
* git client - 
* snyk CLI - https://support.snyk.io/hc/en-us/articles/360003812538-Install-the-Snyk-CLI
* Regisitered account on Snyk App - http://app.snyk.io

# Workshop Steps

_Note: It is assumed your using a mac for these steps but it should also work on windows or linux with some modifications to the scripts potentially_

## Step 1 Fork the highly vulnerable Goof Application

Navigate to the following GitHub repo - https://github.com/snyk/goof

* Click on the "**Fork**" button
* Ensure you are forking this repo to your public GitHub account 
* Click done

![alt tag](https://i.ibb.co/bPqqybM/snyk-starter-open-source-2.png)

## Step 2 - Configure GitHub Integration

First we need to connect Snyk to GitHub so we can import our Repository. Do so by:

* Login to http://app.snyk.io Sign up if you haven't already.
* Navigating to Integrations -> Source Control -> GitHub
* Fill in your Account Credentials to Connect your GitHub Account.

![alt tag](https://i.ibb.co/bPqqybM/snyk-starter-open-source-1.png)

## Step 3 - Find vulnerabilities

Now that Snyk is connected to your GitHub Account, import the Repo into Snyk as a Project.

* Navigate to Projects
* Click "**Add Project**" then select "**GitHub**"
* Click on the Repo you forked.

![alt tag](https://i.ibb.co/q9Rsxsh/snyk-starter-open-source-3.png)

## Step 4 - Fix using a Pull Request

First let's explore the Goof project risks by clicking on the "package.json" file which is the manifest file for the open source dependencies used in the code



## Step 5 - Test using the CLI (test and monitor)


## Step 6 - Failing using Exit Codes


## Step 7 - Viewing Dashboard Reports


## Step 8 - IDE integration VS Code


Thanks for attending and completing this workshop

![alt tag](https://i.ibb.co/7tnp1B6/snyk-logo.png)

<hr />
Pas Apicella [pas at snyk.io] is an Solution Engineer at Snyk APJ
