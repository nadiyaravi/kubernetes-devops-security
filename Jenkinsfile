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


      stage('SonarQube - SAST') {
         steps {
           sh "mvn clean verify sonar:sonar -Dsonar.projectKey=numeric-application -Dsonar.host.url=http://test-devsecops.eastus.cloudapp.azure.com:9000 -Dsonar.login=7cc99dc46f70b0db3d2a69e1b2653f32e326b28b"
       		}
         }
      

      stage('Docker Build and Push') {
     	   steps {
        		withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          		sh 'printenv'
       		    sh 'docker build -t nadiyar/numeric-app:""$GIT_COMMIT"" .'
         		sh 'docker push nadiyar/numeric-app:""$GIT_COMMIT""'
      		   }
       		 }  
      	}


      stage('Kubernetes Deployment - DEV') {
      		steps {
        		withKubeConfig([credentialsId: 'kubeconfig']) {
          		sh "sed -i 's#replace#nadiyar/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
          		sh "kubectl apply -f k8s_deployment_service.yaml"
       		 }
   		   }
    	 }
   	}
}