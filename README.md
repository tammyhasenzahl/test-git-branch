# break-the-analysis branch

To break a build
1. in Code Dx, create a policy where the rule breaks the build (eg filter = only critical and high severity; fix by = not required; and action = Break build)
2. in Jenkins, create a project that pulls in this branch source and Test-break-the-analysis.xml
