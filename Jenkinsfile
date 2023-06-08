pipeline {
    agent { label 'my-slave-label-onWhich-i-will-choose-to-run-the-pipeline-on-it' }
    stages {
        stage('build') {
            steps {
                echo 'build'
                script {
                    withCredentials([usernamePassword(credentialsId: 'my-docker-hub-cred', usernameVariable: 'MY_USERNAME', passwordVariable: 'MY_PASSWORD')]) {
                        sh '''
                            docker login -u ${MY_USERNAME} -p ${MY_PASSWORD}
                            docker build -t mohamedsamirebrahim/carrepair${BRANCH_NAME}:v${BUILD_NUMBER} .
                            docker push mohamedsamirebrahim/carrepair${BRANCH_NAME}:v${BUILD_NUMBER}
                            echo ${BUILD_NUMBER} > ../build.txt
                            echo ${BRANCH_NAME} > ../branchname.txt
                        '''
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                echo 'deploy'
                script {
                    withCredentials([file(credentialsId: 'kubeconfig-for-my-slave', variable: 'MY_KUBECONFIG')]) {
                        sh '''
                            export BUILD_NUMBER=$(cat ../build.txt)
                            mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                            cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                            rm -f Deployment/deploy.yaml.tmp

                            export BRANCH_NAME=$(cat ../branchname.txt)
                            mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                            cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                            rm -f Deployment/deploy.yaml.tmp
                            kubectl apply -f Deployment --kubeconfig ${MY_KUBECONFIG} -n ${BRANCH_NAME}
                        '''
                    }
                }
            }
        }
    }
}
