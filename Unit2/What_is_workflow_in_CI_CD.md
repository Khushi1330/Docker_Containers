# CI/CD Workflow Explained with an Example

A **CI/CD workflow** refers to a series of automated processes that help teams integrate code changes and deploy them to production in a consistent and efficient manner. **CI (Continuous Integration)** involves automatically integrating code changes from multiple contributors into a shared repository multiple times a day. **CD (Continuous Deployment)** or **Continuous Delivery** focuses on automatically deploying these changes to production or staging environments after they pass the CI checks.

In this explanation, we will go through a **CI/CD workflow** using GitHub Actions as the automation tool, along with an example of a simple **Node.js** application.

---

### **CI/CD Workflow Steps**

Here’s a simplified workflow for CI/CD:

1. **Code Commit**:
   Developers write code and push their changes to the repository.

2. **Continuous Integration (CI)**:
   - The CI process is triggered when a developer pushes code to the repository.
   - Automated tests are run to ensure that the new code does not break any existing functionality.
   - If tests pass, the code is integrated into the shared repository.

3. **Continuous Deployment (CD)**:
   - After the CI step, the code is deployed automatically to a staging or production environment.
   - For **Continuous Delivery**, the code may be deployed to staging automatically, but manual approval is required for production.
   - For **Continuous Deployment**, the code is automatically deployed to production after passing the tests.

---

### **Example: CI/CD Workflow for a Node.js Application**

Let’s walk through a practical example of setting up a **CI/CD workflow** using **GitHub Actions** for a simple **Node.js** application.

#### **1. Setting up the Project**

Assume we have a Node.js app with the following structure:

```
my-app/
├── node_modules/
├── src/
│   └── app.js
├── test/
│   └── app.test.js
├── package.json
├── .gitignore
└── README.md
```

- **`src/app.js`**: The main file for the application.
- **`test/app.test.js`**: Contains unit tests for the application.
- **`package.json`**: Contains the dependencies and scripts for the project.

Example of **`app.js`**:

```javascript
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.listen(port, () => {
  console.log(`App listening at http://localhost:${port}`);
});

module.exports = app; // For testing purposes
```

Example of **`app.test.js`** (using **Jest** for testing):

```javascript
const request = require('supertest');
const app = require('../src/app');

describe('GET /', () => {
  it('should return Hello, World!', async () => {
    const response = await request(app).get('/');
    expect(response.text).toBe('Hello, World!');
    expect(response.status).toBe(200);
  });
});
```

---

#### **2. Setting Up GitHub Actions**

Now let’s set up a **GitHub Actions workflow** to automate the CI/CD pipeline.

##### **Create the GitHub Actions Workflow File**

Create the following folder structure in your repository: 

```
.github/
└── workflows/
    └── ci-cd.yml
```

##### **`ci-cd.yml` Configuration**

Here’s an example GitHub Actions configuration to automate the CI/CD workflow for the Node.js app:

```yaml
name: Node.js CI/CD

# Trigger the workflow on push and pull request events
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    # Steps for the build job
    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test

  deploy:
    runs-on: ubuntu-latest
    needs: build  # This ensures that deployment only happens after the build job finishes successfully
    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '14'

    - name: Install dependencies
      run: npm install

    - name: Deploy to Production
      run: |
        # Add deployment steps here, e.g., FTP upload, Docker push, or cloud deployment
        echo "Deploying application to production..."
        # For demonstration, we echo a success message
```

#### **Explanation of the Workflow:**

- **Trigger**: The workflow runs on `push` or `pull_request` events to the `main` branch.
- **Jobs**: We have two jobs: `build` and `deploy`.

  1. **Build Job**:
     - **Checkout Code**: The first step checks out the latest code from the repository.
     - **Setup Node.js**: Sets up the required version of Node.js (in this case, version `14`).
     - **Install Dependencies**: Installs the dependencies listed in `package.json` using `npm install`.
     - **Run Tests**: Runs the test suite using `npm test` (which is typically defined in the `package.json` file, for instance, using **Jest**).

  2. **Deploy Job**:
     - The deploy job only runs if the `build` job is successful (due to the `needs: build` line).
     - **Deploy to Production**: This step would be customized to deploy your application to a production environment, such as using FTP, cloud services like AWS, or a Docker container.

---

#### **3. Setting Up NPM Scripts**

Make sure you have a `test` script in your `package.json` to run the tests. Here’s how your `package.json` might look:

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "description": "My Node.js Application",
  "main": "src/app.js",
  "scripts": {
    "test": "jest",
    "start": "node src/app.js"
  },
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "jest": "^26.6.3",
    "supertest": "^6.1.3"
  },
  "author": "Your Name",
  "license": "MIT"
}
```

---

#### **4. Pushing Code and Triggering CI/CD**

1. **Push to GitHub**: After setting up your GitHub Actions workflow, push your changes to the `main` branch.

   ```bash
   git add .
   git commit -m "Add GitHub Actions CI/CD workflow"
   git push origin main
   ```

2. **GitHub Actions Trigger**: This will trigger the GitHub Actions pipeline:
   - The **build** job will run and install dependencies, followed by running the tests.
   - If the tests pass, the **deploy** job will run, which can deploy the application to the production environment.

3. **Monitor the Actions**: On GitHub, go to the **Actions** tab of your repository to monitor the progress of the workflow. You can see logs for each step and debug if any step fails.

---

### **Summary of CI/CD Workflow**

Here’s how the entire process flows in the CI/CD pipeline for this Node.js application:

1. **Code Commit**: A developer pushes code changes to the `main` branch of the GitHub repository.
   
2. **CI (Continuous Integration)**:
   - The workflow is triggered by the push event.
   - The code is checked out, dependencies are installed, and tests are run.
   - If the tests pass, the workflow proceeds to the deployment step.

3. **CD (Continuous Deployment)**:
   - The `deploy` job runs only if the `build` job completes successfully.
   - The application is deployed to production (this step can be configured based on your preferred deployment method).

---

### **Benefits of CI/CD in this Example**

- **Automated Testing**: The workflow ensures that every change to the code is tested automatically before being merged or deployed.
- **Faster Deployment**: Once code passes the tests, it is automatically deployed without manual intervention, reducing human error and increasing deployment speed.
- **Consistent Environment**: By using GitHub Actions, the process is repeatable and ensures that deployment happens in the same environment each time.
- **Scalability**: As the project grows, you can add more steps to the pipeline (e.g., linting, building Docker containers, deploying to multiple environments).

This is a basic example of how CI/CD works in practice using GitHub Actions for a simple Node.js application.