
### Refer to Spring Boot Playlist

https://www.youtube.com/watch?v=5q4w-c2WUv0&list=PLuNxlOYbv61h66_QlcjCEkVAj6RdeplJJ&index=7


### Continuous Integration

1. Run Tests
2. Code Quality Checks (SonarQube, CheckStyle etc)
3. Deploy to Dev / Staging environment.


### Github Actions

Continuous Integration (CI) is a practice in software development where code changes are automatically tested and integrated into a shared repository frequently, ensuring that the application is always in a deployable state. **GitHub Actions** is a powerful tool that helps automate workflows, including Continuous Integration, directly in your GitHub repository.

In this guide, I’ll walk you through setting up **Continuous Integration (CI)** for your project using **GitHub Actions**.

### Steps for Setting Up Continuous Integration with GitHub Actions

4. **Create a GitHub Repository**:
    
    - If you don’t have a repository already, create one on GitHub or use an existing one.
5. **Create the `.github/workflows` Directory**:
    
    - In the root of your repository, create a directory called `.github/workflows`. This is where your GitHub Actions configuration files (workflow files) will live.
    - For example: `.github/workflows/ci.yml`.
6. **Define a GitHub Actions Workflow File**:
    
    - Inside the `.github/workflows` directory, you’ll create a YAML file (e.g., `ci.yml`) to define your CI workflow.
    
    Below is an example of a simple CI pipeline for a Java project that uses **Maven** for build and testing.
    

### Example: CI with GitHub Actions for a Java Project (Maven)

7. **Create `.github/workflows/build-api.yml`**:


```yaml
name: Build

on:
  push:
    paths:
      - "bookmarker-api/**"
    branches: [ "main" ]
  pull_request:
    paths:
      - "bookmarker-api/**"
    types:
      - opened
      - synchronize
      - reopened

jobs:
  build-bookmarker-api:
    name: Build bookmarker-api
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: ./bookmarker-api
    steps:
      - uses: actions/checkout@v4

      - name: Set up JDK 21
        uses: actions/setup-java@v4
        with:
          java-version: 21
          distribution: 'temurin'
          cache: 'maven'

      - name: Build with Maven
        run: ./mvnw -ntp verify

      - if: ${{ github.ref == 'refs/heads/main' }} // If main branch then execute
        name: Build and Publish Docker Image
        run: |
          docker login -u ${{ secrets.DOCKER_USERNAME }} -p ${{ secrets.DOCKER_PASSWORD }}
          ./mvnw spring-boot:build-image -Dspring-boot.build-image.imageName=${{ secrets.DOCKER_USERNAME }}/bookmarker-api
          docker push ${{ secrets.DOCKER_USERNAME }}/bookmarker-api
```


### Explanation of the Workflow File:

- **name**: This is the name of the workflow. In this example, it's called "Build".
    
- **on**: Specifies the events that trigger the workflow. In this case, it's triggered on:
    
    - `push` events to the `main` branch.
    - `pull_request` events targeting the `main` branch.
- **jobs**: A workflow contains one or more jobs. Each job contains a series of steps.
    
    - **build**: The job is named "build" and runs on an `ubuntu-latest` virtual machine.
        
    - **steps**: These are the individual tasks to run in the job.
        
        - **Checkout code**: The first step is to checkout the code from the GitHub repository using actions/checkout@v4.
        - **Set up JDK**: The second step sets up Java JDK 21 using the actions/setup-java@v4 action.
        - **Build with Maven**: The third step runs the `./mvnw -ntp verify` command to run all the test cases and create the jar file without showing any output.
        - **Run tests**: The final step runs build and publish docker image.

### Common Customizations:

- **Different Java Versions**: You can change the JDK version in the `setup-java` step by modifying the `java-version` parameter (e.g., '8', '17', etc.).
    
- **Other Build Tools**: For projects using Gradle, you can modify the script to use Gradle commands like `gradle build` instead of Maven commands.
    
