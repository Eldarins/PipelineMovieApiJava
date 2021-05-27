pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
        jdk "JDK11"
    }
	
	parameters {
		string(name: 'BRANCH', defaultValue: 'master', description: 'Git branch of the Java Project')
	}

    stages {
        stage('Clean Compile') {
            steps {
                // Get some code from a GitHub repository
                git url: 'https://github.com/matthcol/movieapijava2021.git',
                    branch: "$params.BRANCH"
                

                // Run Maven on a Unix agent.
                sh "/home/srvadmin/tools/apache-maven-3.8.1/bin/mvn clean compile"

            }          
        }
		 stage('Test') {
            steps {              
                // Run Maven on a Unix agent.
                sh "/home/srvadmin/tools/apache-maven-3.8.1/bin/mvn test"

            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    
                }
            }
        }
		 stage('Package') {
            steps {
                // Run Maven on a Unix agent.
                sh "/home/srvadmin/tools/apache-maven-3.8.1/bin/mvn -DskipTests package"
				archiveArtifacts 'target/*.jar'
				sh "cp target/*.jar /home/srvadmin/RepoArtifacts/"
            }
        }
    }
}
