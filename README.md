# break-the-analysis branch

## Overview
The Jenkins plugin has options to fail a build when (1) an analysis fails and/or (2) a policy is violated. Additionally, there's a new option to include Git Source that is being tested. Specifically,  Git Source, Error Handling, and Policy Behavior are tested. Note: the source code in this repo is a small subset of Webgoat where the results only have Info, Low, and Medium results. 

## Set up
1. In SRM, 
- create a new project 
- configure git for the new project: `https://github.com/tammyhasenzahl/test-git-branch/` with `break-the-analysis` branch
- create policy: filter = "only critical and high severity"; fix by = "Not required"; and action = "Break build"; and assign to the new project 
- create an API key with admin privileges

2. In Jenkins, 
- use the latest Jenkins plugin build (unreleased), which is an HPI file (see notes about installing an HPI, below)
- create a Freestyle or Pipeline project (Freestyle is easier/quicker)
- click *Configure* (left side)
- *Source Code Management section:* click "git"; URL: `https://github.com/tammyhasenzahl/test-git-branch/`; and Branch Specifier: `*/break-the-analysis`
- *Post-build Actions section:* Publish to Code Dx. Requires: Code Dx URL, admin API key, and Code Dx project. Then (a) delete `**` from "Source and Binary Files"; (b) check "include git source" and "Specific Branch Name" is *break-the-analysis*; (c) no tool output files; (d) "Analysis Name" doesn't matter; (e) no Target or Base Branches; (f) "Error Handling" is *Mark Build as Failed*; (g) check "Wait for analysis result"; (h) "Policy Behavior/Break build Action" is *Mark Build as Failed*; (i) "Build Failure Conditions" and the rest are *None*; and (j) "Graph Options" is zero. Save.

## Step 1: build passes because there's a successful analysis and no policy violation
In Jenkins, 
- Build Now
- Click the new job link
- Console output (left side): successful analysis, checks for policy violations, and build is successful
- Return to project: successful build (green checkmark)


## Step 2: build fails because the analysis fails
In Jenkins, 
- Configure > Tool Output Files: add `Test-break-the-analysis.xml`
- Save
- Build Now
- Console output: analysis fails and build fails
- Return to project: build fails (red "x")

## Step 3: build fails because policy that is set to "break build" has a violation 
In Jenkins,
- Configure > Tool Output: (a) remove `Test-break-the-analysis.xml` and (b) add  `Policies-test-critical-severity.xml`
- Save
- Build Now
- Console output: policy that is set to "break build" has a violation and build fails
- Return to project: build fails (red "x")
- note: in SRM, there's a red triangle icon indicating the policy failed

## Installing an Unreleased Jenkins Plugin (`.hpi`) to Jenkins
Jenkins Dashboard > Manage Jenkins > Plugin Manager >
Advanced settings (left side) > Deploy Plugin > Choose File > browse to HPI being tested > click Deploy (wait for something about restarting Jenkins > restart Jenkins


