# Jenkins Administration Tasks Guide 🛠️

This guide covers essential Jenkins administration tasks to help you maintain, secure, and optimize your Jenkins environment.

---

## Overview 🗂️

Jenkins administration involves tasks like managing users, scheduling backups, monitoring resources, and keeping your instance secure and up-to-date. Below are the key topics covered:

- [User Management](#user-management-👥)
- [Backup and Restore](#backup-and-restore-💾)
- [Monitoring Jenkins](#monitoring-jenkins-📊)
- [Plugin Management](#plugin-management-🧩)
- [Securing Jenkins](#securing-jenkins-🔒)

---

## User Management 👥

1. **Create Users**:
   - Navigate to **Manage Jenkins > Manage Users**.
   - Click **Create User** and fill in the details (username, password, etc.).

![3](https://github.com/user-attachments/assets/97bbb773-f506-40c8-8a98-f95e19bc4590)

![4](https://github.com/user-attachments/assets/81df2656-649c-498a-8e39-5f79a16c8cf6)

2. **Assign Roles** (with Role-Based Access Control plugin):

Go to **Manage Jenkins > Security**.
   - Change the Authorization parameter to "Role-Based Strategy".
   - Ensure that Security Realm is equal to "Jenkin's own user database".

![1](https://github.com/user-attachments/assets/58067149-0f51-413b-9c2c-ce9d65da7098)


Now the role-based strategy will be available to configure under "Manage Jenkins".

![2](https://github.com/user-attachments/assets/9556303b-e4b9-4e85-8d0e-0a74eff970bb)


Go to **Manage Jenkins > Manage and Assign Roles:
   - Assign roles like Admin, Developer, or Viewer to users.

Now you can see Admin exists which has access to all permissions.

If you want to create a view-only user account you can select 'read' along all of the categories, which follows the principle of least-privilege.

You can repeat this to create users with build-only permissions and so-on as well.

![5](https://github.com/user-attachments/assets/fa463d1d-a954-4dc4-8f99-6a061a7fd72a)

For a source-video see: https://www.youtube.com/watch?v=iR7UW1fgq7E

3. **Audit User Activity**:
   - Check the **Audit Trail** or **Job History** to monitor user actions.

Install the Audit Trail Plugin:

Log in to Jenkins.
- Go to Manage Jenkins > Plugin Manager.
- the Available tab, search for Audit Trail.

![6](https://github.com/user-attachments/assets/d2991409-abcf-4c09-9cea-554a9e076e3e)

Select the plugin and click Install without restart (or Download now and install after restart).

Documentation for Audit Trail: https://plugins.jenkins.io/audit-trail/

2. **Configure the Audit Trail**:
   - After installation, navigate to **Manage Jenkins** > **System Configuration (System)**.
  
![9](https://github.com/user-attachments/assets/b9f8e8d9-4e5d-45d9-ad59-595e0d8cc58b)

   - Scroll down to the **Audit Trail** section.
   - Enable **Audit Trail** and configure it as needed:
     - **Log to File**: You can choose to log actions to a file for easier auditing and review. Set the file path where logs will be saved (e.g., `/var/log/jenkins/audit.log`).
    
![7](https://github.com/user-attachments/assets/310d1eda-c1a6-45a4-8992-3c6ecf525adb)

![8](https://github.com/user-attachments/assets/ce0be0ca-1cf7-4c45-8801-7d36537b4e7f)

- **Log to System Log**: You can log the actions to the Jenkins system log if desired.
- **Actions to Track**: You can select what actions you want to monitor, such as job creation, configuration changes, build triggers, etc.

3. **Save the Configuration**:
   - Once you've made your desired settings, save the configuration.

### Viewing the Logs:
- After you’ve set up the Audit Trail plugin, user actions will be logged and available for review:
  - **Audit Trail Logs**: If you’ve configured logging to a file, you can view the logs by accessing the file directly on the server (e.g., `cat /var/log/jenkins/audit.log`).
  - **System Log**: If you opted for the system log option, you can navigate to **Manage Jenkins** > **System Log** to view the audit trail entries there.

You can monitor job histories by reviewing the logs for actions related to job triggers and modifications. This gives a clear view of who triggered which jobs, when, and any changes made to job configurations.

You can also review ALL SYSTEM LOGS BY VISITNG:
```bash
http://localhost:8080/manage/log/all
```

![10](https://github.com/user-attachments/assets/d555fb99-4881-4282-a24c-d06adf5f1358)


---

## Backup and Restore 💾

1. **Backup Jenkins**:
Backup the following directories:
Jenkins Home: `/var/lib/jenkins` (default location).
Configuration files: `/etc/default/jenkins` (Debian-based systems).

Take note of all the security relevant objects (sensitive) that are in this directory:
![11](https://github.com/user-attachments/assets/033610f0-5f1e-48a8-b1aa-891db71a7895)

You should apply the following security configurations.

Set Ownership and Permissions: 
```bash 
chown -R jenkins:jenkins /var/lib/jenkins
chmod -R 700 /var/lib/jenkins
```

Sensitive Files (`secrets`, `identity.key.enc`, `secret.key`, `secret.key.not-so-secret`):
```bash
chmod 600 /var/lib/jenkins/secrets/* chmod 600 /var/lib/jenkins/identity.key.enc chmod 600 /var/lib/jenkins/secret.key chmod 600 /var/lib/jenkins/secret.key.not-so-secret
```

Job Data (`/var/lib/jenkins/jobs`):
```bash
chmod -R 700 /var/lib/jenkins/jobs
```

Logs (`/var/lib/jenkins/logs`):
```bash
chmod -R 700 /var/lib/jenkins/logs
```
`
Workspace (`/var/lib/jenkins/workspace`):
```bash
chmod -R 700 /var/lib/jenkins/workspace
```

Returning to the matter at hand- backing up important files:

Use `tar` for backups:
```bash
sudo tar -czvf jenkins-backup.tar.gz /var/lib/jenkins
```

2. **Restore Jenkins**:

Stop Jenkins:
```bash
sudo systemctl stop jenkins
```

Extract the backup archive:
```bash
sudo tar -xzvf jenkins-backup.tar.gz -C /var/lib/jenkins
```

Start Jenkins:
```bash
sudo systemctl start jenkins
```

3. **Automated Backups**:

Use the **ThinBackup** plugin for scheduled backups.

See documentation for ThinBackup plugin: https://plugins.jenkins.io/thinBackup/

Tutorial: https://www.youtube.com/watch?v=ZlfqLU3nUkE

---

## Monitoring Jenkins 📊

1. **Check Jenkins Logs**:
   - Default log location: `/var/log/jenkins/jenkins.log`.

View logs in real-time:
```bash
tail -f /var/log/jenkins/jenkins.log
```

2. **Monitor System Resources**:
   - Use tools like `top` or `htop` to monitor CPU and memory usage.
   - Navigate to **Manage Jenkins > System Information** for Jenkins-specific metrics.

![12](https://github.com/user-attachments/assets/68cc13b0-6050-4a53-90ec-b649487bc6fa)


3. **Set Up Alerts**:
   - Configure the **Monitoring** plugin to receive notifications about resource usage or failures.

See documentation for Monitoring plugin: https://plugins.jenkins.io/monitoring/

---

## Plugin Management 🧩

1. **Install Plugins**:
   - Go to **Manage Jenkins > Plugin Manager**.
   - Search for and install plugins from the **Available** tab.

2. **Update Plugins**:
   - Regularly check the **Updates** tab for new versions.
   - Install updates to keep Jenkins secure and functional.

3. **Remove Unused Plugins**:
   - Go to the **Installed** tab and uninstall plugins that are no longer needed.

4. **Recommended Plugins**:
   - Role-Based Authorization Strategy
   - ThinBackup
   - Git Plugin
   - Pipeline Plugin

---

## Tips for Effective Administration 💡

- **Regular Maintenance**:
  - Schedule downtime for updates and backups.
  - Clean up old builds and unused jobs.

- **Document Changes**:
  - Keep track of configurations, installed plugins, and updates.

- **Leverage Automation**:
  - Use scripts or tools like Ansible to automate routine tasks.

---

Once you’ve mastered these administrative tasks, explore advanced topics like configuring distributed builds, integrating with CI/CD pipelines, and scaling Jenkins for larger teams. 
