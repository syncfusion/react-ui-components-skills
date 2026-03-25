# License Key Registration

## Table of Contents
- [Overview](#overview)
- [Method 1: Register License in Project](#method-1-register-license-in-project)
- [Method 2: Register License Using NPX Command](#method-2-register-license-using-npx-command)
- [CI/CD Integration](#cicd-integration)

## Overview

The generated Syncfusion license key must be registered in your React application after any Syncfusion React reference. You can register the license key using one of two methods:

1. **Register in Project** - Add `registerLicense()` call directly in your code
2. **NPX Command** - Use command-line tool with license file or environment variable

**Important Notes:**
- License validation is performed **offline** during application execution
- No internet connection required after registration
- Apps can be deployed on any system without internet access
- License key registration required from v20.1.0.47+ (2022 Volume 1) onwards

## Method 1: Register License in Project

Register the license key directly in your `index.js` file by calling the `registerLicense()` method.

### Step 1: Open index.js

Navigate to your project's `src/index.js` file.

### Step 2: Import registerLicense

```javascript
import { registerLicense } from '@syncfusion/ej2-base';
```

### Step 3: Call registerLicense with Your Key

Add the license registration **after** importing `registerLicense` and **before** rendering your app:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import { registerLicense } from '@syncfusion/ej2-base';

// Registering Syncfusion license key
registerLicense('Replace your generated license key here');

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

**That's it!** Your license is now registered and the application will validate offline during execution.

## Method 2: Register License Using NPX Command

Register the Syncfusion license key using the `npx syncfusion-license activate` command. This method doesn't require code changes.

You can use the NPX command in two ways:
1. **With License File** - Create a text file containing your license key
2. **With Environment Variable** - Set a system environment variable

> **Note:** If both methods are used, the license file takes priority. Remove the license file to use the environment variable.

### Option A: Register with License File

**Step 1:** Create license file in project root

Create a file named `syncfusion-license.txt` in your application's root directory and paste your license key:

```text
Your_License_Key_Here
```

**Step 2:** Add file to .gitignore

```gitignore
syncfusion-license.txt
```

**Step 3:** Activate the license

Open command prompt in the application root directory and run:

```bash
npx syncfusion-license activate
```

**Success message:**
```
(INFO) Syncfusion License imported successfully.
```

**Step 4:** Clear cache

Remove the `.cache` folder from `node_modules`:

```bash
# Windows PowerShell
Remove-Item -Recurse -Force node_modules\.cache

# macOS/Linux
rm -rf node_modules/.cache
```


### Option B: Register with Environment Variable

You can set the `SYNCFUSION_LICENSE` environment variable on your system instead of using a license file.

**Step 1:** Set the environment variable

#### Windows

Open command prompt and use the `setx` command:

```cmd
setx SYNCFUSION_LICENSE "Your_License_Key_Here"
```

#### Mac

Open terminal and add to your bash profile:

```bash
echo 'export SYNCFUSION_LICENSE="Your_License_Key_Here"' >> ~/.bash_profile
```

To modify the environment variable:

```bash
nano ~/.bash_profile
```

Press `Ctrl+X` to exit, then `Y` and `Enter` to save changes.

Close and reopen terminal to see the changes using `env` command.

#### Linux

Open terminal and set the environment variable:

```bash
export SYNCFUSION_LICENSE='Your_License_Key_Here'
```

**Step 2:** Restart your IDE or terminal

After setting the environment variable, restart your IDE or application terminal.

**Step 3:** Activate the license

Open command prompt in the application root directory and run:

```bash
npx syncfusion-license activate
```

**Success message:**
```
(INFO) Syncfusion License imported successfully.
```

**Step 4:** Clear cache

Remove the `.cache` folder from `node_modules`:

```bash
# Windows PowerShell
Remove-Item -Recurse -Force node_modules\.cache

# macOS/Linux
rm -rf node_modules/.cache
```

## CI/CD Integration

### GitHub Actions

Use environment variables in GitHub Actions to register your license in CI pipelines.

**Step 1:** Create a Repository Secret

1. Go to your repository → **Settings** → **Secrets and variables** → **Actions**
2. Click **"New repository secret"**
3. Name: `SYNCFUSION_LICENSE`
4. Value: Your license key
5. Click **"Add secret"**

Or create an [Organization Secret](https://docs.github.com/en/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-an-organization) to share across repositories.

**Step 2:** Add activation step to your workflow

```yaml
steps:
  - name: Install node modules
    run: npm install

  - name: Activate Syncfusion License
    run: npx syncfusion-license activate
    env:
      SYNCFUSION_LICENSE: ${{ secrets.SYNCFUSION_LICENSE }}
```

**Complete Example:**

```yaml
name: Build React App

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
      
      - name: Install dependencies
        run: npm install
      
      - name: Activate Syncfusion License
        run: npx syncfusion-license activate
        env:
          SYNCFUSION_LICENSE: ${{ secrets.SYNCFUSION_LICENSE }}
      
      - name: Build application
        run: npm run build
```

### Azure Pipelines (YAML)

Use environment variables in Azure Pipelines to register your license.

**Step 1:** Create a User-defined Variable

1. Navigate to your pipeline → **Edit** → **Variables**
2. Click **"New variable"**
3. Name: `SYNCFUSION_LICENSE`
4. Value: Your license key
5. Check **"Keep this value secret"**

**Step 2:** Add activation step to your pipeline

**For Windows build agents:**

```yaml
pool:
  vmImage: 'windows-latest'

steps:
  - script: call npm install
    displayName: 'Install node modules'

  - script: call npx syncfusion-license activate
    displayName: 'Activate Syncfusion License'
    env:
      SYNCFUSION_LICENSE: $(SYNCFUSION_LICENSE)
```

**For Linux build agents:**

```yaml
pool:
  vmImage: 'ubuntu-latest'

steps:
  - script: npm install
    displayName: 'Install node modules'

  - script: npx syncfusion-license activate
    displayName: 'Activate Syncfusion License'
    env:
      SYNCFUSION_LICENSE: $(SYNCFUSION_LICENSE)
```

### Azure Pipelines (Classic)

For Classic Editor users:

**Step 1:** Create a User-defined Variable named `SYNCFUSION_LICENSE` with your license key as the value.

**Step 2:** Add a Bash task after npm install with the following script:

```bash
# Activate the license
npx syncfusion-license activate
```

