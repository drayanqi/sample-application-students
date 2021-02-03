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

To create secured variables, ruby must be installed.

env variables can be created like so :

```
env:
  global:
    - secure: mcUCykGm4bUZ3CaW6AxrIMFzuAYjA98VIz6YmYTmM0/8sp/B/54JtQS/j0ehCD6B5BwyW6diVcaQA2c7bovI23GyeTT+TgfkuKRkzDcoY51ZsMDdsflJ94zV7TEIS31eCeq42IBYdHZeVZp/L7EXOzFjVmvYhboJiwnsPybpCfpIH369fjYKuVmutccD890nP8Bzg8iegssVldgsqDagkuLy0wObAVH0FKnqiIPtFoMf3mDeVmK2AkF1Xri1edsPl4wDIu1Ko3RCRgfr6NxzuNSh6f4Z6zmJLB4ONkpb3fAa9Lt+VjJjdSjCBT1OGhJdP7NlO5vSnS5TCYvgFqNSXqqJx9BNzZ9/esszP7DJBe1yq1aNwAvJ7DlSzh5rvLyXR4VWHXRIR3hOWDTRwCsJQJctCLpbDAFJupuZDcvqvPNj8dY5MSCu6NroXMMFmxJHIt3Hdzr+hV9RNJkQRR4K5bR+ewbJ/6h9rjX6Ot6kIsjJkmEwx1jllxi4+gSRtNQ/O4NCi3fvHmpG2pCr7Jz0+eNL2d9wm4ZxX1s18ZSAZ5XcVJdx8zL4vjSnwAQoFXzmx0LcpK6knEgw/hsTFovSpe5p3oLcERfSd7GmPm84Qr8U4YFKXpeQlb9k5BK9MaQVqI4LyaM2h4Xx+wc0QlEQlUOfwD4B2XrAYXFIq1PAEic=
  jobs:
    - USE_NETWORK=true
    - USE_NETWORK=false
    - secure: <you can also put encrypted vars inside matrix>
```

The only secured way is to use secured variables :

```
travis encrypt MY_SECRET_ENV=super_secret --add [env.global]
```

- --add = add directly to travis.yml file
- [env.global] --> specifies scope for newly created env variable
