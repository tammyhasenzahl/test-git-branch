# break-the-analysis branch

To break a build
1. in Code Dx, create a policy where the rule breaks the build (eg filter = only critical and high severity; fix by = not required; and action = Break build)
2. in Jenkins, create a Freestyle or Pipeline project that pulls this branch's XMLs to force the policy violation (add the XMLs to the tool output field; no source is required for this test). The XMLs are `Policies-test-critical-severity.xml` and `Test-break-the-analysis.xml`.

Note: the WebGoat source only has Info, Low, and Medium results.
