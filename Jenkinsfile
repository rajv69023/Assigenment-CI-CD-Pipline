node {
    def mvnHome = tool 'maven-3.5.2'
    def dockerImageTag = "devopsexample:${env.BUILD_NUMBER}"

    try {
        stage('Clone Git Repo') {
            echo 'Cleaning workspace and cloning repo...'
            sh 'rm -rf Assigenment-CI-CD-Pipline'
            sh 'git clone https://github.com/rajv69023/Assigenment-CI-CD-Pipline.git'
        }

        stage('Build Project') {
            dir('Assigenment-CI-CD-Pipline') {
                echo 'Building with Maven...'
                sh "${mvnHome}/bin/mvn clean install"
            }
        }

        stage('Build Docker Image') {
            echo "Building Docker image: ${dockerImageTag}"
            docker.build(dockerImageTag, 'Assigenment-CI-CD-Pipline')
        }

        stage('Login to DockerHub') {
            echo 'Logging in to DockerHub...'
            sh "docker login -u rajv690 -p Rocky69023"
        }

        stage('Push Docker Image') {
            echo 'Tagging and pushing image...'
            sh "docker tag ${dockerImageTag} rajv690/myapplication:${env.BUILD_NUMBER}"
            sh "docker push rajv690/myapplication:${env.BUILD_NUMBER}"
        }

    } catch (err) {
        currentBuild.result = 'FAILURE'
        echo "❌ Pipeline failed: ${err}"
        throw err
    }
}
