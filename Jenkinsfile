pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
		jdk "java11"
        maven "maven3"
    }

    stages {
        stage('Clone') {
            steps {
                git credentialsId: 'GitHub_techonline0001', url: 'https://github.com/techonline0001/spring3-mvc-maven-xml-hello-world.git'
            }
		}
		stage('Build') {
            steps {
                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
		stage('Deploy') {
            steps {
                withCredentials([usernameColonPassword(credentialsId: 'tomcat_manager_app', variable: 'tomcat_cred')]) {
					sh "curl -v -u $tomcat_cred -T /var/lib/jenkins/workspace/spring3-app-pipeline/target/spring3-mvc-maven-xml-hello-world-1.0-SNAPSHOT.war 'http://34.207.136.65:8080/manager/text/deploy?path=/spring3app&update=true'"
				}
            }
        }
    }
    post {
		success {
			archiveArtifacts 'target/*.war'
		}
	}
}

