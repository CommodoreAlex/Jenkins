# Installing and Setting Up Jenkins ⚙️

This guide will walk you through installing Jenkins on a Linux system, configuring it, navigating some common Java related or port-occupied setup issues that cause havoc on getting started and getting your first Jenkins instance up and running.

---

## Prerequisites ✅

Before you start, ensure the following:

1. **Java Installed**:  
   Jenkins requires Java 8 or 11. Verify your installation:  
```bash
java -version
```

If Java is not installed, install it with:
```bash
sudo apt update
sudo apt install openjdk-11-jdk -y
```

2. **System Resources**:
    - Minimum: 256MB RAM and 1GB of storage.
    - Recommended: 1GB+ RAM for larger setups.

---

## Step 1: Install Jenkins 📦

## Debian/Ubuntu[](https://www.jenkins.io/doc/book/installing/linux/#debianubuntu)

On Debian and Debian-based distributions like Ubuntu you can install Jenkins through `apt`.

### [](https://www.jenkins.io/doc/book/installing/linux/#debian-stable)Long Term Support release[](https://www.jenkins.io/doc/book/installing/linux/#debian-stable)

A [LTS (Long-Term Support) release](https://www.jenkins.io/download/lts/) is chosen every 12 weeks from the stream of regular releases as the stable release for that time period. It can be installed from the [`debian-stable` apt repository](https://pkg.jenkins.io/debian-stable/).

Copy and paste this into your terminal to install Jenkins:
```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
  
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
  
sudo apt-get update

sudo apt-get install jenkins
```

---

## Step 2: Start Jenkins 🚀

1. **Start the Jenkins Service**:
```bash
sudo systemctl start jenkins
```

Note: Jenkins may fail to start due to Java version incompatibilities. You can amend this issue by installing Java 17 or Java 21 or going down to version 11.

Selecting (2) which points to the compatible version will fix that issue:
```bash
┌──(root㉿kali)-[/home/kali]
└─# sudo update-alternatives --config java

There are 4 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                         Priority   Status
------------------------------------------------------------
  0            /usr/lib/jvm/java-23-openjdk-amd64/bin/java   2311      auto mode
  1            /usr/lib/jvm/java-11-openjdk-amd64/bin/java   1111      manual mode
* 2            /usr/lib/jvm/java-17-openjdk-amd64/bin/java   1711      manual mode
  3            /usr/lib/jvm/java-21-openjdk-amd64/bin/java   2111      manual mode
  4            /usr/lib/jvm/java-23-openjdk-amd64/bin/java   2311      manual mode

Press <enter> to keep the current choice[*], or type selection number: 
```

**Follow the instructions in this (2:30 minutes) video:**
https://www.youtube.com/watch?v=5HYuqzaHzV8

Another issue that could occur (port in-use) which is difficult to find through systemctl commands.

You can manually start Jenkins if required to see a more verbose output:
```bash
sudo java -jar /usr/share/java/jenkins.war
```

You can address this by finding the processes using default port in-use and stopping them:
```bash
┌──(root㉿kali)-[/home/kali]
└─# sudo lsof -i :8080

COMMAND   PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
ruby     2208  git   21u  IPv4  19688      0t0  TCP localhost:http-alt (LISTEN)
ruby     4828  git   21u  IPv4  19688      0t0  TCP localhost:http-alt (LISTEN)
ruby     4830  git   21u  IPv4  19688      0t0  TCP localhost:http-alt (LISTEN)
ruby     4832  git   21u  IPv4  19688      0t0  TCP localhost:http-alt (LISTEN)
ruby     4834  git   21u  IPv4  19688      0t0  TCP localhost:http-alt (LISTEN)
ruby     4836  git   21u  IPv4  19688      0t0  TCP localhost:http-alt (LISTEN)
ruby     4838  git   21u  IPv4  19688      0t0  TCP localhost:http-alt (LISTEN)
ruby     4841  git   21u  IPv4  19688      0t0  TCP localhost:http-alt (LISTEN)
ruby    35825  git   21u  IPv4  19688      0t0  TCP localhost:http-alt (LISTEN)

┌──(root㉿kali)-[/home/kali]
└─# sudo kill -9 2208 4828 4830 4832 4834 4836 4838 4841 35825
```

2. **Enable Jenkins to Start on Boot**:
```bash
sudo systemctl enable jenkins
```

3. **Check the Status of Jenkins**:  

Ensure Jenkins is running:
```bash
sudo systemctl status jenkins
```

---

## Step 3: Access Jenkins 🖥️

1. Open a web browser and navigate to:
```bash
http://<your-server-ip>:8080
```

For local setups, use:
```bash
http://localhost:8080
```

You will run into this screen that blocks you from accessing the Jenkins page initially:
![4](https://github.com/user-attachments/assets/e9f55443-e845-49d7-ae91-5cf4b0a91e8c)

2. Unlock Jenkins:

Locate the initial admin password:
```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy and paste this password into the web interface to proceed.

3. Follow the Setup Wizard:

![5](https://github.com/user-attachments/assets/1242b40c-828c-484b-a0d3-1a9c5e7eb9d4)

- Install recommended plugins.
- Create your admin user account.

---

## Step 4: Post-Installation Configuration 🔧

1. **Configure System Settings**:  
    Navigate to **Manage Jenkins > Configure System** and set up global settings like environment variables.
    
2. **Secure Your Instance**:
    
    - Enable HTTPS (optional but recommended).
    - Set up role-based access control under **Manage Jenkins > Configure Global Security**.

1. **Verify Plugins**:  
    Go to **Manage Jenkins > Plugin Manager** and ensure critical plugins are installed and up-to-date.
    

---


Jenkins is now installed and ready to use. To start creating your first job or pipeline, check out the [Jenkins Basics Guide](#).
