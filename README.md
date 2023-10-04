# Java App Deployemnt using CICD

### This is a pipeline consisting of the deploying a JAVA Application on CICD using Components like Jenkins, Sonarqube, Gradle, Nexus Multi Stage Docker Build and Helm with future extension scope for the pipeline.

![Workflow Diagram](https://github.com/adityadhopade/java_gradle_cicd_app/assets/48392204/cf0ac47c-74de-4933-887b-384986220e97)

### Tools Summary:

- ```Gradle```: It is build tool for java; lighter and faster than maven
- ```Jenkins```: Used a s a Continious Integration tool to breach the ntegration between various tools used with the respective plugins and pipeline implementetion.
- ```Nexus```: It is a image Repository; we have created a **PRIVATE REPOSITORY** in our case for storing image generated.
- ```Multi-Stage Docker Builds```: The image first generated is cascaded as input to the other docker image to reduce the size of overall image.

We have discussed all this concepts in a detailed blog and followed implementetion with the best practices here.

[BLOG LINK TO FOLLOW FOR IMPLEMENTATION](https://codemyworld.hashnode.dev/cicd-using-jenkins-sonarqube-gradle-nexus-multi-stage-docker-file-and-helm)

To run gradle we follows here: 

build tool is ** gradle **

when we build the code using command ```./gradlew build ``` it will generate war file. that war can be placed in tomcat server to see application web page

code is integrated with sonarqube plugin which help us in static code analysis 

``` ./gradlew sonarqube ```
