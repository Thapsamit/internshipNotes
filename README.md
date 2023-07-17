## How to setup gitlab runner in aws ec2 ubuntu instance


### Steps to inatall and configure gitlab runner 
- Install repo first

```bash
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
```

- Install gitlab runner

```bash
sudo apt-get install gitlab-runner
```

- Regiser gitlab runner
```bash
sudo apt-get install gitlab-runner
```

- Setup gitlab runner from gitlab.com under your project

   - Here are the steps to obtain the GitLab instance URL:

   - Log in to your GitLab account and navigate to the project for which you want to set up CI/CD.

   - Click on the "Settings" tab within the project.

- Add tags to the gitlab runner
- create and get url and token

   - In the left sidebar, select "CI/CD".

   - Scroll down to the "Runners" section and look for the "Specific Runners" or "Shared Runners" settings, depending on your setup.

   - In the "Runners" section, you will find the URL listed under "Set up a specific Runner manually" or "Shared Runners settings". It will typically be in the format: https://gitlab.example.com/.
