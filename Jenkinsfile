stage('Build Docker Image') {
    steps {
        sh '''
        docker build -t nodejs-app:latest .
        '''
    }
}

stage('Deploy Application') {
    steps {
        sh '''
        docker stop nodejs-app 2>/dev/null || true
        docker rm nodejs-app 2>/dev/null || true

        docker run -d \
          --name nodejs-app \
          --restart unless-stopped \
          -p 3000:3000 \
          nodejs-app:latest
        '''
    }
}

stage('Verify Deployment') {
    steps {
        sh '''
        docker ps
        '''
    }
}
