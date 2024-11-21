# resusable-repo-1
This repo are used for working on example on the concept of reusable workflows

# Workflow Overview
This repository contains multiple GitHub Actions workflows that are interconnected, enabling the passing of input messages between different workflows for processing. The workflows are designed to be reusable and can be triggered manually or by other workflows. Below is a breakdown of each workflow and its role in the overall setup.

# Workflows in the Repository
## Workflow A (workflow-a.yml)
### Trigger: Manual trigger via workflow_dispatch.
### Inputs: Accepts an input_message from the user.
### Action: This workflow triggers Workflow B, passing the input_message as an input.
## Workflow B (workflow-b.yml)
### Trigger: Triggered by another workflow using workflow_call.
### Inputs: Accepts an input_message from the calling workflow (e.g., Workflow A).
### Action: This workflow processes the input_message. In the example provided, it runs on a Windows environment and simply outputs the message.
## Workflow C (workflow-c.yml)
### Trigger: Triggered by Workflow B from a separate repository (reusable-repo-2).
### Inputs: Accepts an input_message passed from Workflow B.
### Action: This workflow processes the input_message on a Windows machine and outputs the message for logging or further use.
## Main A (main-a.yml)
### Trigger: Manual trigger via workflow_dispatch.
### Inputs: Accepts an input_message from the user.
### Action: This workflow triggers Main B, passing the input_message to it.
## Main B (main-b.yml)
### Trigger: Triggered by Main A using workflow_call.
### Inputs: Accepts an input_message from Main A.
### Action: This workflow triggers Main C, passing the input_message to it.
## Main C (main-c.yml)
### Trigger: Triggered by Main B using workflow_call.
### Inputs: Accepts an input_message passed from Main B.
### Action: This workflow processes the input_message on a Windows machine and outputs the message for logging or further processing.

# How to Trigger Workflows
## Manual Trigger: Some workflows (e.g., Workflow A, Main A) can be triggered manually via the GitHub UI. When triggered, you will be prompted to provide an input_message.
## Workflow Chaining: Other workflows (e.g., Workflow B, Main B) are called by preceding workflows via workflow_call. The input message is passed along through this chain.
