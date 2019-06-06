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
     def dockerRun = 'docker run -p 80:80 --name wordpress -d thecobolt/word'
     sshagent(['ubu1611']) {
       sh "ssh -o StrictHostKeyChecking=no vagrant@10.100.0.218 ${dockerRun}"
     }
   }
}
