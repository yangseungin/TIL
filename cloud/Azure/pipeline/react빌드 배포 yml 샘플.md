trigger:
- main

pool:
  vmImage: ubuntu-latest

variables:
  AzureStaticWebAppsApiToken: '0006cb5c39e99b361b1b97e8a80bffb9e17a0b961bddef37ac5eff5ab89479785-b3979d79-d442-4cc9-aec5-97b3b2b77334000135179'

steps:
- task: NodeTool@0
  inputs:
    versionSpec: '16.20.2'
  displayName: 'Install Node.js'

- script: |
    npm install
    npm run build
  displayName: 'npm install and build'

- task: AzureStaticWebApp@0
  inputs:
    app_location: '/'  
    api_location: ''
    output_location: 'build'
    azure_static_web_apps_api_token: $(AzureStaticWebAppsApiToken)