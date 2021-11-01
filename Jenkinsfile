pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }
        }

      stage('Unit Tests') {
            steps {
              sh "mvn test"
            }  
        } 

      stage('Docker Build and Push') {
     	   steps {
        		withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          		sh 'printenv'
       		    sh 'docker build -t siddharth67/numeric-app:""$GIT_COMMIT"" .'
         		sh 'docker push siddharth67/numeric-app:""$GIT_COMMIT""'
      		   }
       		 }  
      	}
   	 }
}