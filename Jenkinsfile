pipeline{
    agent { label 'tech' }
    stages{

        stage('代码扫描'){
          steps{
              sh """
                 mvn sonar:sonar \
                    -Dsonar.projectKey=exporter2 \
                    -Dsonar.host.url=http://60.205.224.183:9000 \
                    -Dsonar.login=ae78faed08f37e8344f3a427be7ca585c05ca40b
              """
          }
        }

        stage('执行单侧'){
            steps{
                sh """
                    mvn clean test -Dmaven.test.failure.ignore=true
                """
                }
        }

        stage('打包推送'){
            steps{
                  sh """
                  mvn package deploy
                  """
            }
        }

        stage('编译镜像'){
            steps{
                sh """
                    sudo docker build -t 60.205.224.183:5000/java-exporter .
                    sudo docker push 60.205.224.183:5000/java-exporter
                """
                }
        }


        stage('部署环境'){
            steps{
                sh """
                    sudo docker pull 60.205.224.183:5000/java-exporter
                    sudo docker rm -f java-exporter || echo 'There is no java-exporter running'
                    sudo docker run --name java-exporter -d -p 8999:1234 60.205.224.183:5000/java-exporter
                """
                }
            }
    }
    post{
        always{
            sh 'echo 111'
        }
    }
}