- **Other Actions**: You can add more actions for different tasks, such as linting, code coverage, or deploying.

### Additional Features for CI:

8. **Matrix Builds**: You can use a matrix to run the same tests across multiple configurations, like different Java versions or operating systems.
    
    Example of a matrix for Java versions:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        java-version: [8, 11, 17]
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up JDK
        uses: actions/setup-java@v2
        with:
          java-version: ${{ matrix.java-version }}

      - name: Build with Maven
        run: mvn clean install
```


### Creating **secrets** in GitHub


Creating **secrets** in GitHub is an essential part of securing sensitive information such as API keys, access tokens, or other credentials used in your workflows or repositories. GitHub Actions can use these secrets during automated workflows without exposing them in your code.

### Steps to Create Secrets in GitHub:

9. **Go to Your GitHub Repository**:
    
    - Navigate to the **repository** where you want to add secrets.
10. **Access the Settings Page**:
    
    - On the main repository page, click on the **"Settings"** tab near the top of the page.
11. **Go to the Secrets Section**:
    
    - In the left sidebar of the settings page, scroll down and click on **"Secrets"**.
    - Then click on **"Actions"** under the **Secrets** category. This will allow you to add secrets specifically for GitHub Actions.
12. **Add a New Secret**:
    
    - On the **Secrets** page, click on the **"New repository secret"** button.
        
    - A form will appear where you can add the **name** and **value** of the secret.
        
        - **Name**: This is the **identifier** you will use to reference the secret in your GitHub Actions workflow. It's a good practice to use **uppercase letters** with underscores separating words (e.g., `MY_API_KEY`, `AWS_ACCESS_KEY`).
        - **Value**: This is the **sensitive data** that you want to store as a secret. For example, API keys, tokens, passwords, etc.
13. **Save the Secret**:
    
    - Once you’ve entered the secret name and value, click on the **"Add secret"** button to save it.

### Example:

- **Name**: `AWS_ACCESS_KEY_ID`
- **Value**: `AKIAIOSFODNN7EXAMPLE`

Once created, your secret is stored securely by GitHub. You can now use it in your GitHub Actions workflows.

### Using Secrets in GitHub Actions:

After creating the secrets, you can reference them in your GitHub Actions workflows by using the following syntax:

```yaml
env:
  MY_SECRET: ${{ secrets.MY_SECRET_NAME }}
```

Here’s an example of a GitHub Actions workflow that uses secrets:

```yaml
name: Deploy to AWS

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Set up AWS CLI
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: 'us-west-2'

    - name: Deploy
      run: |
        # Your deployment commands
        aws s3 cp my-file.txt s3://my-bucket/
