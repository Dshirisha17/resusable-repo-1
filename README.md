# resusable-repo-1
  This repo are used for working on example on the concept of reusable workflows

# Workflow Overview
  This repository contains multiple GitHub Actions workflows that are interconnected, enabling the passing of input messages between different workflows for processing. The workflows are designed to be reusable 
  and can be triggered manually or by other workflows. Below is a breakdown of each workflow and its role in the overall setup.

# Workflows in the Repository
## Workflow A (workflow-a.yml)
####  Trigger: Manual trigger via workflow_dispatch.
####  Inputs: Accepts an input_message from the user.
####  Action: This workflow triggers Workflow B, passing the input_message as an input.
## Workflow B (workflow-b.yml)
####  Trigger: Triggered by another workflow using workflow_call.
####  Inputs: Accepts an input_message from the calling workflow (e.g., Workflow A).
####  Action: This workflow processes the input_message. In the example provided, it runs on a Windows environment and simply outputs the message.
## Workflow C (workflow-c.yml)
####  Trigger: Triggered by Workflow B from a separate repository (reusable-repo-2).
####  Inputs: Accepts an input_message passed from Workflow B.
#### Action: This workflow processes the input_message on a Windows machine and outputs the message for logging or further use.
## Main A (main-a.yml)
####  Trigger: Manual trigger via workflow_dispatch.
####  Inputs: Accepts an input_message from the user.
####  Action: This workflow triggers Main B, passing the input_message to it.
## Main B (main-b.yml)
#### Trigger: Triggered by Main A using workflow_call.
#### Inputs: Accepts an input_message from Main A.
#### Action: This workflow triggers Main C, passing the input_message to it.
## Main C (main-c.yml)
####  Trigger: Triggered by Main B using workflow_call.
####  Inputs: Accepts an input_message passed from Main B.
####  Action: This workflow processes the input_message on a Windows machine and outputs the message for logging or further processing.

# How to Trigger Workflows
  Manual Trigger: Some workflows (e.g., Workflow A, Main A) can be triggered manually via the GitHub UI. When triggered, you will be prompted to provide an input_message.
  Workflow Chaining: Other workflows (e.g., Workflow B, Main B) are called by preceding workflows via workflow_call. The input message is passed along through this chain.

  const express = require('express');
const { createProxyMiddleware } = require('http-proxy-middleware');
const path = require('path');


const configureApp = (port, target, staticFolder) => {
  const app = express();

  // Serve static files
  app.use(express.static(staticFolder));

  // Proxy setup
  app.use('/api', createProxyMiddleware({
    target,
    changeOrigin: true,
    pathRewrite: { '^/api': '' },
    secure: false
  }));

  // Fallback to index.html for SPA
  app.get('*', (req, res) => {
    res.sendFile(path.resolve(staticFolder, 'index.html'));
  });

  // Start the server
  app.listen(port, () => {
    console.log(`Server running on http://localhost:${port}`);
  });

  return app;
};

// Set up static folder paths
const staticFolderPath = path.join(__dirname, 'public');

// Configure and start servers
const appV1 = configureApp(204, 'http://10.10.1.204:5040', staticFolderPath);
const appV3 = configureApp(182, 'http://10.10.1.182:5040', staticFolderPath);

// without proxy this it throwimg error 

// const express = require('express');
// const path = require('path');

// // Helper function to configure an Express app
// const createServer = (port) => {
//   const app = express();

//   // Define the path for the static files
//   const staticFolderPath = path.join(__dirname, 'public');

//   // Serve static files
//   app.use(express.static(staticFolderPath));

//   // Fallback to index.html for SPA
//   app.get('*', (req, res) => {
//     res.sendFile(path.resolve(staticFolderPath, 'index.html'));
//   });

//   // Start the server
//   app.listen(port, () => {
//     console.log(`Server is running at http://localhost:${port}`);
//   });

//   return app;
// };

// // Create and start servers for port 182 and 204
// const app182 = createServer(182);
// const app204 = createServer(204);
{
  "name": "cardservice",
  "version": "1.0.0",
  "description": "A Node.js application serving static files on multiple ports using Express.",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.19.2",
    "http-proxy-middleware": "^3.0.0"
  },
  "engines": {
    "node": ">=14.0.0"
  },
  "keywords": ["express", "static-files", "nodejs", "multiple-ports"],
  "author": "shirisha",
  "license": "MIT"
}


