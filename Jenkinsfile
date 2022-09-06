@Library("First-SL")
def gv
pipeline{
    agent any
    tools{
        maven 'Maven'
    }
    stages{
        stage("bulding jar"){
            steps{
                script{
                   buildJar()
                }
            }
        }
           stage("testing"){
            steps{
                script{
                    echo "========testing jar========"
                    // sh 'mvn package'
                }
            }
        }
        stage("bulding image"){
            steps{
                script{
                    echo "=========== bulding image ============"
                    bulidDockerImage "mar97/first-repositary:1.4"
                    dockerLogin()
                    dockerPush "mar97/first-repositary:1.4"
                }
            }
        }
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