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

![6](https://github.com/user-attachments/assets/40df2070-94e6-4c77-b6e8-9d702170616a)


## 1. Configure System Settings
- **Navigate to "Manage Jenkins"**:
  1. Go to the Jenkins dashboard.
  2. Click on **Manage Jenkins** from the sidebar.
  3. Select **Configure System** from the options.

Should be located at:
```bash
http://localhost:8080/manage/configure
```

- **Here you are able to set up global settings if desired**:
  - **Environment Variables**: Add any required global environment variables for your build processes.
  - **JDK**: Ensure the Java Development Kit (JDK) is correctly configured. For example, use Java 17 if compatible.
  - **Git**: Configure the Git installation path if not automatically detected.
  - **SMTP Settings**: Set up your SMTP server for email notifications.
  - **Tool Locations**: Specify locations for tools like Maven, Gradle, and others.

---

## 2. Secure Your Instance
- **Enable HTTPS** (optional but recommended):
  1. Use a reverse proxy (e.g., Nginx or Apache) or configure Jenkins directly to enable HTTPS.
  2. If using Jenkins directly, update the service configuration to specify the SSL certificate and private key paths.
  3. HTTPS ensures secure communication between your Jenkins instance and users.
 
- **Source to configure HTTPS directly in your local Jenkins instance**:
https://dev.to/jeansen/set-up-jenkins-with-https-250k

- **Enable Security Settings**:
  - Use Matrix-based security if RBAC isn't available (this is a plugin to install later).
  - Restrict anonymous access to the Jenkins instance.
  - Use a strong administrator password and disable sign-ups unless necessary.

---

## 3. Verify Plugins
- **Check Installed Plugins**:
  1. Navigate to **Manage Jenkins** > **Plugin Manager**.
  2. Go to the **Installed** tab to confirm that key plugins are available:
     - **Git Plugin** (for Git repository integration)
     - **Pipeline Plugin** (to create CI/CD pipelines)
     - **Role-Based Authorization Strategy Plugin** (for RBAC)
     - **Credentials Plugin** (for securely storing secrets)

- **Install or Update Plugins**:
  1. In **Plugin Manager**, go to the **Available** tab to search for new plugins.
  2. Go to the **Updates** tab to update installed plugins to their latest compatible versions.

![8](https://github.com/user-attachments/assets/dbabc8da-e580-4c46-aea3-6f4361d24a34)


You will encounter typical documentation / instructions for plugin installation:
![7](https://github.com/user-attachments/assets/ebad95c8-93ad-41e8-ad68-b622aa73611e)


- **Restart Jenkins**:
  - After installing or updating plugins, restart Jenkins to ensure the changes are applied properly.
  - Use the safe restart option under **Manage Jenkins** to avoid interrupting active jobs.

---


Jenkins is now installed and ready to use. To start creating your first job or pipeline, check out the [Jenkins Basics Guide](#).
