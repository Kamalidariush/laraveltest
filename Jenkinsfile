def getdockertag(){
    return "${env.GIT_BRANCH}".replace("/",".") + "."+"${env.BUILD_ID}"
}    
pipeline {
   agent any
   environment {
        DOCKER_REGISTRY = "172.16.3.116:8081/repository/cicd"
        DOCKER_TAG = getdockertag()
		NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "172.16.3.116:8081/repository"
        NEXUS_REPOSITORY = "cicd"
        NEXUS_CREDENTIAL_ID = "Jenkins-user"
	}

   stages {
      stage('Build') {
        steps {
          echo 'Building...'
		  echo "${env.GIT_BRANCH}".replace("/",".") + "."+"${env.BUILD_ID}"
          echo "Running ${env.BUILD_ID} ${env.BUILD_DISPLAY_NAME} on ${env.NODE_NAME} and JOB ${env.JOB_NAME}"
		  script{
                    docker.withRegistry( '', "${env.NEXUS_CREDENTIAL_ID}") {
                        
                        def customImage = docker.build("${env.DOCKER_REGISTRY}:${env.DOCKER_TAG}")
                        customImage.push()

                        if ( GIT_BRANCH ==~ /.*master|.*hotfix\/.*|.*release\/.*/ )
                            customImage.push('latest')
                    }
                }
		  sh 'docker exec -it admin_api'
		  sh 'php artisan migrate --force'
		  sh 'php artisan view:clear'
		  sh 'php artisan route:clear'
		  sh 'php artisan cache:clear'
		  sh 'php artisan config:clear'
		  sh 'php artisan clear-compiled'
		  sh 'composer dump-autoload'
		  sh 'php artisan storage:link'
        }
   }
   
	
   stage('Test') {
     steps {
        echo 'Testing...'
     }
   }
   stage('DockerPush') {
        steps {
          echo 'Building...'
          echo "Running ${env.BUILD_ID} ${env.BUILD_DISPLAY_NAME} on ${env.NODE_NAME} and JOB ${env.JOB_NAME}"
        }
	}
   stage('Deploy_Dev') {
     steps {
        echo 'Deploy_Dev'
     }
   }
   stage('Deploy_Prod') {
     steps {
       echo 'Deploying_Prod'
     }
   }
  }
}