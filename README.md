# Jenkins-Jobs

## Demo Project

**Create a CI Pipeline with Jenkinsfile (Freestyle, Pipeline, Multibranch Pipeline)**

**Technologies used:** Jenkins, Docker, Linux, Git, Java, Maven

**Project Description:**
CI Pipeline for a Java Maven application to build and push to the repository

**Installation Steps:**
1. Install Build Tools (Maven, Node) in Jenkins
    1. Enter the container as root: `docker exec -u 0 -it 3ad844beaaab bash`
    2. Check which Linux distribution the container is running: `cat /etc/issue`
    3. Install Node.js:
        - `apt update`
        - `apt install curl`
        - `curl -sL https://deb.nodesource.com/setup_10.x -o nodesource_setup.sh`
        - `bash nodesource_setup.sh`
        - `apt install nodejs`
        - `nodejs -v`
2. Make Docker available on Jenkins server
    1. Create the Jenkins container with mounted Docker:
        - `docker run -p 8080:8080 -p 50000:50000 -d -v jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/usr/bin/docker jenkins/jenkins:lts`
    2. Enter as root and modify Docker.sock permission:
        - `docker exec -u 0 -it 3a1a5e1f119c bash`
        - `chmod 666 /var/run/docker.sock`
    3. Restart Docker:
        - `systemctl restart docker`
    4. Start the container again:
        - `docker start 3ad844beaavc`
3. Create Jenkins credentials for a git repository
4. Create different Jenkins job types (Freestyle, Pipeline, Multibranch pipeline) for the Java Maven project with Jenkinsfile to

    a. Connect to the application’s git repository
    
    b. Build Jar: In the Maven build step, add the command `package`
    
    c. Build Docker Image: In the shell script, add `docker build -t java-maven-app:1.0 .`
    
    d. Push to private DockerHub repository:
    
      - **DockerHub Repository**
        
         1. Add DockerHub credentials in Jenkins
         2. Define variables for the username and password:
                - Select: "Build Environment" → "Use secret text(s) or file(s)" → "Add" → "Username" and "Password" (separate)
         3. Build configuration in shell execution:
         
                ```
                docker build -t uniyalrachna/demo-app:1.0 .
                echo $PASSWORD | docker login -u $USERNAME --password-stdin
                docker push uniyalrachna/demo-app:1.0
                ```
      - **Nexus Repository**
        
         1. Configure the host for Nexus by creating the file `/etc/docker/daemon.json` and adding insecure registries to it
            
                ```
                {
                    "insecure-registries" : ["64.227.152.141:8083"]
                }
                ```
                
         2. Enter as root and modify Docker.sock permission:
            
                - `docker exec -u 0 -it 3a1a5e1f119c bash`
                
                - `chmod 666 /var/run/docker.sock`
                
         3. Create credentials for Nexus
           
         4. Build configuration in shell execution:
         
                ```
                docker build -t 64.227.152.141:8083
                ```
                
Java- Maven app : https://gitlab.com/rachana6/java-maven-app.git
