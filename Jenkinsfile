pipeline{
    agent any
    stages{
        stage("git checkout"){
            steps{
                echo "====++++executing git checkout++++===="
                git 'https://github.com/njokuifeanyigerald/jenkins_ansible_ec2_kubernetes.git'
            }
            post{
                success{
                    echo "====++++git checkout executed successfully++++===="
                }
                failure{
                    echo "====++++git checkout execution failed++++===="
                }
        
            }
        }
        stage("sending docker file to ansible server over ssh"){
            steps{
                echo "====++++executing sending docker file to ansible server over ssh++++===="
                script {
                    sshagent(['ansible_server']) {
                        sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.161'
                        sh 'scp /var/lib/jenkins/workspace/pipeline_demo/*  ubuntu@172.31.38.161:/home/ubuntu'  
                    }
                }
            }
            post{
                success{
                    echo "====++++sending docker file to ansible server over ssh executed successfully++++===="
                }
                failure{
                    echo "====++++sending docker file to ansible server over ssh execution failed++++===="
                }
        
            }
        }
        stage("docker build images"){
            steps{
                echo "====++++executing docker build images++++===="
                script{
                    sshagent(['ansible_server']) {
                        sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.161 cd /home/ubuntu'
                        sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.161 docker  build -t $JOB_NAME:V1.$BUILD_ID  --no-cache .'
                    }
                }
            }
            post{
                success{
                    echo "====++++docker build images executed successfully++++===="
                }
                failure{
                    echo "====++++docker build images execution failed++++===="
                }
        
            }
        }
        stage("docker image tagging"){
            steps{
                echo "====++++executing docker image tagging++++===="
                script{
                    sshagent(['ansible_server']) {
                        sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.161 cd /home/ubuntu'
                        sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.161 docker  tag  $JOB_NAME:V1.$BUILD_ID bopgeek/$JOB_NAME:V1.$BUILD_ID '
                        sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.161 docker  tag  $JOB_NAME:V1.$BUILD_ID bopgeek/$JOB_NAME:latest'
                    }
                }
            }
            post{
                success{
                    echo "====++++docker image tagging executed successfully++++===="
                }
                failure{
                    echo "====++++docker image tagging execution failed++++===="
                }
        
            }
        }

        stage("push docker to dockerhub"){
            steps{
                echo "====++++executing push docker to dockerhub++++===="
                script{
                    sshagent(['ansible_server']) {
                        withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhubcred')]) {
                            sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.161 docker login -u bopgeek -p ${dockerhubcred} '
                            sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.161 docker push bopgeek/$JOB_NAME:latest '
                            sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.161 docker rmi $JOB_NAME:V1.$BUILD_ID bopgeek/$JOB_NAME:V1.$BUILD_ID '
                            sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.161 docker rmi bopgeek/$JOB_NAME:latest  '
                        }
                    }
                }               
            }
            post{
                success{
                    echo "====++++push docker to dockerhub executed successfully++++===="
                }
                failure{
                    echo "====++++push docker to dockerhub execution failed++++===="
                }
        
            }
        }
        // stage("coping files to the web server"){
        //     steps{
        //         echo "====++++executing coping files to the web server++++===="
        //         script{
        //             sshagent(['kub']) {
        //                 sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.42.243 '
        //                 sh 'scp /var/lib/jenkins/workspace/pipeline_demo/*  ubuntu@172.31.42.243:/home/ubuntu'
        //             }
        //         }
        //     }
        //     post{
        //         success{
        //             echo "====++++coping files to the web server executed successfully++++===="
        //         }
        //         failure{
        //             echo "====++++coping files to the web server execution failed++++===="
        //         }
        
        //     }
        // }

        // stage("sending docker file to ansible server over ssh"){
        //     steps{
        //         echo "====++++executing sending docker file to ansible server over ssh++++===="
        //         script {
        //             sshagent(['ansible_server']) {
        //                 sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.161'
        //                 sh 'scp /var/lib/jenkins/workspace/pipeline_demo/*  ubuntu@172.31.38.161:/home/ubuntu'  
        //             }
        //         }
        //     }
        //     post{
        //         success{
        //             echo "====++++sending docker file to ansible server over ssh executed successfully++++===="
        //         }
        //         failure{
        //             echo "====++++sending docker file to ansible server over ssh execution failed++++===="
        //         }
        
        //     }
        // }


        // stage("Kubernetes Deployment using ansible"){
        //     steps{
        //         echo "====++++executing Kubernetes Deployment using ansible++++===="
        //         script{
        //             sshagent(credentials: ['ansible_server']) {
        //                 sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.161 cd /home/ubuntu'
        //                 sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.161 ansible -m ping node'
        //                 sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.161 ansible-playbook ansible.yml'
        //             }
        //         }
        //     }
        //     post{
        //         success{
        //             echo "====++++Kubernetes Deployment using ansible executed successfully++++===="
        //         }
        //         failure{
        //             echo "====++++Kubernetes Deployment using ansible execution failed++++===="
        //         }
        
        //     }
        // }


        
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}