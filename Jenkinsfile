def getdockertag(){
    return "${env.GIT_BRANCH}".replace("/",".") + "."+"${env.BUILD_ID}"
}    
pipeline {
   agent any
   environment {
        DOCKER_REGISTRY = "kamalidariush/cicd"
        kamaliid = "kamaliid"
        DOCKER_TAG = getdockertag()
	}

   stages {
      stage('Build') {
        steps {
          echo 'Building...'
		  echo "${env.GIT_BRANCH}".replace("/",".") + "."+"${env.BUILD_ID}"
          echo "Running ${env.BUILD_ID} ${env.BUILD_DISPLAY_NAME} on ${env.NODE_NAME} and JOB ${env.JOB_NAME}"
		  sh 'composer install'
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