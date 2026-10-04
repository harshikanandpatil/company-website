
# Git + Docker + Jenkins + ngrok
## Company Website Deployment Documentation

This document describes the commands used to build and run a company website using Docker, push the project to GitHub, install and configure Jenkins, integrate Jenkins with Docker, and expose the application using ngrok.

---

# 1. Check User Groups

### Command

```bash
groups
```

### Purpose

Displays the groups associated with the current Linux user.

### Why it is used

This is useful for checking whether the current user belongs to the `docker` group and can execute Docker commands without `sudo`.

---

# 2. Check Running Docker Containers

### Command

```bash
docker ps
```

### Purpose

Displays currently running Docker containers.

### Example Output

```text
CONTAINER ID   IMAGE             PORTS
xxxxxxxxxxxx   company-website   0.0.0.0:8080->80/tcp
```

---

# 3. Check Docker Images

### Command

```bash
docker images
```

### Purpose

Lists all Docker images available locally.

---

# 4. Build the Company Website Docker Image

### Command

```bash
docker build -t company-website .
```

### Purpose

Builds a Docker image from the `Dockerfile` located in the current directory.

### Explanation

- `docker build` → Builds a Docker image.
- `-t company-website` → Assigns the image name `company-website`.
- `.` → Uses the current directory as the Docker build context.

---

# 5. Verify Docker Image

### Command

```bash
docker images
```

### Purpose

Confirms that the `company-website` image was successfully created.

---

# 6. Run the Company Website Container

### Command

```bash
docker run -d \
  --name company-website \
  -p 8080:80 \
  company-website
```

### Purpose

Creates and starts a Docker container from the `company-website` image.

### Explanation

| Option | Description |
|---|---|
| `-d` | Runs the container in detached/background mode |
| `--name company-website` | Assigns a name to the container |
| `-p 8080:80` | Maps host port 8080 to container port 80 |
| `company-website` | Docker image name |

### Application URL

```text
http://localhost:8080
```

---

# 7. Verify the Running Container

### Command

```bash
docker ps
```

### Purpose

Checks whether the `company-website` container is running.

---

# 8. Find the Public IP Address

### Command

```bash
curl ifconfig.me
```

### Purpose

Displays the public IP address of the system.

### Example

```text
106.215.181.146
```

The website can potentially be accessed using:

```text
http://<PUBLIC-IP>:8080
```

---

# 9. Test the Website Using curl

### Command

```bash
curl http://localhost:8080
```

### Purpose

Tests whether the website is responding locally on port `8080`.

### Expected Result

The command should return the HTML content of the website.

---

# 10. Check Port 8080

### Command

```bash
sudo ss -tulpn | grep 8080
```

### Purpose

Checks whether port `8080` is listening on the system.

### Why it is used

This helps troubleshoot situations where the Docker application is running but the website cannot be accessed.

---

# 11. Remove the Docker Container

### Command

```bash
docker rm -f company-website
```

### Purpose

Stops and removes the `company-website` container.

### Explanation

- `rm` → Removes a container.
- `-f` → Forces removal, including stopping a running container.

> Note: Removing a container does not remove the Docker image.

---

# 12. Check Docker Containers Again

### Command

```bash
docker ps
```

### Purpose

Confirms whether the container has been removed.

---

# 13. Check Git Installation

### Command

```bash
git --version
```

### Purpose

Displays the installed Git version.

---

# 14. Check Git Repository Status

### Command

```bash
git status
```

### Purpose

Displays the current Git repository status, including modified, untracked, and staged files.

---

# 15. Stage Project Files

### Command

```bash
git add .
```

### Purpose

Stages all modified and untracked files in the current directory.

---

# 16. Commit the Project

### Command

```bash
git commit -m "Added company website with Docker"
```

### Purpose

Creates a Git commit containing the staged changes.

---

# 17. Check Git Branch

### Command

```bash
git branch
```

### Purpose

Displays the local Git branches and identifies the currently active branch.

---

# 18. Push Code to GitHub

### Command

```bash
git push origin main
```

### Purpose

Pushes the local `main` branch to the GitHub repository named `origin`.

---

# 19. Check Git Remote Repository

### Command

```bash
git remote -v
```

### Purpose

Displays the remote GitHub repository URL configured for the project.

---

# 20. Rename Current Branch to main

### Command

```bash
git branch -M main
```

### Purpose

Renames the current Git branch to `main`.

---

# 21. Push Main Branch and Set Upstream

### Command

```bash
git push -u origin main
```

### Purpose

Pushes the `main` branch to GitHub and establishes `origin/main` as its upstream branch.

After this, future pushes can generally be performed using:

```bash
git push
```

---

# 22. Check Java Version

### Command

```bash
java -version
```

### Purpose

Checks whether Java is installed and displays the installed Java version.

### Why it is required

Jenkins requires Java to run.

---

# 23. Check Jenkins Service

### Command

```bash
sudo systemctl status jenkins
```

### Purpose

Checks the current status of the Jenkins service.

---

# 24. Search for Jenkins Package

### Command

```bash
sudo apt search jenkins
```

### Purpose

Searches the Ubuntu package repositories for Jenkins packages.

