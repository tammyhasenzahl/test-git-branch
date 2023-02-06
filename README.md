# break-the-analysis branch

## Overview
The Jenkins plugin has options to fail a build when (1) an analysis fails and/or (2) a policy is violated. Additionally, there's a new option to include Git Source that is being tested. The source code in this repo is a small subset of Webgoat where the results only have Info, Low, and Medium results. 

## Set up
1. In Code Dx, 
- create a new project 
- configure git for the new project: `https://github.com/tammyhasenzahl/test-git-branch/` with `break-the-analysis` branch
- create policy: filter = "only critical and high severity"; fix by = "Not required"; and action = "Break build"; and assign to the new project 
- create an API key with admin privileges

2. In Jenkins, 
- use the latest Jenkins plugin build (unreleased), which is an HPI file (see notes about installing an HPI, below)
- create a Freestyle or Pipeline project (Freestyle is easier/quicker)
- configure (left side)
- *Source Code Management section:* click "git"; URL: `https://github.com/tammyhasenzahl/test-git-branch/`; and Branch Specifier: `*/break-the-analysis`
- *Post-build Actions section:* Publish to Code Dx. Requires: Code Dx URL, admin API key, and Code Dx project. Use the defaults except (a) delete `**` from "Source and Binary Files" and (b) check "Wait for analysis result".  Save.

## Step 1: build passes because there's a successful analysis and no policy violation
In Jenkins, 
- Build Now
- Click the new job link
- Console output (left side): successful analysis and build passes
- Return to project: successful build


## Step 2: build fails because the analysis fails
In Jenkins, 
- Configure > Tool Output: `Test-break-the-analysis.xml`
- Save
- Build Now
- Console output: analysis fails and build fails
- Return to project: build fails (red)


## Step 3: build fails because there's a policy violation
In Jenkins,
- Configure > Tool Output: (a) remove `Test-break-the-analysis.xml` and (b) add  `Policies-test-critical-severity.xml`
- Save
- Build Now
- Console output: policy is violated and build fails
- Return to project: build fails (red)

## Installing an Unreleased Jenkins Plugin to Jenkins
Jenkins Dashboard > Manage Jenkins > Plugin Manager >
Advanced settings (left side) > Deploy Plugin > Choose File > browse to HPI being tested > click Deploy (wait for something about restarting Jenkins > restart Jenkins


