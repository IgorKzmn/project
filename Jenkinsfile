node{
   stage('Down Git'){
       git credentialsId: 'git-creds', url: 'git@github.com:IgorKzmn/project.git'
   }
   stage('Build Docker Image'){
     sh 'docker build -t thecobolt/word .'
   }
   stage('Push Docker Image'){
     withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPwD')]) {
        sh "docker login -u thecobolt -p ${dockerHubPwD}"
     }
     sh 'docker push thecobolt/word'
   }
   stage('Run on Dev Server'){
     def dockerRun = 'docker run -p 8081:80 --name docker-wordpress-nginx -d eugeneware/docker-wordpress-nginx'
     sshagent(['ubu1611']) {
       sh "ssh -o StrictHostKeyChecking=no vagrant@10.100.0.218 ${dockerRun}"
     }
   }
}