---

# 25. Update Ubuntu Packages

### Command

```bash
sudo apt update
```

### Purpose

Updates the local package repository information.

---

# 26. Upgrade Installed Packages

### Command

```bash
sudo apt upgrade -y
```

### Purpose

Upgrades installed Ubuntu packages.

### Explanation

`-y` automatically confirms the installation prompts.

---

# 27. Install Jenkins Dependencies

### Command

```bash
sudo apt install -y git curl wget gnupg fontconfig
```

### Purpose

Installs common packages required for Jenkins installation and operation.

### Packages

- `git` → Source code management
- `curl` → HTTP/network requests
- `wget` → Download files
- `gnupg` → GPG key management
- `fontconfig` → Font configuration support

---

# 28. Locate Java

### Command

```bash
which java
```

### Purpose

Displays the location of the Java executable.

---

# 29. Add Jenkins Repository Key

### Command

```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
```

### Purpose

Downloads and stores the Jenkins repository signing key.

---

# 30. Add Jenkins Repository

### Command

```bash
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | \
sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

### Purpose

Adds the official Jenkins Debian repository to the system's APT sources.

---

# 31. Update Package Repository

### Command

```bash
sudo apt update
```

### Purpose

Refreshes package information after adding the Jenkins repository.

---

# 32. Install Jenkins

### Command

```bash
sudo apt install -y jenkins
```

### Purpose

Installs Jenkins on the system.

---

# 33. Check Jenkins Service Status

### Command

```bash
sudo systemctl status jenkins
```

### Purpose

Verifies whether Jenkins is running successfully.

---

# 34. Retrieve Jenkins Initial Administrator Password

### Command

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

### Purpose

Displays the initial Jenkins administrator password required during the first Jenkins web setup.

### Jenkins URL

```text
http://localhost:8080
```

---

# 35. Check Docker

### Command

```bash
docker ps
```

### Purpose

Confirms that Docker is installed and accessible.

---

# 36. Add Jenkins User to Docker Group

### Command

```bash
sudo usermod -aG docker jenkins
```

### Purpose

Adds the Jenkins service account to the Docker group.

### Why it is required

This allows Jenkins to execute Docker commands without requiring `sudo`.

---

# 37. Verify Jenkins Docker Group

### Command

```bash
groups jenkins
```

### Purpose

Checks whether the `jenkins` user belongs to the Docker group.

### Expected Result

The output should contain:

```text
docker
```

---

# 38. Restart Jenkins

### Command

```bash
sudo systemctl restart jenkins
```

### Purpose

Restarts Jenkins so the new Docker group membership can take effect.

---

# 39. Test Docker Access as Jenkins

### Command

```bash
sudo -u jenkins docker ps
```
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/d7419599-9ac2-4212-bddf-214e1143f58e" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/ad34caba-8ccd-4872-a6ac-a36e993225ee" />

### Purpose

Tests whether the Jenkins user can execute Docker commands.

### Expected Result

Docker should execute without a permission-denied error.

---

# 40. Navigate to Company Website Project

### Command

```bash
cd ~/company-website
```

### Purpose

Moves into the local company website project directory.

---

# 41. List Project Files

### Command

```bash
ls
```

### Purpose

Displays files and directories inside the project.

---

# 42. Verify GitHub Remote

### Command

```bash
git remote -v
```

### Purpose

Checks the GitHub repository configured for the project.

---

# 43. Push Project to GitHub

### Command

```bash
git push -u origin main
```

### Purpose

Pushes the project to the GitHub `main` branch and establishes upstream tracking.

---

# 44. Stage Website Changes

### Command

```bash
git add .
```

### Purpose

Stages all project changes for the next commit.

---

# 45. Commit Docker Project

### Command

```bash
git commit -m "Added company website Docker project"
```

### Purpose

Creates a commit containing the company website Docker project changes.

---

# 46. Push Changes to GitHub

### Command

```bash
git push -u origin main
```

### Purpose

Uploads the latest committed changes to GitHub.

---

# 47. Build Docker Image Again

### Command

```bash
docker build -t company-website .
```

### Purpose

Builds an updated Docker image for the company website.

---

# 48. Verify Docker Images

### Command

```bash
docker images
```

### Purpose

Confirms that the `company-website` Docker image exists.

---

# 49. Run Website on Port 8081

### Command

```bash
docker run -d \
  --name company-website \
  -p 8081:80 \
  company-website
