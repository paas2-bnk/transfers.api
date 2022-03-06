pipeline {   
    
    parameters {
        string (name: 'IMAGE_TAG', defaultValue: 'latest',  description: 'Image tag to deploy')       
    }    

    agent any
    
    stages {
        stage('preparation') {
            steps {                
                checkout scm
                script {
                    IMAGE_TAG = sh(returnStdout: true, script: "git log -n 1 --pretty=format:'%h'").trim()
                }
            }
        }     

        stage('docker build') {  
            steps {              
                script {
                    sh("/usr/local/bin/run-kaniko-job.sh --dockerfile Dockerfile --context https://github.com/mc-istio/transfers-api.git --destination kadirzade/mc-istio-transfers-api:${IMAGE_TAG}")  
                    sh("/usr/local/bin/wait-on-kaniko-job.sh")      
                } 
            }                                             
        }

        // stage('tag') {
        //     steps{
        //         sh("git tag -a ${IMAGE_TAG} -m 'Jenkins'")
        //             withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: '7d28effb-6679-40bd-a7f2-90b4d6da1ced', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD']]) {
        //                 sh("git config credential.username ${env.GIT_USERNAME}")
        //                 sh("git config credential.helper '!f() { echo password=\$GIT_PASSWORD; }; f'")
        //                 sh("GIT_ASKPASS=true git push origin --tags")    

        //                 sh("git config --unset credential.username")
        //                 sh("git config --unset credential.helper")
        //             }
        //     }
        // }
                              
    }
}