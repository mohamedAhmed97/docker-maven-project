@Library("First-SL")
def gv
pipeline{
    agent any
    tools{
        maven 'Maven'
    }
    stages{
        stage("bulding jar"){
            when{
                experssion{
                    BRANCH_NAME=="stage"
                }
            }
            steps{
                script{
                   buildJar()
                }
            }
        }
           stage("testing"){
            when{
                experssion{
                    BRANCH_NAME=="test"
                }
            }
            steps{
                script{
                    echo "========testing jar========"
                    // sh 'mvn package'
                }
            }
        }
        stage("bulding image"){
            steps{
                 when{
                    experssion{
                        BRANCH_NAME=="production"
                    }
             }
                script{
                    echo "=========== bulding image ============"
                    buildImage()
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