```

### Purpose

Runs the website container and maps host port `8081` to container port `80`.

### Application URL

```text
http://localhost:8081
```

### Why port 8081?

Port `8081` was used to avoid conflict with another application already using port `8080`.

---

# 50. Verify Container

### Command

```bash
docker ps
```

### Purpose

Checks whether the new container is running.

---

# 51. Test Website on Port 8081

### Command

```bash
curl http://localhost:8081
```

### Purpose

Tests whether the website is responding on port `8081`.

---

# 52. Remove Website Container

### Command

```bash
docker rm -f company-website
```

### Purpose

Stops and removes the running website container.

---

# 53. Verify Container Removal

### Command

```bash
docker ps
```

### Purpose

Confirms that the container is no longer running.

---

# 54. Verify Docker Images

### Command

```bash
docker images
```

### Purpose

Confirms that the Docker image still exists after removing the container.

---

# 55. Test Website After Container Removal

### Command

```bash
curl http://localhost:8081
```

### Expected Result

The request should fail because the container is no longer running and port `8081` is no longer serving the website.

---

# 56. Display Docker Images

### Command

```bash
docker image
```

### Purpose

Displays Docker image-related commands/help.

### Recommended Command

For listing images, use:

```bash
docker images
```

---

# 57. Commit Website Version 2

### Command

```bash
git add .
```

Stage the latest website changes.

Then:

```bash
git commit -m "Updated website to version 2"
```

Create a Git commit for the updated website.

---

# 58. Push Version 2 to GitHub

### Command

```bash
git push origin main
```

### Purpose

Pushes the latest website version to the GitHub `main` branch.

---

# 59. Install ngrok Repository Key

### Command

```bash
curl -s https://ngrok-agent.s3.amazonaws.com/ngrok.asc | \
sudo tee /etc/apt/trusted.gpg.d/ngrok.asc >/dev/null
```

### Purpose

Downloads and installs the ngrok repository signing key.

> This command appeared twice in the command history. It only needs to be executed once.

---

# 60. Add ngrok Repository

### Command

```bash
echo "deb https://ngrok-agent.s3.amazonaws.com bookworm main" | \
sudo tee /etc/apt/sources.list.d/ngrok.list
```

### Purpose

Adds the ngrok APT repository to the system.

---

# 61. Update Package Repository

### Command

```bash
sudo apt update
```

### Purpose

Refreshes the package information after adding the ngrok repository.

---

# 62. Install ngrok

### Command

```bash
sudo apt install ngrok
```

### Purpose

Installs ngrok on the system.

---

# 63. Verify ngrok Installation

### Command

```bash
ngrok version
```

### Purpose

Displays the installed ngrok version.

---

# 64. Configure ngrok Authentication

### Command

```bash
ngrok config add-authtoken YOUR_NGROK_TOKEN
```

### Purpose

Associates the local ngrok installation with an authenticated ngrok account.

### Important Security Note

Do **not** commit or share the actual ngrok authtoken in GitHub, documentation, screenshots, or shell history.

Use:

```bash
YOUR_NGROK_TOKEN
```

as a placeholder in documentation.

---

# 65. Expose Local Application Using ngrok

### Command

```bash
ngrok http 8080
```

### Purpose

Creates a public ngrok URL that forwards traffic to the local application running on port `8080`.

### Important

If the website is actually running on port `8081`, use:

```bash
ngrok http 8081
```

The ngrok port must match the port on which the Docker application is currently running.

---

# 66. Clear Terminal

### Command

```bash
clear
```

### Purpose

Clears the terminal screen.

---

# 67. View Command History

### Command

```bash
history
```

### Purpose

Displays previously executed shell commands.

---

# Overall DevOps Workflow

The complete workflow followed in this project is:

```text
Developer
   |
   v
Git / GitHub
   |
   v
Source Code
   |
   v
Dockerfile
   |
   v
Docker Image
   |
   v
Docker Container
   |
   v
Application Port
   |
   +------> Jenkins
   |          |
   |          v
   |      CI/CD Automation
   |
   v
ngrok
   |
   v
Public URL
```

---

# Important Commands Summary

## Docker

```bash
docker ps
docker images
docker build -t company-website .
docker run -d --name company-website -p 8080:80 company-website
docker run -d --name company-website -p 8081:80 company-website
docker rm -f company-website
```

## Git

```bash
git status
git add .
git commit -m "Added company website with Docker"
git branch
git branch -M main
git remote -v
git push -u origin main
```

## Jenkins

```bash
sudo apt update
sudo apt install -y git curl wget gnupg fontconfig
sudo apt install -y jenkins
sudo systemctl status jenkins
sudo systemctl restart jenkins
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
sudo usermod -aG docker jenkins
groups jenkins
sudo -u jenkins docker ps
```

## ngrok

```bash
ngrok version
ngrok config add-authtoken YOUR_NGROK_TOKEN
ngrok http 8080
```

---

# Project Objective

The objective of this project is to demonstrate a basic DevOps workflow for deploying a company website using:

- Git and GitHub for source-code management
- Docker for application containerization
- Jenkins for CI/CD automation
- Linux/WSL for the development environment
- ngrok for temporary public access to the locally running application

This setup provides a practical foundation for understanding how source code can move from a Git repository through Docker-based deployment and Jenkins automation.

---

# Security Best Practices

1. Never commit passwords, API keys, access keys, or ngrok tokens to GitHub.
2. Use placeholders such as `YOUR_NGROK_TOKEN` in documentation.
3. If a secret is accidentally exposed, revoke or rotate it immediately.
4. Add sensitive files to `.gitignore`.
5. Avoid running unnecessary services with `sudo`.
6. Use Jenkins credentials management instead of hardcoding secrets in Jenkins jobs.
7. Do not expose Jenkins (`8080`) publicly unless authentication and network security are properly configured.
