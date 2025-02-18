# Docker File Examples for Popular Front End frameworks

### 1. **React.js (using create-react-app)**

Assuming you created your React app using `create-react-app`, the Dockerfile would look like this:

**Dockerfile for React:**

```dockerfile
# Use the official Node.js image as a base image
FROM node:16-alpine

# Set the working directory in the container
WORKDIR /app

# Copy the package.json and package-lock.json files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the app's source code
COPY . .

# Build the React app
RUN npm run build

# Use a lightweight web server to serve the app
RUN npm install -g serve

# Expose the port that the app will run on
EXPOSE 3000

# Command to run the app
CMD ["serve", "-s", "build", "-l", "3000"]
```

This Dockerfile:
- Uses Node.js to install dependencies.
- Builds the React app using `npm run build`.
- Uses `serve` to serve the static build files.

### 2. **Vue.js**

For a Vue.js project (created with Vue CLI), the Dockerfile would be:

**Dockerfile for Vue:**

```dockerfile
# Use Node.js as the base image
FROM node:16-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json for installing dependencies
COPY package*.json ./

# Install project dependencies
RUN npm install

# Copy the entire source code into the container
COPY . .

# Build the Vue app
RUN npm run build

# Install a lightweight web server to serve the app
RUN npm install -g serve

# Expose the port the app will run on
EXPOSE 8080

# Command to run the app
CMD ["serve", "-s", "dist", "-l", "8080"]
```

This Dockerfile:
- Installs dependencies with npm.
- Builds the Vue project with `npm run build`.
- Uses `serve` to serve the built app.

### 3. **Angular**

For an Angular project, you'd use a Dockerfile like this:

**Dockerfile for Angular:**

```dockerfile
# Use the official Node.js image
FROM node:16-alpine

# Set the working directory
WORKDIR /app

# Copy the package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the Angular source code
COPY . .

# Build the Angular app
RUN npm run build --prod

# Install a lightweight server
RUN npm install -g http-server

# Expose port 8080 for the server
EXPOSE 8080

# Command to serve the app
CMD ["http-server", "dist/your-app-name", "-p", "8080"]
```

This Dockerfile:
- Installs Angular dependencies.
- Builds the app in production mode.
- Uses `http-server` to serve the app on port `8080`.

Make sure to replace `"your-app-name"` with the actual name of the Angular app directory in the `dist` folder.

### 4. **Svelte**

For a Svelte project, this is how you can create a Dockerfile:

**Dockerfile for Svelte:**

```dockerfile
# Use the official Node.js image
FROM node:16-alpine

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the source code
COPY . .

# Build the Svelte app
RUN npm run build

# Install a lightweight server
RUN npm install -g serve

# Expose the port the app will run on
EXPOSE 5000

# Command to serve the app
CMD ["serve", "-s", "public", "-l", "5000"]
```

This Dockerfile:
- Installs dependencies for Svelte.
- Builds the app.
- Serves the static files with `serve` on port `5000`.

### 5. **Ember.js**

For an Ember project, use the following Dockerfile:

**Dockerfile for Ember:**

```dockerfile
# Use the official Node.js image
FROM node:16-alpine

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install Ember CLI globally
RUN npm install -g ember-cli

# Install dependencies
RUN npm install

# Copy the source code
COPY . .

# Build the Ember app
RUN ember build --environment=production

# Install a lightweight web server to serve the app
RUN npm install -g http-server

# Expose port 4200 for the Ember app
EXPOSE 4200

# Command to serve the app
CMD ["http-server", "dist", "-p", "4200"]
```

This Dockerfile:
- Installs Ember and its dependencies.
- Builds the app in production mode.
- Uses `http-server` to serve the app on port `4200`.

### 6. **Preact**

For a Preact project, use the following Dockerfile:

**Dockerfile for Preact:**

```dockerfile
# Use Node.js as the base image
FROM node:16-alpine

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the source code
COPY . .

# Build the Preact app
RUN npm run build

# Install a lightweight web server
RUN npm install -g serve

# Expose port 8080
EXPOSE 8080

# Command to serve the app
CMD ["serve", "-s", "build", "-l", "8080"]
```

This Dockerfile:
- Installs dependencies.
- Builds the Preact app.
- Uses `serve` to serve the app on port `8080`.

### 7. **Backbone.js**

For a Backbone.js app (assuming it's an app served by a basic web server), you can use the following Dockerfile:

**Dockerfile for Backbone.js:**

```dockerfile
# Use Node.js as the base image
FROM node:16-alpine

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the source code
COPY . .

# Build the Backbone.js app (if there's a build step)
RUN npm run build  # Only if you have a build step; otherwise, this can be skipped

# Install a lightweight web server
RUN npm install -g http-server

# Expose port 8080
EXPOSE 8080

# Command to serve the app
CMD ["http-server", "public", "-p", "8080"]
```

This Dockerfile:
- Installs Backbone.js dependencies.
- Uses `http-server` to serve the app from the `public` directory.

### 8. **Alpine.js**

Alpine.js is often used directly in the HTML file without needing a build step, so here's a simple Dockerfile for serving a static site with Alpine.js:

**Dockerfile for Alpine.js:**

```dockerfile
# Use a simple Nginx image to serve the static site
FROM nginx:alpine

# Copy the static site files (HTML, JS, CSS, etc.)
COPY ./public /usr/share/nginx/html

# Expose port 80 for the app
EXPOSE 80

# Nginx is the default command for this image, no CMD needed
```

This Dockerfile:
- Uses Nginx to serve static files that include Alpine.js.
- Exposes port `80`.

### 9. **Mithril.js**

For a Mithril.js app, you can use the following Dockerfile:

**Dockerfile for Mithril.js:**

```dockerfile
# Use Node.js as the base image
FROM node:16-alpine

# Set the working directory
WORKDIR /app

# Copy package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the source code
COPY . .

# Build the Mithril app
RUN npm run build  # Only if you have a build step

# Install a lightweight web server
RUN npm install -g http-server

# Expose port 8080
EXPOSE 8080

# Command to serve the app
CMD ["http-server", "dist", "-p", "8080"]
```

This Dockerfile:
- Builds the Mithril app.
- Uses `http-server` to serve the app on port `8080`.

---

### Summary

Each of the above Dockerfiles provides a way to build and serve a front-end framework (React, Vue, Angular, Svelte, Ember, Preact, Backbone, Alpine, or Mithril) using Docker. For frameworks that require building (like React, Vue, Angular, etc.), these Dockerfiles install dependencies, build the app, and then serve it using a simple static server (`serve` or `http-server`).

You can adapt these Dockerfiles depending on your specific project structure or configuration.