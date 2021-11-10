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
      		withSonarQubeEnv('sonarqube') {
       	 sh "mvn sonar:sonar --Dsonar.projectKey=numericapp -Dsonar.host.url=http://test-devsecops.eastus.cloudapp.azure.com:9000 -Dsonar.login=cad2021c05392d4f2944f8ab0f6e782161dba47f"
      }
}
    //	  timeout(time: 2, unit: 'MINUTES') {
    	  //	script {
      	//	waitForQualityGate abortPipeline: true
      //	 }
    //  	}
  //    }
//    }
     
 
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