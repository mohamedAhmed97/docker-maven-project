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
                    sh 'mvn build-helper:parse-version versions:set \
                    -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} versions:commit'
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
                     sh "mvn clean package"
                     sh "mvn package"
                  }
            }
        }
        stage("bulding image"){
            steps{
                script{
                    echo "=========== bulding image ============"
                    withCredentials([usernamePassword(credentialsId: 'docker-hub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh "docker build -t mar97/first-repositary:$IMAGE_VERSION ."
                        sh "echo $PASSWORD | docker login -u $USERNAME  --password-stdin"
                        sh "docker push  mar97/first-repositary:$IMAGE_VERSION"
                        }
                }
            }
        }
        stage("pushing to repo"){
            steps{
                script{

                        sh "git add ."
                        sh 'git commit -m "ci: version bumb "'
                        sshagent (credentials: ['github-ssh	']) {
                            sh 'git remote set-url origin git@github.com:mohamedAhmed97/docker-maven-project.git'
                            sh 'git push origin master'
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
