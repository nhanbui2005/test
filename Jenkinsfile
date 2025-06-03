
pipeline {
  agent any

  stages {
    stage('Clone') {
       steps {
            git branch: 'main', url: 'https://github.com/nhanbui2005/test.git'
        }
    }

    stage('Build') {
      steps {
        sh 'npm install'
        sh 'npm run build'
      }
    }

    stage('Deploy') {
      steps {
        sh '''
        rsync -avz -e "ssh -p 2405" ./build/ soc@42.112.59.109:/home/soc/test-deploy/
        ssh -p 2405 soc@42.112.59.109 "cd /home/soc/test-deploy && pm2 restart app || pm2 start npm --name app -- start"
        '''
      }
    }
  }
}
