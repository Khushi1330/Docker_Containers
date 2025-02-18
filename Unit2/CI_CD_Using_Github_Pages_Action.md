# CI/CD using GitHub Pages and GitHub Actions with a Portfolio as an Example

Continuous Integration (CI) and Continuous Deployment (CD) are important aspects of modern software development, allowing teams to automatically build, test, and deploy code. GitHub Pages and GitHub Actions provide an integrated platform for implementing these practices. Let’s go through a detailed example using a **personal portfolio** website hosted on **GitHub Pages** with **GitHub Actions** to automate the process.

### **Overview of CI/CD with GitHub Pages and GitHub Actions**
- **CI (Continuous Integration)**: Automatically build and test code when changes are pushed to a repository. 
- **CD (Continuous Deployment)**: Automatically deploy changes to a production environment once code passes CI tests.

#### **Tools Used**
- **GitHub Pages**: A service that allows you to host static websites directly from a GitHub repository.
- **GitHub Actions**: An automation service to define workflows for CI/CD tasks in GitHub repositories.

---

### **Step-by-Step Example: Setting up CI/CD for a Portfolio using GitHub Pages**

### 1. **Create the Portfolio Repository**
First, create a new repository on GitHub to store your portfolio.

- Go to GitHub and create a new repository named `portfolio`.
- Initialize it with a `README.md` file and a `.gitignore` (choose Node or None depending on your setup).
- Clone the repository locally and create your portfolio.

---

### 2. **Build Your Portfolio (Static Website)**

For the sake of this example, assume your portfolio is a simple HTML, CSS, and JavaScript project. The structure might look like this:

```
portfolio/
├── index.html
├── style.css
├── app.js
├── images/
│   ├── profile.jpg
│   └── project1.jpg
└── README.md
```

You can add a simple HTML structure for the portfolio, for example:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Portfolio</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>Welcome to My Portfolio</h1>
    </header>
    <section>
        <h2>About Me</h2>
        <p>Hi, I am [Your Name], a web developer.</p>
    </section>
    <section>
        <h2>Projects</h2>
        <ul>
            <li><a href="project1.html">Project 1</a></li>
            <li><a href="project2.html">Project 2</a></li>
        </ul>
    </section>
    <footer>
        <p>&copy; 2025 My Portfolio</p>
    </footer>
    <script src="app.js"></script>
</body>
</html>
```

Add styles and interactivity to your portfolio as needed.

---

### 3. **Push Your Portfolio Code to GitHub**
Once your portfolio is ready, push it to your GitHub repository.

```bash
git init
git remote add origin https://github.com/username/portfolio.git
git add .
git commit -m "Initial portfolio commit"
git push -u origin master
```

### 4. **Enable GitHub Pages for the Repository**
Now, configure **GitHub Pages** to serve your portfolio.

- Go to your repository on GitHub.
- Click on **Settings** -> **Pages**.
- In the **Source** section, choose **main** (or **master** branch), then select the `/ (root)` folder as the source.
- GitHub will generate a URL (e.g., `https://username.github.io/portfolio`) where your portfolio will be live.

---

### 5. **Set Up GitHub Actions for CI/CD**

Now, we need to set up **GitHub Actions** to automate the CI/CD process.

#### **a. Create GitHub Actions Workflow**
Create a new folder `.github/workflows/` in your repository and add a new YAML file `deploy.yml`. This file will define the GitHub Actions workflow.

Here’s an example of what the workflow could look like:

```yaml
name: CI/CD for Portfolio

on:
  push:
    branches:
      - main
      - master

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up Node.js (if your project requires it)
      uses: actions/setup-node@v2
      with:
        node-version: '14'

    - name: Install dependencies
      run: |
        npm install

    - name: Build the project (if needed)
      run: |
        npm run build  # Adjust if using a build tool or framework

    - name: Deploy to GitHub Pages
      uses: JamesIves/github-pages-deploy-action@v4
      with:
        branch: gh-pages  # The branch where the static site will be deployed
        folder: .  # Path to the folder to deploy (can be 'build' or root)
        token: ${{ secrets.GITHUB_TOKEN }}  # GitHub automatically provides this token

    - name: Notify deployment success (optional)
      run: echo "Portfolio deployed successfully!"
```

#### **Explanation of Workflow:**
- **Trigger**: The workflow is triggered on `push` events to the `main` or `master` branches.
- **Jobs**: 
  - `build-and-deploy` job runs on an Ubuntu virtual environment.
  - **Checkout code**: Uses the `actions/checkout` action to pull the latest code.
  - **Set up Node.js** (optional, if your project uses Node.js for building): Installs a specific version of Node.js.
  - **Install dependencies**: Installs necessary dependencies (e.g., `npm install` for Node.js projects).
  - **Build the project**: Runs a build script if necessary. This is often needed if you're using a front-end framework (React, Vue.js, etc.) that needs to be built into static files.
  - **Deploy to GitHub Pages**: The `github-pages-deploy-action` action deploys your files to the `gh-pages` branch of the repository, which is where GitHub Pages serves your site from.

#### **b. Add GitHub Token**
- The `GITHUB_TOKEN` used in the workflow is automatically provided by GitHub and allows the action to authenticate and deploy to your repository.

---

### 6. **Push Changes and Trigger CI/CD Workflow**
Whenever you make changes to your portfolio, you simply commit and push them to the `main` or `master` branch:

```bash
git add .
git commit -m "Updated portfolio with new project"
git push origin main
```

This will trigger the **GitHub Actions workflow** defined in `deploy.yml`. GitHub Actions will:
1. Build your project (if applicable).
2. Deploy it to the `gh-pages` branch.
3. Automatically update the portfolio website hosted on GitHub Pages.

### 7. **Check the Deployment**
Once the workflow completes successfully, visit your **GitHub Pages URL** (e.g., `https://username.github.io/portfolio`) to see the updated portfolio.

---

### **Benefits of CI/CD with GitHub Pages and GitHub Actions**

1. **Automated Deployments**: Each time you push a change to the `main` branch, the workflow automatically builds and deploys your portfolio to GitHub Pages.
2. **Version Control**: All changes are tracked in your GitHub repository, and any previous version of your site can be accessed through Git history.
3. **No Manual Steps**: With GitHub Actions, you eliminate the need for manual deployment. Everything is automated when you push code.
4. **Scalable**: As your portfolio grows, you can easily extend the CI/CD pipeline to handle more complex tasks, such as automated testing, linting, or integrating with external APIs.

---

### **Conclusion**

With **GitHub Pages** for hosting and **GitHub Actions** for automation, you can implement a simple yet powerful CI/CD pipeline for deploying a personal portfolio. Every time you push changes to your GitHub repository, the automated workflow will ensure your portfolio is always up to date with minimal manual intervention. This approach can be scaled to more complex websites or projects as needed.

By leveraging **GitHub Pages** and **GitHub Actions**, you benefit from a seamless integration between your version control, CI/CD, and deployment process, making it ideal for personal projects, portfolios, and even small to medium-scale applications.