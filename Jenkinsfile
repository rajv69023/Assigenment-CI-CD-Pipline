node {
    def mvnHome = tool 'maven-3.5.2'
    def repoDir = 'Assigenment-CI-CD-Pipline'
    def dockerImageTag = "devopsexample:${env.BUILD_NUMBER}"
    def dockerImageId = ''

    try {
        stage('Clone Git Repo') {
            echo '🔄 Cleaning and cloning repo...'
            sh """
                if [ -d "${repoDir}" ]; then
                    echo "⚠️ Old repo folder exists. Removing..."
                    rm -rf ${repoDir}
                fi
                git clone https://github.com/rajv69023/Assigenment-CI-CD-Pipline.git
            """
        }

        stage('Build Project with Maven') {
            dir(repoDir) {
                echo '🛠️ Running Maven build...'
                sh "${mvnHome}/bin/mvn clean install"
            }
        }

        stage('Build Docker Image') {
            echo "🐳 Building Docker image: ${dockerImageTag}"
            docker.build(dockerImageTag, repoDir)

            dockerImageId = sh(script: "docker images -q ${dockerImageTag}", returnStdout: true).trim()
            echo "✅ Docker image ID: ${dockerImageId}"
        }

        stage('Login to DockerHub') {
            echo '🔐 Logging in to DockerHub...'
            sh "docker login -u rajv690 -p Rocky69023"
        }

        stage('Push Docker Image') {
            def remoteTag = "rajv690/myapplication:${env.BUILD_NUMBER}"
            echo "📤 Pushing image as ${remoteTag}"
            sh "docker tag ${dockerImageId} ${remoteTag}"
            sh "docker push ${remoteTag}"
        }

        stage('Deploy') {
            echo "🚀 Deploying Docker container from image: ${dockerImageId}"
            sh """
                docker rm -f myapp-${env.BUILD_NUMBER} || true
                docker run -d --name myapp-${env.BUILD_NUMBER} ${dockerImageId}
            """
        }

    } catch (err) {
        currentBuild.result = 'FAILURE'
        echo "❌ Pipeline failed: ${err}"
        throw err
    }
}
