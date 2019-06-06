node{
   stage('Git'){
       git credentialsId: 'git-creds', url: 'git@github.com:IgorKzmn/docker-wordpress-nginx.git'
   }
   stage('Build Docker Image'){
     sh 'docker build -t thecobolt/test4:1 .'
   }
   stage('Push Docker Image'){
     withCredentials([string(credentialsId: 'docker-pwd', variable: 'dockerHubPwD')]) {
        sh "docker login -u thecobolt -p ${dockerHubPwD}"
     }
     sh 'docker push thecobolt/test4:1'
   }
   stage('Run Wordpress on Dev Server'){
     def dockerRun = 'docker run -p 80:80 --name test4 -d thecobolt/test4:1'
     sshagent(['ubu1611']) {
       sh "ssh -o StrictHostKeyChecking=no vagrant@10.100.0.218 ${dockerRun}"
     }
   }
}