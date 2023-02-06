# break-the-analysis branch

## Overview
The Jenkins plugin has options to fail a build when (1) an analysis fails and/or (2) a policy is violated. The source code in this repo is a small subset of Webgoat where the results only have Info, Low, and Medium results. 

## Set up
1. In Code Dx, 
- create a new project 
- create policy: filter = "only critical and high severity"; fix by = "Not required"; and action = "Break build"; and assign to the new project 
- configure git: `https://github.com/tammyhasenzahl/test-git-branch/` with `break-the-analysis` branch
- create an API key with admin privileges
2. In Jenkins, 
- create a Freestyle or Pipeline project (Freestyle is easier/quicker)
- configure (left side)
- Source Code Management section: click "git"; URL: `https://github.com/tammyhasenzahl/test-git-branch/`; and Branch Specifier: `*/break-the-analysis`
- Post-build Actions section: Publish to Code Dx. Requires: Code Dx URL, admin API key, and Code Dx project. Use the defaults.  Save.

## Step 1: build passes (analysis is successful and the policy is not violated)
In Jenkins, 
- Build Now
- Click the new job 
- Console output (left side): successful analysis and build passes
- return to project: successful build


## Step 2: build fails 
In Jenkins, 
- Tool Output: `Test-break-the-analysis.xml`
- Save
- Build Now
- Console output: analysis is unsucessful and build fails
- return to project: build fails (red)


## Step 3: policy is violated
In Jenkins,
- Tool Output: (a) remove `Test-break-the-analysis.xml` and (b) add  `Policies-test-critical-severity.xml`
- Save
- Build Now
- Console output: 