```

### Explanation:

- In the above example, secrets are accessed using `${{ secrets.AWS_ACCESS_KEY_ID }}` and `${{ secrets.AWS_SECRET_ACCESS_KEY }}`.
- These secrets are injected into the workflow at runtime and are not exposed in the logs or code.

### Secret Scoping:

14. **Repository Secrets**:
    
    - Secrets added through the repository settings are scoped to that specific repository. They are available to all workflows in the repository.
15. **Organization Secrets**:
    
    - If you are using an organization, you can add **organization-level secrets**. These secrets can be used across all repositories within the organization, provided those repositories have access to them.
16. **Environment Secrets**:
    
    - You can also add secrets at the **environment** level. This allows you to create different secrets for different environments (e.g., `production`, `staging`, `development`), and GitHub Actions can use them accordingly.

### Security Considerations:

- **Secrets are encrypted**: GitHub encrypts secrets at rest, and they can only be accessed by workflows running in your repository or organization.
- **Access control**: Secrets are **not** exposed in logs. If you accidentally print them to the logs, they will be redacted automatically.
- **Limit secret scope**: Make sure that secrets are only available to the necessary workflows or jobs. Avoid giving unnecessary permissions to secrets.
- **Secret rotation**: Regularly rotate and update your secrets to reduce the risk in case they are compromised.

### Conclusion:

By creating secrets in GitHub, you can securely store sensitive information and use them in your workflows without exposing them in your source code. This is crucial for keeping API keys, access tokens, and credentials safe while automating your workflows with GitHub Actions.


![[20250227123944.png]]


![[20250227125818.png]]

![[20250227131614.png]]

docker-compose-app.yml file

```yaml
version: '3.8'
services:
  bookmarker-api:
    build:
      context: bookmarker-api
      dockerfile: Dockerfile.layered
    environment:
      SPRING_PROFILES_ACTIVE: docker
      SPRING_DATASOURCE_DRIVER_CLASS_NAME: org.postgresql.Driver
      SPRING_DATASOURCE_URL: jdbc:postgresql://bookmarker-db:5432/appdb
      SPRING_DATASOURCE_USERNAME: postgres
      SPRING_DATASOURCE_PASSWORD: postgres
    ports:
      - "18080:8080"
    restart: always
    depends_on:
      - bookmarker-db

  bookmarker-ui-nextjs:
    container_name: bookmarker-ui-nextjs
    build:
      context: bookmarker-ui-nextjs
      dockerfile: Dockerfile
    ports:
      - "13000:3000"
    environment:
      SERVER_SIDE_API_BASE_URL: http://bookmarker-api:8080
      CLIENT_SIDE_API_BASE_URL: http://localhost:18080
```

docker-compose.yml  file

```yaml
services:
  bookmarker-db:
    image: postgres:17-alpine
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=appdb
    ports:
      - "15432:5432"
```

### run.sh file

```bash
#!/bin/bash

declare dc_infra=docker-compose.yml
declare dc_app=docker-compose-app.yml

function build_api() {
    cd bookmarker-api
    ./mvnw clean package -DskipTests
    cd ..
}

function start_infra() {
    echo "Starting infra docker containers...."
    docker-compose -f ${dc_infra} up -d
    docker-compose -f ${dc_infra} logs -f
}

function stop_infra() {
    echo "Stopping infra docker containers...."
    docker-compose -f ${dc_infra} stop
    docker-compose -f ${dc_infra} rm -f
}

function start() {
    build_api
    echo "Starting all docker containers...."
    docker-compose -f ${dc_infra} -f ${dc_app} up --build -d
    docker-compose -f ${dc_infra} -f ${dc_app} logs -f
}

function stop() {
    echo "Stopping all docker containers...."
    docker-compose -f ${dc_infra} -f ${dc_app} stop
    docker-compose -f ${dc_infra} -f ${dc_app} rm -f
}

function restart() {
    stop
    sleep 3
    start
}

action="start"

if [[ "$#" != "0"  ]]
then
    action=$@
fi

eval ${action}
```


![[20250228112837.png]]

Docker file (Multistage) for Nextjs Application.

```bash
# Install dependencies only when needed
FROM node:22-alpine AS deps
RUN apk add --no-cache libc6-compat
WORKDIR /app
COPY package.json yarn.lock ./
RUN yarn install --frozen-lockfile

# Rebuild the source code only when needed
FROM node:22-alpine AS builder
WORKDIR /app

COPY . .
COPY --from=deps /app/node_modules ./node_modules
RUN yarn build

# Production image, copy all the files and run next
FROM node:22-alpine AS runner
WORKDIR /app

ENV NODE_ENV production

RUN addgroup -g 1001 -S nodejs
RUN adduser -S nextjs -u 1001

# You only need to copy next.config.js if you are NOT using the default configuration
COPY --from=builder /app/next.config.js ./
COPY --from=builder /app/public ./public
COPY --from=builder --chown=nextjs:nodejs /app/.next ./.next
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json

USER nextjs

EXPOSE 3000

CMD ["yarn", "start"]
```

### Refer the code

https://github.com/sivaprasadreddy/springboot-kubernetes-youtube-series



