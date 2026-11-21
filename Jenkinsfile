node {
    def appDir = '/var/www/nextjs-app'

    stage('Clean Workspace') {
        echo 'Cleaning Jenkins Workspace...'
        deleteDir()
    }

    stage('Clone Repository') {
        echo 'Cloning the repository...'
        git (
            branch: 'main',
            url: 'https://github.com/Greshanris/cicd_jenkins_aws_nextjs.git',
        )
    }

    stage('Deploy to EC2') {
        echo 'Deploying to EC2...'
        sh """
            sudo mkdir -p ${appDir}
            sudo chown -R jenkins:jenkins ${appDir}
            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appDir}

            cd ${appDir}
            sudo apt install nodejs npm -y
            sudo npm install
            sudo npm run build
            sudo fuser -k 3000/tcp || true
            npm run start
        """
    }
}