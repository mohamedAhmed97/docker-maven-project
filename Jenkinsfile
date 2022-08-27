pipeline{
    agent any
    tools{
        maven 'Maven'
    }
    stages{
        stage("bulding jar"){
            steps{
                script{
                    echo "========bulding jar========"
                    sh 'mvn package'
                }
            }
        }
        stage("bulding image"){
            steps{
                script{
                    echo "=========== bulding image ============"
                    withCredentials(['UsernamePassword', credentialsId:'docker-hub-credentials',
                        usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD'
                    ])

                    sh "docker bulid -t mar97/first-repositary:1.2 ."
                    sh "echo $PASSWORD | docker login -u $USERNAME --password-stdin"
                    sh "docker push mar97/first-repositary:1.2"
                }
            }
        }
    }
    steps{
        success{
            echo "Done ===============>"
        }
        failure{
            echo "Failure ============>"
        }
    }
    
}