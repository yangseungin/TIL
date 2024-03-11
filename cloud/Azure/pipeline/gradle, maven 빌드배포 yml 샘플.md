# Gradle
```yaml 
# Gradle
# Build your Java project and run tests with Gradle using a Gradle wrapper script.
# Add steps that analyze code, save build artifacts, deploy, and more:
# https://docs.microsoft.com/azure/devops/pipelines/languages/java

trigger:
- main

pool:
  vmImage: ubuntu-latest

## 변수 선언
variables:
  azureSubscription: ##구독정보
  appName: ## app name
  resourceGroupName: ## 리소스 그룹
  location: ## 지역 정보 

steps:

- task: Gradle@2
  inputs:
    gradleWrapperFile: 'gradlew'
    workingDirectory: '$(System.DefaultWorkingDirectory)'
    tasks: 'build'
    publishJUnitResults: true
    testResultsFiles: '**/TEST-*.xml'
    javaHomeOption: 'JDKVersion'
    jdkVersionOption: '1.17'
    gradleOptions: '-Xmx3072m'
    sonarQubeRunAnalysis: false
    spotBugsAnalysis: false

- task: AzureRmWebAppDeployment@4
  displayName: Azure Web App Deploy
  inputs:
    ConnectionType: 'AzureRM'
    azureSubscription: $(azureSubscription)
    appType: 'webAppLinux'
    WebAppName: $(appName)
    packageForLinux: '$(System.DefaultWorkingDirectory)/build/libs/test-api-0.0.1-SNAPSHOT.jar' ## 샘플 jar
    AdditionalArguments: 
```


# Maven
```yaml
# Maven
# Build your Java project and run tests with Apache Maven.
# Add steps that analyze code, save build artifacts, deploy, and more:
# https://docs.microsoft.com/azure/devops/pipelines/languages/java

trigger:
- main

pool:
  vmImage: ubuntu-latest

steps:
- task: Maven@3
  inputs:
    mavenPomFile: 'pom.xml'
    mavenOptions: '-Xmx3072m'
    javaHomeOption: 'JDKVersion'
    jdkVersionOption: '1.17'
    jdkArchitectureOption: 'x64'
    publishJUnitResults: true
    goals: 'package'
  displayName: 'build spring boot app'

- task: AzureRmWebAppDeployment@4
  inputs:
    ConnectionType: 'AzureRM'
    azureSubscription: 'Azure subscription 1(51e8bfa0-5c6a-4b68-8b81-185c36b0ebd7)'
    appType: 'webAppLinux'
    WebAppName: 'dev-yang'
    packageForLinux: '$(System.DefaultWorkingDirectory)/target/test-ma-0.0.1-SNAPSHOT.jar'
    RuntimeStack: 'JAVA|17-java17'
    


```