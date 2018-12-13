pipeline {
	agent any

	parameters {
		string(name: 'tomcat_dev', defaultValue: '54.212.67.74', description: 'Staging Server')
		string(name: 'tomcat_prod', defaultValue: '54.184.202.103', description: 'Production Server')
	}

	triggers {
		pollSCM('* * * * *')
	}

	stages {

		stage('Build'){
			steps {
				bat 'mvn clean package'
			}
			post {
				success {
					echo 'Now Archiving...'
					archiveArtifacts artifacts: '**/target/*.war'
				}
			}
		}
		
		stage ('Deployments'){
			parallel {
				stage('Deploy to Staging'){
					steps {
						bat "copy -i C:\\Softwares\\cmder\\tomcatdemo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
					}
				}

				stage('Deploy to Production'){
					steps {
						bat "copy -i C:\\Softwares\\cmder\\tomcatdemo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
					}
				}
			}
		}
	}

}