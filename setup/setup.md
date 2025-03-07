## 📌 Overview

This guide provides step-by-step instructions for setting up **SonarQube** with **PostgreSQL** using **Docker Compose**. It includes PostgreSQL as the database, a health check mechanism, and how to run a SonarQube scan.

---

## 🚀 Prerequisites

Ensure you have the following installed on your system:

- **Docker** ([Install Docker](https://docs.docker.com/get-docker/))
- **Docker Compose** ([Install Docker Compose](https://docs.docker.com/compose/install/)) (**Optional**)

---

## 📥 Download the Sonar Scanner

Follow this guide: [How to Install SonarScanner CLI](https://medium.com/novai-devops-101/how-to-install-sonarscanner-cli-client-on-windows-linux-and-macos-94b033f719c4)

---

## 🛠 Setup SonarQube Using Docker

### Description

Using Docker to run SonarQube allows you to quickly set up and deploy SonarQube in a containerized environment. This approach isolates SonarQube from your system and provides an easy way to spin up or tear down the service.

### Steps to Set Up SonarQube with Docker

1. **Pull the SonarQube Docker Image**
    
    ```shell
    docker pull sonarqube
    ```
    
2. **Run SonarQube in a Docker Container**
    
    ```shell
    docker run -d -p 9000:9000 --name sonarqube sonarqube
    ```
    
3. **Access SonarQube**
    
    SonarQube will be available at `http://localhost:9000`.
   
---

## 📦 Steps to Set Up SonarQube with Docker-Compose

### 🔧 Step 1: Create the `docker-compose.yml` File

Create a `docker-compose.yml` file:

```shell
touch docker-compose.yml
```

Paste the following configuration inside `docker-compose.yml`:

```yaml
services:
  postgresql:
    image: postgres:13
    container_name: sonarqube_postgres
    restart: unless-stopped
    environment:
      POSTGRES_DB: sonarqube
      POSTGRES_USER: sonar
      POSTGRES_PASSWORD: sonarpassword
    ports:
      - '5432:5432'
    volumes:
      - postgresql_data:/var/lib/postgresql/data
    healthcheck:
      test: ['CMD-SHELL', 'pg_isready -U sonar -d sonarqube']
      interval: 10s
      retries: 5
      timeout: 5s

  sonarqube:
    image: sonarqube
    container_name: sonarqube
    restart: unless-stopped
    depends_on:
      postgresql:
        condition: service_healthy
    environment:
      SONAR_JDBC_URL: jdbc:postgresql://postgresql:5432/sonarqube
      SONAR_JDBC_USERNAME: sonar
      SONAR_JDBC_PASSWORD: sonarpassword
    ports:
      - '9000:9000'
    volumes:
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_extensions:/opt/sonarqube/extensions
      - sonarqube_logs:/opt/sonarqube/logs

volumes:
  postgresql_data:
  sonarqube_data:
  sonarqube_extensions:
  sonarqube_logs:
```

### 🔄 Step 2: Start the Services in Background

Run the following command to start SonarQube and PostgreSQL in the background:

```shell
docker-compose up -d
```

This will download the required images and start both services in detached mode.

Check if the containers are running:

```shell
docker ps
```

---

## 🔍 Verify SonarQube is Running

1. Open **[http://localhost:9000](http://localhost:9000/)** in your browser.
2. Use the default login credentials:

   - **Username:** admin
   - **Password:** admin

   **Change the password when prompted.**

3. Once logged in, click on `Create a local project`.
![Pasted image 20250306120931](https://github.com/user-attachments/assets/f1295251-babb-4861-a397-e62d2baae030)
4. Fill in the necessary fields and click **Next**.
![Pasted image 20250306121156](https://github.com/user-attachments/assets/ddbbefa0-75d1-4a7f-8544-bdd99b912ffd)
5. Select your project settings as per your requirements and click **Create Project**.
![Pasted image 20250306121441](https://github.com/user-attachments/assets/591eca92-f083-4215-9608-6dcf90f98b11)
![Pasted image 20250306121618](https://github.com/user-attachments/assets/5c357a8c-26e2-465e-9dc6-f24d5f70be3b)
6. SonarQube will generate a token for your project. Copy this token.
![Pasted image 20250306121656](https://github.com/user-attachments/assets/d35511ab-e342-4b37-baa6-f8b3f1cdf99e)


---

## 📄 Step 3: Create a `sonar-project.properties` File

### 🔧 Description
The `sonar-project.properties` file is essential for configuring SonarQube analysis. It defines key settings such as the project key, name, SonarQube server URL, authentication token, and source files for analysis.

### 📌 Steps to Create the File

1. Create a `sonar-project.properties` file in your project root:
    
    ```shell
    touch sonar-project.properties
    ```
    
2. Add the following content:

    ```properties
    sonar.projectKey=your_project_key
    sonar.projectName=Your Project Name
    sonar.host.url=http://localhost:9000
    sonar.login=your_generated_token
    sonar.sources=.
    ```
    
    Make sure to replace `your_project_key` and `your_generated_token` with actual values from your SonarQube project.

3. For more details, refer to the official SonarQube properties documentation: [SonarQube Project Properties](https://docs.sonarqube.org/latest/analysis/analysis-parameters/)

---

## 📌 Step 4: Enable Code Coverage in SonarQube

###     🔧 Description

- SonarQube does not calculate code coverage on its own. Instead, it relies on external test coverage tools such as Jest (for JavaScript/TypeScript), JaCoCo (for Java), or pytest-cov (for Python).

Follow this link: [Code coverage](https://docs.sonarsource.com/sonarqube-server/10.8/analyzing-source-code/test-coverage/overview/)

---

## 🔎 Run the Sonar Scanner

Execute the scanner in your project directory:

```shell
sonar-scanner
```

This will analyze your project and send the results to SonarQube. You can check the results in the **SonarQube dashboard** under your project.

---

## ✅ Conclusion

You have successfully set up **SonarQube** with **PostgreSQL** using **Docker Compose**, created a project, and run a SonarQube scan. 🎉
