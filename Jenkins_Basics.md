

# Jenkins Basics Guide 🛠️

Welcome to the Jenkins Basics Guide! This tutorial covers the core functionality of Jenkins and how to create your first job to automate tasks.

---

## What Are Jenkins Jobs? 🤔

In Jenkins, a **job** is a task or series of tasks executed by Jenkins. For example:
- Building software.
- Running automated tests.
- Deploying applications to production.

Jenkins jobs are the building blocks of continuous integration and delivery pipelines.

---

## Step 1: Accessing the Dashboard 🖥️

1. Open your web browser and go to:

```bash
http://your-server-ip:8080
```

For local installations:
```bash
http://localhost:8080
```

2. Log in with your admin credentials.

3. You’ll see the Jenkins Dashboard, which is the central hub for managing your jobs, builds, and system settings.

---

## Step 2: Creating Your First Job 🎉

Follow these steps to create a basic Jenkins job:

1. **Click "New Item"**:  
- Located on the left-hand menu.

![1](https://github.com/user-attachments/assets/c62d54fb-58df-4a42-9d04-de6fc70a7df5)


2. **Name Your Job**:  
- Enter a meaningful name (e.g., `My_First_Job`).  


3. **Select a Job Type**:  
- Choose **Freestyle Project** for simple tasks.  
- Click **OK** to proceed.

![2](https://github.com/user-attachments/assets/100ee812-53fd-48e8-9877-096cc661b600)

4. **Configure the Job**:  
- In the **General** section, add a description of the job.  
- Go to the **Build** section, and choose **Execute Shell** (for Linux) or **Execute Windows Batch Command**.  

![3](https://github.com/user-attachments/assets/13ce2a5c-6113-4368-8d82-e91d3c765d5f)

5. **Add Build Commands**:  
Example for Linux:  
```bash
echo "Hello, Jenkins!"
```

Example for Windows:
```bash
echo Hello, Jenkins!
```

6. **Save and Apply**:
    - Click **Save** to store the job configuration.

---

## Step 3: Running Your Job 🚀

1. Go back to the Jenkins Dashboard.
2. Locate your job in the list.
3. Click the **Build Now** button in the left-hand menu.

![4](https://github.com/user-attachments/assets/523e6c1a-581a-4c50-8dd0-8e6a9d6a3a92)

4. Monitor progress:
    - Navigate to **Build History** on the left side to see past and ongoing builds.
    - Click on a specific build to view details and console output.

![5](https://github.com/user-attachments/assets/8deb1572-d5b2-41dc-8357-a23941b7ee0c)

---

## Step 4: Understanding Job Outputs 📊

1. **Console Output**:
    
    - After running the job, click **Console Output** for logs.
    - Check for messages like `Hello, Jenkins!` to verify the job ran successfully.
2. **Status Indicators**:
    - ✅ Blue (or green): Build succeeded.
    - ❌ Red: Build failed.
    - ⚪ Grey: Build didn’t run.

![6](https://github.com/user-attachments/assets/eed26bf7-6ae9-467a-96c7-b408bb82ef8e)

---

## Tips for Effective Jenkins Use 💡

1. **Keep Descriptions Clear**:
    
    - Document each job's purpose in the description field.
2. **Organize Your Jobs**:
    
    - Use folders or naming conventions to keep jobs manageable.
3. **Regular Backups**:
    
    - Back up your Jenkins configurations to avoid losing data.
4. **Explore Plugins**:
    
    - Extend Jenkins functionality by adding plugins for Git, Docker, and more.

---
