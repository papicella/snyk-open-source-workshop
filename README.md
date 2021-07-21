# Introduction to Snyk Open Source Workshop

Snyk is an open source security platform designed to help software-driven businesses enhance developer security. Snyk's dependency scanner makes it the only solution that seamlessly and proactively finds, prioritizes and fixes vulnerabilities and license violations in open source dependencies

Snyk's GitHub integration allows developers to easily onboard their GitHub repositories to scan and continuously monitor them for open source security and license risks. This integration also enables Snyk's Automated Fix Pull Requests and adds Snyk checks on every Pull Request.

In this **hands-on** demo we will achieve the follow

* [Step 1 Fork the highly vulnerable Goof Application](#step-1-fork-the-highly-vulnerable-goof-application)
* [Step 2 Configure GitHub Integration](#step-2-configure-gitHub-integration)
* [Step 3 Find vulnerabilities](#step-3-find-vulnerabilities)
* [Step 4 Fix using a Pull Request](#step-4-fix-using-a-pull-request)
* [Step 5 Test using the CLI](#step-5-test-using-the-cli)
* [Step 6 Failing using Exit Codes](#step-6-failing-using-exit-codes)
* [Step 7 Viewing Dashboard Reports](#step-7-viewing-dashboard-reports)
* [Step 8 IDE integration VS Code](#step-8-ide-integration-vs-code)

## Prerequisites

* public GitHub account - http://github.com
* git CLI - https://git-scm.com/downloads
* snyk CLI - https://support.snyk.io/hc/en-us/articles/360003812538-Install-the-Snyk-CLI
* Registered account on Snyk App - http://app.snyk.io

# Workshop Steps

_Note: It is assumed your using a mac for these steps but it should also work on windows or linux with some modifications to the scripts potentially_

## Step 1 Fork the highly vulnerable Goof Application

Navigate to the following GitHub repo - https://github.com/snyk/goof

* Click on the "**Fork**" button
* Ensure you are forking this repo to your public GitHub account 
* Click done

![alt tag](https://i.ibb.co/bPqqybM/snyk-starter-open-source-2.png)

## Step 2 Configure GitHub Integration

First we need to connect Snyk to GitHub so we can import our Repository. Do so by:

* Login to http://app.snyk.io Sign up if you haven't already.
* Navigating to Integrations -> Source Control -> GitHub
* Fill in your Account Credentials to Connect your GitHub Account.

![alt tag](https://i.ibb.co/bPqqybM/snyk-starter-open-source-1.png)

## Step 3 Find vulnerabilities

Now that Snyk is connected to your GitHub Account, import the Repo into Snyk as a Project.

* Navigate to Projects
* Click "**Add Project**" then select "**GitHub**"
* Click on the Repo you forked.

![alt tag](https://i.ibb.co/q9Rsxsh/snyk-starter-open-source-3.png)

_Note: The import can take up to one minute so you can view the import log while it's running as shown below_

![alt tag](https://i.ibb.co/RQsX6jZ/snyk-starter-open-source-14.png)

## Step 4 Fix using a Pull Request

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

## Step 5 Test using the CLI

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
* We can instruct the Snyk App to actually monitor our code in the UI as shown below, so run "**snyk monitor**" to achieve that. 

```bash
$ snyk monitor

Monitoring /Users/pasapicella/snyk/SE/workshops/snyk-starter-open-source/goof (goof)...

Explore this snapshot at https://app.snyk.io/org/workshops-admin-org/project/7709e818-d3a2-41ee-9aec-5ddc35321b50/history/204008e7-d79f-4215-8c48-544ea8bee921

Notifications about newly disclosed issues related to these dependencies will be emailed to you.
```

* Returning to the Snyk App UI will show our CLI "snyk monitor" result BUT this time we didn't use the GitHub integration

![alt tag](https://i.ibb.co/RzNHq8F/snyk-starter-open-source-13.png)

## Step 6 Failing using Exit Codes

On typical Unix and Linux systems, programs would be able to pass a value to their parent processes while terminating. Values like these are referred to as Exit codes
As part of Snyk output you must have seen Snyk process terminating with exit codes, we typically see

1. Exit code 0 This means Snyk did not find vulnerabilities in your code an exited the process without failing the job.
1. Exit code 1 This means Snyk found vulnerabilities in your code and have failed the build
1. Exit code 2 This means Snyk exited with an error, please re-run with `-d` to see further information.
1. Exit code 3 This means Snyk did not detect any supported projects/manifests to scan. Re-check the command or if the command should run in a different directory.

* Create a script called "**goof-break-build-for-CRITICAL.sh**" as follows 

```bash
#!/bin/bash

snyk test ./goof --severity-threshold=critical

exit_code=$?

echo ""
echo "snyk cli test exit code equals = $exit_code "
echo ""

if [ $exit_code -eq 1 ]
then
  echo "*****"
  echo "Build step has to fail we found at least 1 CRITICAL issue with the goof application .. "
  echo "****"
fi
```

* Make the script executable as shown below
  
```bash
$ chmod +x goof-break-build-for-CRITICAL.sh
```

* Run it from one directory level back from "**goof**" directory source code as shown below. You will see that from the exit code we have identified at least 1 critical issue exists and so we must fail the build

```
pasapicella@192-168-1-113:~/snyk/SE/workshops/snyk-starter-open-source$ d
total 36168
drwxr-xr-x  24 pasapicella  staff       768 19 Jul 15:33 goof/
-rwxr-xr-x   1 pasapicella  staff       302 21 Jul 11:52 goof-break-build-for-CRITICAL.sh*

$ ./goof-break-build-for-CRITICAL.sh

Testing ./goof...

Tested 553 dependencies for known issues, found 4 issues, 13 vulnerable paths.

.....

snyk cli test exit code equals = 1

*****
Build step has to fail we found at least 1 CRITICAL issue with the goof application ..
****
```

[Failing with Exit Codes](https://support.snyk.io/hc/en-us/articles/360006786558-Failing-with-exit-code-Generic-Error)

## Step 7 Viewing Dashboard Reports

The Reports area offers data and analytics across all of your projects, displaying historical and aggregated data about projects, issues, dependencies, and licenses. Data in each of the four tabs (seen below) is displayed based on the organization in which you are working, and you can filter this data with different parameters depending on the tab you're viewing

* Click on the "Reports" link at the top of the Snyk App UI page's toolbar. You may need to wait briefly while the report page displays

![alt tag](https://i.ibb.co/wNs3JC1/snyk-starter-open-source-11.png)

## Step 8 IDE integration VS Code

Optionally if you have time, and you have VS Code installed you can install a plugin to allow you to scan your "**goof**" code within VS code while in an IDE

* Install it using the following link - [Install VS Code Snyk Plugin](https://marketplace.visualstudio.com/items?itemName=snyk-security.vscode-vuln-cost)

![alt tag](https://i.ibb.co/zHZ3qxv/snyk-starter-open-source-12.png)


Thanks for attending and completing this workshop

![alt tag](https://i.ibb.co/7tnp1B6/snyk-logo.png)

<hr />
Pas Apicella [pas at snyk.io] is an Solution Engineer at Snyk APJ
