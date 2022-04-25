pipeline{
    agent any
	
	triggers {
		pollSCM('H/5 * * * *')
	}
	
    stages{
        stage("Build API") {
			when {
				anyOf {
					changeset "**"
				}
			}
            steps{
                sh "docker-compose build frontend"
            }
			post {
				always{
					withCredentials([string(credentialsId: 'WEBHOOKURL', variable: 'WEBHOOK_URL')]) {
						discordSend description: "Jenkins Pipeline Build", footer: "Footer Text", link: env.BUILD_URL, result: currentBuild.currentResult, title: JOB_NAME, webhookURL: "${WEBHOOK_URL}"
					}
				}
			}
        }
        stage("Clean containers") {
            steps {
                script {
                    try {
                        sh "docker-compose down"
                    }
                    finally { }
                }
            }
        }
        stage("Deploy") {
            steps {
                sh "docker-compose up -d"
            }
        }
    }
}
