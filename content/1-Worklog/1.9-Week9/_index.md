---
title: "Week 9 Worklog"
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

### Week 9 Objectives:

* **CI/CD Fundamentals:** Understand Continuous Integration and Continuous Deployment pipelines.
* **Source Control:** Use AWS CodeCommit (or GitHub integration) for version control.
* **Build Automation:** Configure AWS CodeBuild to compile code and run tests.
* **Deployment Automation:** Use AWS CodeDeploy to automate releases to Compute services.
* **Pipeline Orchestration:** Tie everything together with AWS CodePipeline.

### Tasks to be carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| 1 | - Introduction to AWS Code Services (CodeCommit) <br> - **Practice:** <br>&emsp; + Set up a repository for the Jewelry Project Backend <br>&emsp; + Push initial code skeleton | 27/10/2025 | 27/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 2 | - AWS CodeBuild <br> - **Practice:** <br>&emsp; + Create a `buildspec.yml` file <br>&emsp; + Run a build job to install dependencies and run unit tests | 28/10/2025 | 28/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - AWS CodeDeploy <br> - **Practice:** <br>&emsp; + Create an `appspec.yml` file <br>&emsp; + Deploy a revision to a test EC2 instance or Lambda | 29/10/2025 | 29/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - AWS CodePipeline <br> - **Practice:** <br>&emsp; + Create a pipeline that triggers on Git push -> Builds -> Deploys | 30/10/2025 | 30/10/2025 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - **Team Project (Backend):** <br>&emsp; + Initialize CI/CD pipeline for the Jewelry API <br>&emsp; + Ensure every commit triggers a build check | 31/10/2025 | 31/10/2025 | <https://cloudjourney.awsstudygroup.com/> |

### Week 9 Achievements:

* Established a fully automated CI/CD pipeline for backend development.
* Integrated source control with build and deploy systems to reduce manual errors.
* Configured automated testing within the build phase to ensure code quality for the team project.