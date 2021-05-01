node{
  stage('Git Checkout'){
      git url: 'https://github.com/javahometech/my-app',
          branch:'master'
  }
  stage('MVN Package'){
    def mvnHome = tool name: 'maven-3', type: 'maven'
    sh "${mvnHome}/bin/mvn clean package"
  }
stage("Image Prune"){
        sh 'docker system prune -a -f'
    }
  stage('Build Docker Image'){
    sh '''
    docker build -t poojadraut55/my-app:0.0.1 .
    docker-compose up 
    
    
  }

  stage('Upload Image to DockerHub'){
     withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
      sh "docker login -u poojadraut55 -p ${PASSWORD}"
       
    }
    sh 'docker push poojadraut55/my-app:0.0.1'
  }
 // stage('Remove Old Containers'){
 //   sshagent(['my-app-dev']) {
  //    try{
  //      def sshCmd = 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.18.198'
  //      def dockerRM = 'docker rm -f my-app'
  //      sh "${sshCmd} ${dockerRM}"
  //    }catch(error){

    //  }
   // }
// }
  stage('Deploy-Dev'){
    sshagent(['my-app-dev']) {
      input 'Deploy  to Dev?'
      def sshCmd = 'ssh -o StrictHostKeyChecking=no ec2-user@172.31.18.198'
      def dockerRun = 'docker run -d -p 8080:8080 --name my-app kammana/my-app:0.0.1'
      sh "${sshCmd} ${dockerRun}"
    }
  }
}
