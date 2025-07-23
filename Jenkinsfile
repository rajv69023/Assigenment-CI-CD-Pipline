node {
<<<<<<< HEAD
    // Reference to Maven tool configured in Jenkins
    def mvnHome = tool 'maven-3.5.2'

    // Docker image variables
=======
    def mvnHome = tool 'maven-3.5.2'
>>>>>>> b65f566 (jenkins file update)
    def dockerImage
    def dockerImageTag = "devopsexample:${env.BUILD_NUMBER}"

    try {
        stage('Clone Repo') {
<<<<<<< HEAD
            // Clone the 'main' branch explicitly
            git branch: 'main', url: 'https://github.com/rajv69023/Assigenment-CI-CD-Pipline.git'

            // Get Maven tool reference
            mvnHome = tool 'maven-3.5.2'
        }

        stage('Build Project') {
            // Build project using Maven
            sh "'${mvnHome}/bin/mvn' clean install"
        }

        stage('Build Docker Image') {
            // Build Docker image with tag
            dockerImage = docker.build(dockerImageTag)
        }

        stage('Login to DockerHub') {
            // Login to DockerHub
=======
            echo "Cloning GitHub repo..."
            git branch: 'main', url: 'https://github.com/rajv69023/Assigenment-CI-CD-Pipline.git'
        }

        stage('Build Project') {
            echo "Building project with Maven..."
            dir('Assigenment-CI-CD-Pipline') {
                sh "'${mvnHome}/bin/mvn' clean package -DskipTests"
            }
        }

        stage('Build Docker Image') {
            echo "Building Docker image..."
            dir('Assigenment-CI-CD-Pipline') {
                dockerImage = docker.build(dockerImageTag)
            }
        }

        stage('Login to DockerHub') {
            echo "Logging in to DockerHub..."
>>>>>>> b65f566 (jenkins file update)
            sh "docker login -u rajv690 -p Rocky69023"
        }

        stage('Push Docker Image') {
<<<<<<< HEAD
            // Tag the built image with repo name and push
=======
            echo "Pushing Docker image to DockerHub..."
>>>>>>> b65f566 (jenkins file update)
            sh "docker tag ${dockerImageTag} rajv690/myapplication:${env.BUILD_NUMBER}"
            sh "docker push rajv690/myapplication:${env.BUILD_NUMBER}"
        }

<<<<<<< HEAD
=======
        stage('Deploy') {
            echo "🚀 Deploying container..."
            sh "docker rm -f devopsexample || true"
            sh "docker run -d --name devopsexample -p 2222:2222 rajv690/myapplication:${env.BUILD_NUMBER}"
        }

>>>>>>> b65f566 (jenkins file update)
    } catch (err) {
        currentBuild.result = 'FAILURE'
        echo "❌ Pipeline failed: ${err}"
        throw err
    }
}
