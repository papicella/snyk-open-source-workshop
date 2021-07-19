# Introduction to Snyk Open Source Workshop

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

First let's explore the Goof project risks by clicking on the "**package.json**" file which is the manifest file for the open source dependencies are declared. 

* Click on "**package.json**"

![alt tag](https://i.ibb.co/THs4g5q/snyk-starter-open-source-4.png)

For each Vulnerability, Snyk displays the following ordered by our [Proprietary Priority Score](https://snyk.io/blog/snyk-priority-score/) :
1. The module that introduced it and, in the case of transitive dependencies, its direct dependency,
1. Details on the path and proposed Remediation, as well as the specific vulnerable functions
1. Overview
1. Exploit maturity
1. Links to CWE, CVE and CVSS Score
1. Plus more ...

![alt tag](https://i.ibb.co/LYm0fVf/snyk-starter-open-source-5.png)

When using the GitHub integration, and if a fix is available, Snyk can automatically upgrade the vulnerable dependency to a non-vulnerable version through a Pull Request. 

* Click on "Fix this vulnerability" for "**typeorm Prototype Pollution**" issue a shown below

![alt tag](https://i.ibb.co/sRJjfnC/snyk-starter-open-source-6.png)

* On the next screen, you'll be able to confirm the issue to fix with this PR. Click "Open a Fix PR"

![alt tag](https://i.ibb.co/w7m8HPv/snyk-starter-open-source-7.png)
![alt tag](https://i.ibb.co/J7MmSQc/snyk-starter-open-source-8.png)

* Once it's ready, you'll be taken to the PR in GitHub, where you can review the changes in the file diff view:

![alt tag](https://i.ibb.co/P6bcxkh/snyk-starter-open-source-9.png)

* We see that CI checks completed successfully, assuring us we didn't introduce a breaking change

![alt tag](https://i.ibb.co/SPMCMpP/snyk-starter-open-source-10.png)

* Optionally Now, go ahead and merge the PR!
* Back in Snyk, we can appreciate that our package.json file has 1 less High Severity Vulnerability if you did fix it

## Step 5 - Test using the CLI (test and monitor)

In addition to the Snyk App UI we also have, snyk - CLI and build-time tool to find & fix known vulnerabilities in open-source dependencies. The CLI is what is used in DevOps pipelines to introduce Application Security Scans as part of that workflow to push applications into production. 

* Authorize the snyk CLI with your account as follows

```bash
$ snyk auth

Now redirecting you to our auth page, go ahead and log in,
and once the auth is complete, return to this prompt and you'll
be ready to start using snyk.

If you can't wait use this url:
https://snyk.io/login?token=ff75a099-4a9f-4b3d-b75c-bf9847672e9c&utm_medium=cli&utm_source=cli&utm_campaign=cli&os=darwin&docker=false


Your account has been authenticated. Snyk is now ready to be used.
```

* Clone your forked repository as shown below. You would use your own GitHub repo here instead of the one shown below

```bash
$ git clone https://github.com/papicella/goof
Cloning into 'goof'...
remote: Enumerating objects: 2056, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 2056 (delta 0), reused 2 (delta 0), pack-reused 2053
Receiving objects: 100% (2056/2056), 3.89 MiB | 9.16 MiB/s, done.
Resolving deltas: 100% (1417/1417), done.
```

* Change to the "goof" directory

```bash
$ cd goof
```

* To have better control over your tests, you can pass the severity-threshold flag to the snyk test command with one of the supported options (low|medium|high|critical). With this flag, only vulnerabilities of provided level or higher will be reported. Let's set that to "**critical**" and run a test as shown below.

```bash
$ snyk test --severity-threshold=critical

Testing /Users/pasapicella/snyk/SE/workshops/snyk-starter-open-source/goof...

Tested 553 dependencies for known issues, found 4 issues, 13 vulnerable paths.

Issues to fix by upgrading:

  Upgrade adm-zip@0.4.7 to adm-zip@0.4.11 to fix
  ✗ Arbitrary File Write via Archive Extraction (Zip Slip) [Critical Severity][https://snyk.io/vuln/npm:adm-zip:20180415] in adm-zip@0.4.7
    introduced by adm-zip@0.4.7

  Upgrade lodash@4.17.4 to lodash@4.17.20 to fix
  ✗ Prototype Pollution [Critical Severity][https://snyk.io/vuln/SNYK-JS-LODASH-590103] in lodash@4.17.4
    introduced by lodash@4.17.4 and 9 other path(s)

  Upgrade mongoose@4.2.4 to mongoose@4.2.5 to fix
  ✗ DLL Injection [Critical Severity][https://snyk.io/vuln/SNYK-JS-KERBEROS-568900] in kerberos@0.0.24
    introduced by mongoose@4.2.4 > mongodb@2.0.46 > mongodb-core@1.2.19 > kerberos@0.0.24


Issues with no direct upgrade or patch:
  ✗ Prototype Pollution [Critical Severity][https://snyk.io/vuln/SNYK-JS-HANDLEBARS-534988] in handlebars@4.0.11
    introduced by tap@11.1.5 > nyc@11.9.0 > istanbul-reports@1.4.0 > handlebars@4.0.11
  This issue was fixed in versions: 4.5.3, 3.0.8


Organization:      pas.apicella-41p
Package manager:   npm
Target file:       package-lock.json
Project name:      goof
Open source:       no
Project path:      /Users/pasapicella/snyk/SE/workshops/snyk-starter-open-source/goof
Licenses:          enabled

Tip: Run `snyk wizard` to address these issues.
```

## Step 6 - Failing using Exit Codes


## Step 7 - Viewing Dashboard Reports


## Step 8 - IDE integration VS Code


Thanks for attending and completing this workshop

![alt tag](https://i.ibb.co/7tnp1B6/snyk-logo.png)

<hr />
Pas Apicella [pas at snyk.io] is an Solution Engineer at Snyk APJ
