# sample-application-students

This is a sample application Springboot - Vue - Postgres Student application

## Backend

Simple Rest API Server

## Frontend

Simple Vue.js Client

## Database

Simple Postgres 9 Database

# TP : TRAVIS

- added npm test script in package.json
- added repo on travis via sso

here is what the actual travis.yml file looks like :

```
git:
  depth: 5
jobs:
  include:
    - stage: "Build and Test Java"
      language: java
      jdk: oraclejdk11
      before_script:
        - cd sample-application-backend
      script :
        - mvn clean verify
    - stage: "Build and Test Nodejs"
      language: node_js
      node_js: "12.20"
      before_script:
        - cd sample-application-frontend
      script:
        - npm run test
cache:
  directories:
    - "$HOME/.m2/repository"
    - "$HOME/.npm"
```

### Allow Travis acces on github repo via token

- Create access token in github account : https://github.com/settings/tokens : Don't forget to match the rights with Travis requirements
- Login to travis via this token

```
travis login [--pro] --github-token [MY_ACCESS_TOKEN]
```

use pro if connecting to travis.com instead of .org

### Creating secured variables

To create secured variables, the best way is to do so from the travis console, in the settings tab of your project.

Actual travis configuration : variavles are used like so `$MY_VAR`

```
git:
  depth: 5
jobs:
  include:
  - stage: Build And Test JAVA
    language: java
    jdk: oraclejdk11
    before_script:
    - cd sample-application-backend
    script:
    - mvn clean verify org.jacoco:jacoco-maven-plugin:prepare-agent install sonar:sonar -Dsonar.projectKey=devops-2021

  - stage: Build and Test Nodejs
    language: node_js
    node_js: '12.20'
    before_script:
    - cd sample-application-frontend
    script:
    - echo "npm install ..."
    - npm run lint
    - npm install
    - npm run test

  - stage: Build and push Frontend Image
    before_script:
    - cd sample-application-backend
    script:
    - echo "Docker build ..."
    - docker build  -t $DOCKER_LOGIN/backend .
    - echo "Logging in to docker ... $DOCKER_LOGIN"
    - echo "$DOCKER_PASSWORD" | docker login --username "$DOCKER_LOGIN" --password-stdin
    - echo "docker push images..."
    - docker push $DOCKER_LOGIN/backend

  - stage: Build and push Backend Image
    before_script:
    - cd sample-application-backend
    - echo "Logging in to docker..."
    - echo "$DOCKER_PASSWORD" | docker login --username "$DOCKER_LOGIN" --password-stdin
    - docker build -t $DOCKER_LOGIN/frontend .
cache:
  directories:
  - "$HOME/.m2/repository"
  - "$HOME/.npm"

services:
  - docker
```

Now, each push on github will trigger travis builds, which will trigger docker builds/pushes for both backend and frontend
