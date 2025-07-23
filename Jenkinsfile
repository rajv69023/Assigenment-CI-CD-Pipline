node {
    def mvnHome = tool 'maven-3.5.2'
    def repoDir = 'Assigenment-CI-CD-Pipline'
    def dockerImageTag = "devopsexample:${env.BUILD_NUMBER}"
    def dockerImageId = ''

    try {
        stage('Clone Git Repo') {
            echo 'Cleaning only old Git repo folder...'
            sh "rm -rf ${repoDir}"

            echo 'Cloning fresh repo...'
            sh "git clone https://github.com/rajv69023/${repoDir}.git"
        }

        stage('Build Project with Maven') {
            dir(repoDir) {
                echo 'Running Maven build...'
                sh "${mvnHome}/bin/mvn clean install"
            }
        }

        stage('Build Docker Image') {
            echo "Building Docker image: ${dockerImageTag}"
            docker.build(dockerImageTag, repoDir)

            // Get latest image ID after build
            dockerImageId = sh(script: "docker images -q ${dockerImageTag}", returnStdout: true).trim()
            echo "✅ Docker image ID: ${dockerImageId}"
        }

        stage('Login to DockerHub') {
            echo 'Logging in to DockerHub...'
            sh "docker login -u rajv690 -p Rocky69023"
        }

        stage('Push Docker Image') {
            echo 'Tagging and pushing image to DockerHub...'
            sh "docker tag ${dockerImageId} rajv690/myapplication:${env.BUILD_NUMBER}"
            sh "docker push rajv690/myapplication:${env.BUILD_NUMBER}"
        }

        stage('Deploy') {
            echo "🚀 Deploying Docker image ID: ${dockerImageId}"
            // Sample deployment command using the image ID
            sh "docker run -d --name myapp-${env.BUILD_NUMBER} ${dockerImageId}"
        }

    } catch (err) {
        currentBuild.result = 'FAILURE'
        echo "❌ Pipeline failed: ${err}"
        throw err
    }
}
