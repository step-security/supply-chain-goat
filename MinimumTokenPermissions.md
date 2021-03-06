<p align="left">
  <img src="https://raw.githubusercontent.com/step-security/supply-chain-goat/main/images/Logo.png" alt="Step Security Logo" width="340">
</p>

# Tutorial: Set minimum permissions for the `GITHUB_TOKEN`
_Estimated completion time: 3 minutes_

## Summary of past incidents

### VS Code GitHub Actions Exploit
In December 2020, [ryotkak](https://twitter.com/ryotkak) reported as part of the Bug Bounty program how he exfiltrated the `GITHUB_TOKEN` from a GitHub Actions workflow. The token was used to push code to a release branch. You can read the details [here](https://www.bleepingcomputer.com/news/security/heres-how-a-researcher-broke-into-microsoft-vs-codes-github/?&web_view=true) and [here](https://blog.ryotak.me/post/vscode-write-access/). 

### OSSF Scorecards check for Token permissions

[Scorecards](https://github.com/ossf/scorecard) is an automated security tool that flags risky supply chain practices. You can use the [Scorecards action](https://github.com/marketplace/actions/ossf-scorecard-action) to follow best security practices. Once configured, the Scorecards action runs automatically on repository changes, and alerts developers about risky supply chain practices using the built-in code scanning experience. The Scorecards project runs a number of checks, including for token permissions. 

## How does StepSecurity mitigate this threat?
[StepSecurity's Online tool](https://app.stepsecurity.io) uses https://github.com/step-security/secure-workflows which has a growing knowledge base of token permissions needed by different GitHub Actions to set the appropriate minimum GitHub token permissions. Setting these minimum token permissions secure the workflow by revoking the redundant access that the github token provides by default. [OSSF Scorecards](https://github.com/ossf/scorecard) recommends using the StepSecurity tool to set minimum token permissions.

## Tutorial
Learn how to set minimum permissions for the `GITHUB_TOKEN`. 

1. Create a fork of the repo.

2. Go to the `Actions` tab in the fork. Click the `I understand my workflows, go ahead and enable them` button. 
   
   <img src="https://raw.githubusercontent.com/step-security/supply-chain-goat/main/images/EnableActions.png" alt="Enable Actions" width="800">

3. Click on the `Test and coverage` workflow and then click `Run workflow`. Once you do this, a GitHub workflow will get triggered.

   <img src="https://raw.githubusercontent.com/step-security/supply-chain-goat/main/images/RunCIWorkflows.png" alt="Run Workflow" width="820">

4. Click on the Actions tab again, click on the workflow that just started, and in the job run logs, have a look at the permissions assigned to the `GITHUB_TOKEN`. 
   
   <img src="https://raw.githubusercontent.com/step-security/supply-chain-goat/main/images/TokenPermissions.png" alt="Token permissions" width="805">

5. By default, the `GITHUB_TOKEN` has a lot of permissions assigned. As a [security best practice](https://github.blog/changelog/2021-04-20-github-actions-control-permissions-for-github_token/), the `GITHUB_TOKEN` should be assigned the minimum permissions.

6. Review the workflow file at `./github/workflows/ci.yml`. You can now manually add the `permissions` key, but it is hard to know what the permissions should be. Different 3rd party Actions may use different permissions. In this tutorial, let us fix the permissions automatically. 

7. Visit https://app.stepsecurity.io. Copy the workflow file and paste it in the editor. For this tutorial, only check the `Restrict permissions for GITHUB_TOKEN` check box and click on `Secure workflow` button. 

    <img src="https://raw.githubusercontent.com/step-security/supply-chain-goat/main/images/SetTokenPermissions.png" alt="Set token permissions" width="815">

8. Observe that the workflow now has updated permissions. Copy the updated workflow and edit the workflow file at `./github/workflows/ci.yml`. 

9. Run the workflow again. 

10. Have a look at the permissions assigned to the `GITHUB_TOKEN`. Now it has the minimum permissions assigned. Even if the token is compromised, the damage potential is reduced. 
