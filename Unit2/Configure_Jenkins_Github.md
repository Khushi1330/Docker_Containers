# Jenkins on a Local Windows Machine for CI/CD with GitHub

To set up Jenkins for Continuous Integration and Continuous Deployment (CI/CD) with GitHub on a local Windows machine, follow the steps below.

#### **Prerequisites:**
1. A **Windows machine** (with administrative privileges).
2. **Git** installed on your machine (required for Jenkins to interact with GitHub repositories).
3. A **GitHub repository** that will be used to trigger Jenkins builds.
4. **Jenkins** installation.

---

### **Step 1: Install Jenkins on Windows**

1. **Download Jenkins:**
   - Go to the official Jenkins website: https://www.jenkins.io/download/
   - Download the **Windows** installer (`.war` or `.msi` file).

2. **Install Jenkins:**
   - Run the installer file (`jenkins.msi`).
   - Follow the installation instructions, ensuring that Jenkins is installed as a service (which will allow it to run in the background).
   - During installation, Jenkins will be set up to run on the default port: `8080`.

3. **Start Jenkins:**
   - Once installation is complete, Jenkins should automatically start running.
   - Open your browser and visit `http://localhost:8080` to access the Jenkins dashboard.

4. **Unlock Jenkins:**
   - During the initial setup, Jenkins will prompt for a **unlock key**.
   - Find this key by navigating to the file `C:\Program Files (x86)\Jenkins\secrets\initialAdminPassword` (or wherever Jenkins was installed).
   - Open the file and copy the password.
   - Paste it in the Jenkins setup page to proceed.

5. **Install Suggested Plugins:**
   - Once unlocked, Jenkins will ask if you want to install the suggested plugins.
   - Choose "Install suggested plugins" to proceed with the installation of necessary plugins for CI/CD.

6. **Create Jenkins Admin User:**
   - After installing plugins, Jenkins will prompt you to create an admin user. Fill in the details (username, password, etc.).

---

### **Step 2: Install Required Plugins in Jenkins**

To set up Jenkins for interacting with GitHub and Git repositories, you need to install a few plugins.

1. **Navigate to Jenkins Dashboard:**
   - Go to `http://localhost:8080` (Jenkins homepage).

2. **Install Git Plugin:**
   - Go to **Manage Jenkins** > **Manage Plugins**.
   - In the **Available** tab, search for **Git Plugin** and click **Install**.

3. **Install GitHub Plugin:**
   - Go to **Manage Jenkins** > **Manage Plugins**.
   - In the **Available** tab, search for **GitHub Plugin** and click **Install**.

4. **Install Pipeline Plugin (Optional for CI/CD Pipelines):**
   - In the **Available** tab, search for **Pipeline** and install it.

---

### **Step 3: Configure GitHub for Jenkins Integration**

You need to connect Jenkins with GitHub for it to be able to access the GitHub repository and pull code.

1. **Generate GitHub Personal Access Token:**
   - Go to [GitHub Developer Settings](https://github.com/settings/tokens).
   - Generate a **Personal Access Token** with the necessary permissions (e.g., **repo**, **workflow**).
   - Keep the token handy as you'll need it for Jenkins configuration.

2. **Configure GitHub Credentials in Jenkins:**
   - Go to **Manage Jenkins** > **Manage Credentials** > **(global)** > **Add Credentials**.
   - Choose **Kind**: `GitHub Personal Access Token`.
   - **Username**: Use your GitHub username.
   - **Password**: Paste the Personal Access Token you generated earlier.
   - Give it an ID (e.g., `github-token`) and save it.

---

### **Step 4: Create a New Jenkins Job for CI/CD**

1. **Create a New Job:**
   - From the Jenkins dashboard, click on **New Item**.
   - Give the job a name (e.g., `my-project-ci-cd`).
   - Select **Freestyle project** (for simplicity) or **Pipeline** (if you're using a Jenkinsfile).
   - Click **OK**.

2. **Configure GitHub Repository in Jenkins Job:**
   - Under **Source Code Management**, select **Git**.
   - In **Repository URL**, enter the **GitHub repository URL** (e.g., `https://github.com/username/my-project.git`).
   - Under **Credentials**, select the GitHub credentials you added earlier (`github-token`).
   - In the **Branches to build**, enter `*/main` (or whichever branch you want Jenkins to build).

3. **Set Build Triggers (GitHub Webhook for CI/CD):**
   - Under **Build Triggers**, select **GitHub hook trigger for GITScm polling**.
   - This will allow Jenkins to listen for webhook events from GitHub, triggering a build whenever there’s a change in the repository.

4. **Configure Build Steps:**
   - Under **Build**, you can define your build steps. If you're using Docker, for example, you can add shell commands to build Docker images, run tests, or deploy the application.
   - Example for a simple **Shell Command**:
     - `echo "Building Project"`
     - `mvn clean install` (for Java projects) or any appropriate build command for your project.

5. **Configure Post-build Actions (Optional for CI/CD):**
   - You can define actions like sending notifications (e.g., email or Slack) or deploying to a server after the build is successful.

---

### **Step 5: Set Up GitHub Webhook for Automatic Build Trigger**

1. **Go to GitHub Repository Settings:**
   - Navigate to your GitHub repository.
   - Click on **Settings** > **Webhooks** > **Add webhook**.

2. **Configure Webhook:**
   - **Payload URL**: `http://<JENKINS_HOST>:8080/github-webhook/` (replace `<JENKINS_HOST>` with your local IP address or `localhost` if running locally).
   - **Content type**: `application/json`.
   - **Which events would you like to trigger this webhook?**: Select **Just the push event** (or any other events that you want to trigger the build).
   - Click **Add webhook**.

---

### **Step 6: Test the CI/CD Pipeline**

1. **Push Changes to GitHub:**
   - Make a change to the code in your GitHub repository (e.g., update a README or push a new feature).
   - When you push the changes, GitHub will automatically send a webhook to Jenkins.

2. **Monitor Jenkins Build:**
   - After the push, Jenkins should trigger a new build.
   - Go to Jenkins Dashboard and select your job to monitor the build progress.

3. **Check Build Results:**
   - Jenkins will show if the build was successful or if there were errors.
   - If configured, Jenkins will deploy your app or perform other post-build actions.

---

### **Step 7: Optional - Set Up Deployment Steps**

To fully automate the CI/CD pipeline, you can add deployment steps to your Jenkins pipeline:

1. **For Docker-based deployment**:
   - After building your application, you can configure Jenkins to build and push Docker images.
   - Example **Post-build action** (for Docker):
     ```bash
     docker build -t myapp:latest .
     docker run -d -p 8080:8080 myapp:latest
     ```

2. **For Kubernetes Deployment**:
   - Use the Kubernetes plugin for Jenkins to deploy the application to a Kubernetes cluster once the build is successful.

---

### **Summary**

1. **Install Jenkins** on your Windows machine and set it up to run as a service.
2. **Install required plugins** for Git, GitHub, and any other CI/CD tools.
3. **Configure GitHub** credentials and webhook to link Jenkins with your GitHub repository.
4. **Create a Jenkins job** for your project and configure the source code management and build steps.
5. **Set up GitHub webhooks** to trigger Jenkins builds automatically on changes in the repository.
6. **Test and deploy** by pushing changes to your GitHub repo and checking the Jenkins build logs for success or failure.

By following these steps, you will have Jenkins set up for CI/CD on your local machine, integrated with GitHub for automatic builds and deployments.