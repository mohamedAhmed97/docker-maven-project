pipeline{
    agent any
    tools{
        maven 'Maven'
    }
    stages{
        stage("prepration"){
            steps{
                script{
                    echo "=============increment pom version=========="
                    sh "Maven build-helper:parse-version versions:set -DnewVersion=\${parsedVersion.majorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.nextIncrementalVersion} versions:commit"
                    def matcher=readFile("pom.xml")=~'<version>(.+)</version>'
                    def version=matcher[0][1]
                    env.IMAGE_VERSION = "$version-$BUILD_NUMBER"
                }
            }
        }
        stage("bulding application"){
            steps{
                  script{
                     echo "=========building jar==========="
                     sh "Maven clean package"
                     sh "Maven package"
                  }
            }
        }
        stage("bulding image"){
            steps{
                script{
                    echo "=========== bulding image ============"
                    withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh "docekr bulid -t mar97/first-repositary:$IMAGE_VERSION ."
                        sh "echo $PASSWORD | docker login -u $USERNAME  --password-stdin"
                        sh "docekr push  mar97/first-repositary:$IMAGE_VERSION"
                        }
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
