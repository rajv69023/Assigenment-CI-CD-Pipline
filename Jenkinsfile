node {
    def mvnHome = tool 'maven-3.5.2'
    def dockerImage
    def dockerImageTag = "devopsexample:${env.BUILD_NUMBER}"

    try {
        stage('Clone Repo') {
            echo "Cloning GitHub repo..."
            git branch: 'main', url: 'https://github.com/rajv69023/Assigenment-CI-CD-Pipline.git'
        }

        stage('Build Project') {
            echo "Building project with Maven..."
            sh "'${mvnHome}/bin/mvn' clean package -DskipTests"
        }

        stage('Build Docker Image') {
            echo "Building Docker image..."
            dockerImage = docker.build(dockerImageTag)
        }

        stage('Login to DockerHub') {
            echo "Logging in to DockerHub..."
            sh "docker login -u rajv690 -p Rocky69023"
        }

        stage('Push Docker Image') {
            echo "Pushing Docker image to DockerHub..."
            sh "docker tag ${dockerImageTag} rajv690/myapplication:${env.BUILD_NUMBER}"
            sh "docker push rajv690/myapplication:${env.BUILD_NUMBER}"
        }

    } catch (err) {
        currentBuild.result = 'FAILURE'
        echo "❌ Pipeline failed: ${err}"
        throw err
    }
}
