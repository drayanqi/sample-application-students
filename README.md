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
