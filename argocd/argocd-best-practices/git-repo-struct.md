## Git Repository Structure

- Having separate repository for Configurations than the Application code is a good practice.

**Advantages:**

- Cleaner Commit history
- Separation of access control
- Less Overhead on Argo CD repo-server for cloning the repo.

### Repo Structure for Different Environments

Types of Environments:
- Static Environments(Dev, Staging, Production)
- Dynamic Environments(Feature Branches)

#### Dynamic Environments

- Using **ApplicationSets Pull request generator** to create a new environment for each feature branch. And the env will get deleted once the PR is closed.
- **Single branch for all the dynamic environments**. And a folder per environment and each folder will contain the related manifests. (You need to write a script using CI pipeline to create theses folders and manifests per Pull request)

#### Static Environments

- Branch per Static Environment. Example: `dev`, `staging`, `production`
- Single branch for all the static environments. 

Date of Creation: 28/04/2024