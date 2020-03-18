node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/rajkumar-ui/CI-CD-project.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.5.6", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t nani903020/buildcijenkinsapp .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'Docker_Hub_Pwd', variable: 'Docker_Hub_Pwd')]) {
          sh "docker login -u nani903030 -p ${Docker_Hub_Pwd}"
        }
        sh 'docker push nani903020/buildcijenkinsapp'
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name buildcijenkinsapp nani903020/buildcijenkinsapp'
         
         sshagent(['DOCKER_SERVER']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.86.162 docker stop buildcijenkinsapp || true'
          sh 'ssh  ubuntu@172.31.86.162 docker rm buildcijenkinsapp || true'
          sh 'ssh  ubuntu@172.31.86.162 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@172.31.86.162 ${dockerRun}"
       }
       
    }
     
     
}
