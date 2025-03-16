# GitHub Actions CI/CD Pipeline Setup

## Introduction

This guide explains the steps to set up a CI/CD pipeline using GitHub Actions for a Node.js application. This process involves installing necessary tools, setting up a GitHub Actions workflow, writing tests using Jest, and debugging common issues along the way.

## Prerequisites

Before starting, make sure you have the following installed:

- **Git**: To clone repositories and push changes.
- **Node.js**: To run JavaScript code and handle dependencies.
- **npm**: For managing Node.js packages and dependencies.
- **GitHub Account**: To push code to GitHub and set up GitHub Actions.

## Step-by-Step Guide

### 1. Set up the Node.js Application

1. **Create a new directory for your project**:

    ```bash
    mkdir github-actions-demo
    cd github-actions-demo
    ```

2. **Initialize a new Node.js project**:

    ```bash
    npm init -y
    ```

    This will generate a `package.json` file.

3. **Install Jest for testing**:

    ```bash
    npm install --save-dev jest
    ```

4. **Create a simple test file `index.test.js`**:

    ```javascript
    test('Check if script runs', () => {
        expect(true).toBe(true);
    });
    ```

    Save this file in your project directory.

### 2. Set up GitHub Repository

1. **Create a GitHub repository**:

    Go to GitHub and create a new repository, for example, `github-actions-demo`.

2. **Push your code to GitHub**:

    ```bash
    git init
    git remote add origin https://github.com/your-username/github-actions-demo.git
    git add .
    git commit -m "Initial commit with Jest test"
    git push -u origin main
    ```

### 3. Set up GitHub Actions Workflow

1. **Create GitHub Actions folder**:

    Create the directory `.github/workflows` in your project:

    ```bash
    mkdir -p .github/workflows
    ```

2. **Create a GitHub Actions workflow YAML file**:

    Create a new file `.github/workflows/ci.yml` with the following contents:

    ```yaml
    name: CI Pipeline

    on:
      push:
        branches:
          - main
      pull_request:
        branches:
          - main

    jobs:
      build-and-test:
        runs-on: ubuntu-latest

        steps:
          - name: Checkout Code
            uses: actions/checkout@v4

          - name: Set up Node.js
            uses: actions/setup-node@v4
            with:
              node-version: '20'

          - name: Install Dependencies
            run: npm install

          - name: Run Tests
            run: npm test
    ```

3. **Commit and push the changes**:

    ```bash
    git add .github/
    git commit -m "Add GitHub Actions CI pipeline"
    git push origin main
    ```

### 4. Run the GitHub Actions Workflow

Once you push your code to GitHub, GitHub Actions will automatically run the workflow as defined in the `ci.yml` file.

- Go to your GitHub repository → **Actions** tab to see the status of the pipeline.

## Debugging Common Issues

### 1. "Permission denied" when running Jest tests

**Problem**: You may see an error like `sh: 1: jest: Permission denied.`

**Solution**:

- Make sure Jest is installed correctly. You can install it globally as a workaround:

    ```bash
    npm install -g jest
    ```

- Alternatively, ensure that `node_modules/.bin` is included in the path where Jest is executed by specifying it directly in the run section of GitHub Actions:

    ```yaml
    - name: Run Tests
      run: ./node_modules/.bin/jest
    ```

### 2. "npm install" fails in GitHub Actions

**Problem**: The `npm install` command might fail because some dependencies need elevated permissions.

**Solution**:

- You can add the `sudo` command to fix permissions issues in the GitHub Actions workflow:

    ```yaml
    - name: Install Dependencies
      run: sudo npm install
    ```

### 3. "No tests found" error

**Problem**: If Jest is not finding any tests, you might see `No tests found`.

**Solution**:

- Make sure your test file has the correct naming convention (`*.test.js`) and is placed in the root of your project or in a directory where Jest can find it.

- Double-check the `package.json` file to ensure the test script is correct:

    ```json
    "scripts": {
      "test": "jest"
    }
    ```

## Final Thoughts

Congratulations! You have successfully set up a CI/CD pipeline with GitHub Actions for your Node.js application. This workflow ensures that every time you push code to GitHub, the application will automatically run tests and check if the changes are valid.

Feel free to modify the workflow for different environments, integrate more tools, or expand with custom actions based on your project needs.

If you face any issues or have further questions, feel free to open an issue in the repository!

Happy coding!
