# Sync-Kiwi-Gitlab-issues
Code for the YouTube video https://youtu.be/DjnyrirGTng

# Step 1 - make sure you have Docker installed 
I'm using actual Docker-ce for this video, not Podman see this guide for RHEL https://docs.docker.com/engine/install/rhel/
Reason I'm also using RHEL is for compatibility reasons, when creating a video for demo want to have not too much issues for the installation. 

## KIWI TCMS
- Install Kiwi through script kiwi.sh ```sudo ./kiwi.sh```
- After it has completed add your required config (Test Plan, Product, version etc and Test Case) in there. 
- Note: I do change the ports of Kiwi because of the port conflicts (80/443 is also being used by Gitlab). 
- After Kiwi has been installed and is running, stop the Containers using ```/usr/local/bin/docker-compose down```
- Change the ports in de ```docker-compose.yml``` file and up the containers again ```/usr/local/bin/docker-compose up -d```

## Gitlab EE
```
sudo dnf install -y curl policycoreutils openssh-server perl
sudo systemctl start sshd --now
```
- Note: only needed if you want to use firewall, generally a GOOD practice but for this throw away demo environment, didn't even bother. Check if opening the firewall is needed with: 
```sudo systemctl status firewalld```
```
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo systemctl reload firewalld
```
```curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash```
- Note: you need to adjust host or ip below to match your own value. 
``sudo EXTERNAL_URL="http://<host or ip>" dnf install -y gitlab-ee```
- Beware that it could take some time for this to complete... 
- To get the initial root password ```cat /etc/gitlab/initial_root_password```
- Open browser (in my case it was http... but could be different for you if you use https) to the IP of the machine and login with root and password
- Create project and setup API Token for that project 

## Configuration of Kiwi
- Like described in the video, use the following parameters when filling the form:
  - base_url: URL to a GitLab repository for which we're going to report issues. For example http://192.168.1.10/root/my-awesome-project
  - api_url: URL to a GitLab instance. For example http://192.168.1.10
  - api_password: GitLab API token with the ``api`` scope. See https://gitlab.com/-/profile/personal_access_tokens

# Now create new testrun and sync some issues!


# References
https://github.com/kiwitcms/Kiwi/pull/2565/commits/ae557ac2a774f54fe553a45ce09feab61d8c4e10
https://docs.docker.com/engine/install/rhel/
