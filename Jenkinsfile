pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Checkout'){
            
            steps{
                
               git branch: 'main', url: 'https://github.com/rajdeepsingh642/helm-argocd-k8s.git'
                    
                    
                }
        }
        
        stage('docker build image'){
            steps{
                script{
                    sh 'docker build -t 192.168.1.70:8083/helm-argocd:${BUILD_ID} .'

                }
            }
        }
        stage('docker image push nexus'){
            steps{
                script{
                     withCredentials([string(credentialsId: 'nexus-token', variable: 'nexus_creds')]) {
                        sh "docker login -u admin -p $nexus_creds 192.168.1.70:8083"
                        sh 'docker push 192.168.1.70:8083/helm-argocd:${BUILD_ID}'

                }
          
                  }
                    }
        }
         stage('pushin helm chart to nexus'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'nexus-token', variable: 'nexus_creds')]) {
             
    
                     echo "Packing helm chart"
                      sh "helm package -d ${WORKSPACE}/helm ${WORKSPACE}/helm/devopsodia"
           // sh "${WORKSPACE}/build.sh --pack_helm --push_helm --helm_repo ${HELM_REPO} --helm_usr ${HELM_USR} --helm_psw ${HELM_PSW}"
                        sh "curl -u admin:${nexus_creds} http://192.168.1.70:8081/repository/helm-repo/ --upload-file ${WORKSPACE}/helm/devopsodia-1.tgz -v"
            
        }
                }
            }
         }
    }
}
