node {
    // Reference to Maven tool configured in Jenkins
    def mvnHome = tool 'maven-3.5.2'

    // Docker image variables
    def dockerImage
    def dockerImageTag = "devopsexample:${env.BUILD_NUMBER}"

    try {
        stage('Clone Repo') {
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
            sh "docker login -u rajv690 -p Rocky69023"
        }

        stage('Push Docker Image') {
            // Tag the built image with repo name and push
            sh "docker tag ${dockerImageTag} rajv690/myapplication:${env.BUILD_NUMBER}"
            sh "docker push rajv690/myapplication:${env.BUILD_NUMBER}"
        }

    } catch (err) {
        currentBuild.result = 'FAILURE'
        echo "❌ Pipeline failed: ${err}"
        throw err
    }
}
