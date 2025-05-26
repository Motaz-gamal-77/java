@Library('libx')_

pipeline{
    agent {
        label 'any'
    }

    environment{
        DOCKER_USER = credentials('dockerhub-user')
        DOCKER_PASS = credentials('dockerhub-password')
    }
    stages{
        stage("Build java app"){
            steps{
                sh "mvn clean package install"
            }
        }
        stage("build java app image"){
            steps{
                script{
                    def dockerx = new org.iti.docker()
                    dockerx.build("java", "${BUILD_NUMBER}")
                }
                sh "docker login -u ${DOCKER_USER} -p ${DOCKER_PASS} "
            }
        }
        stage("push java app image"){
            steps{
                script{
                    def dockerx = new org.iti.docker()
                    dockerx.login("${DOCKER_USER}", "${DOCKER_PASS}")
                    dockerx.push("java","${DOCKER_USER}", "${BUILD_NUMBER}")
                }
            }
        }
    }
}