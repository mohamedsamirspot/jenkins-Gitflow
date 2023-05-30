pipeline {
    agent { label 'my-slave-label-onWhich-i-will-choose-to-run-the-pipeline-on-it' }
    parameters {
        string(name: 'BRANCH_NAME', defaultValue: '', description: 'Branch name')
    }
    stages {
        stage('build') {
            steps {
                echo 'build'
                script {
                    if (params.BRANCH_NAME == 'release') {
                        withCredentials([usernamePassword(credentialsId: 'my-docker-hub-cred', usernameVariable: 'MY_USERNAME', passwordVariable: 'MY_PASSWORD')]) {
                            sh '''
                                docker login -u ${MY_USERNAME} -p ${MY_PASSWORD}
                                docker build -t mohamedsamirebrahim/carrepair:v${BUILD_NUMBER} .
                                docker push mohamedsamirebrahim/carrepair:v${BUILD_NUMBER}
                                echo ${BUILD_NUMBER} > ../build.txt
                            '''
                        }
                    }
                    else {
                        echo "user chose ${params.BRANCH_NAME}"
                    }
                }
            }
        }
        stage('deploy') {
            steps {
                echo 'deploy'
                script {
                    if (params.BRANCH_NAME == 'dev' || params.BRANCH_NAME == 'test' || params.BRANCH_NAME == 'prod') {
                        withCredentials([file(credentialsId: 'kubeconfig-for-my-slave', variable: 'MY_KUBECONFIG')]) {
                            sh '''
                                export BUILD_NUMBER=$(cat ../build.txt)
                                mv Deployment/deploy.yaml Deployment/deploy.yaml.tmp
                                cat Deployment/deploy.yaml.tmp | envsubst > Deployment/deploy.yaml
                                rm -f Deployment/deploy.yaml.tmp
                                kubectl apply -f Deployment --kubeconfig ${MY_KUBECONFIG} -n ${params.BRANCH_NAME}
                            '''
                        }
                    }
                    else {
                        echo "user chose ${params.BRANCH_NAME}"
                    }
                }
            }
        }
    }
}
