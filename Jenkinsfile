node {
    def repo = "Assigenment-CI-CD-Pipline"
    def tag = "${env.BUILD_NUMBER}"
    def imageName = "rajv690/myapplication:${tag}"
    def containerName = "devopsexample"

    try {
        stage('Clean Workspace') {
            echo "🧹 Cleaning up old files..."
            sh "rm -rf ${repo}"
        }

        stage('Clone Git Repo') {
            echo "🔄 Cloning repository..."
            sh "git clone https://github.com/rajv69023/${repo}.git"
            sh "ls -la ${repo}"
        }

        stage('Build Docker Image') {
            echo "🐳 Building Docker image with tag: ${tag}"
            sh "cd ${repo} && docker build -t ${imageName} ."
        }

        stage('Push Docker Image') {
            echo "📤 Pushing image to DockerHub"
            sh "docker login -u rajv690 -p Rocky69023"
            sh "docker push ${imageName}"
        }

        stage('Deploy Docker Container') {
            echo "🚀 Deploying container..."
            sh "docker rm -f ${containerName} || true"
            sh "docker run --name ${containerName} -d -p 2222:2222 ${imageName}"
            echo "✅ App is live at: http://<your-server-ip>:2222"
        }

    } catch (err) {
        currentBuild.result = 'FAILURE'
        echo "❌ Pipeline failed: ${err}"
        throw err
    }
}
