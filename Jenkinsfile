pipeline {  
  agent {
    node {
      label 'maven'
    }
  }
     
  environment {
    // the address of your harbor registry
    REGISTRY = 'docker.io'
    // your docker hub username
    DOCKERHUB_USERNAME = 'yasir2000'
    // docker image name
    APP_NAME = 'devops-go-sample'
    // ‘dockerhubid’ is the credential id you created in KubeSphere for docker access token
    DOCKERHUB_CREDENTIAL = credentials('2ed957b9-283e-4444-9ae5-53aa2bcb735b')
    //DOCKERHUB_CREDENTIAL = '2ed957b9-283e-4444-9ae5-53aa2bcb735b'
    //the kubeconfig credential id you created in KubeSphere
    KUBECONFIG_CREDENTIAL_ID = 'github-go-kubeconfig'
    // the name of the project you created in KubeSphere, not the DevOps project name
    PROJECT_NAME = 'demo-devops'
  }
     
  stages {
    stage('docker login') {
      steps{
        container ('maven') {
          sh 'echo $DOCKERHUB_CREDENTIAL_PSW  | docker login -u $DOCKERHUB_CREDENTIAL_USR --password-stdin'
            }
          }  
        }
           
    stage('build & push') {
      steps {
        container ('maven') {
          sh 'git clone https://github.com/yuswift/devops-go-sample.git'
          sh 'cd devops-go-sample && docker build -t $REGISTRY/$DOCKERHUB_USERNAME/$APP_NAME .'
          sh 'docker push $REGISTRY/$DOCKERHUB_USERNAME/$APP_NAME'
          }
        }
      }
    stage ('deploy app') {
      steps {
        container('maven') {
          kubernetesDeploy(configs: 'devops-go-sample/manifest/deploy.yaml', kubeconfigId: "$KUBECONFIG_CREDENTIAL_ID")
          }
        }
      }
    }
  }
