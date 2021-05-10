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
                script {
                    
                    }
                }
            }
        }
        
        stage('DockerPush') {
            when {
                expression {
                    GIT_BRANCH ==~ /.*master|.*release\/.*|.*develop|.*hotfix\/.*/
                }
            }
            steps {
                script{
                    docker.withRegistry( '', 'kamaliid') {
                        
                        def customImage = docker.build("${env.DOCKER_REGISTRY}:${env.DOCKER_TAG}")
                        customImage.push()

                        if ( GIT_BRANCH ==~ /.*master|.*hotfix\/.*|.*release\/.*/ )
                            customImage.push('latest')
                    }
                }
            }
        }
        stage('Deploy_Dev') {
            when {
                expression {
                    GIT_BRANCH ==~ /.*develop/
                }
            }
            steps {
                script{
                    
                    }
                }

                input message: "Do you want to run migration?"

                script{
                    echo "Deploy application on developmment environment"
                    dir("ansible") {
                        
                    }
                }

                input message: "Do you want to run seeding?"

                script{
                    echo "Deploy application on developmment environment"
                    
                    }
                }
            }
        }
        
        stage('Undeploy_Dev'){
            when {
                expression {
                    GIT_BRANCH ==~ /.*develop/
                }
            }
            // timeout(time:5, unit:'DAYS') {
            //     input message:'Approve deployment?', submitter: 'it-ops'
            // }
            steps {
                input message: "Do you want to undeploy DEV?"
                script {
                    
                    }
                }
            }
        }
        stage('Deploy_Prod') {
            when {
                expression {
                    GIT_BRANCH ==~ /.*master|.*release\/.*|.*hotfix\/.*/
                }
            }
            steps {
                input message: "Do you want to proceed for production deployment?"
                script{
                    echo "Deploy application on stage environment"
                    }
                }

                input message: "Do you want to proceed for production migration?"
                script{
                    echo "Deploy application on stage environment"
                    
                    }
                }
            }
        }

    }
}