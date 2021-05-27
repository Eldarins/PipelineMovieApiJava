pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
        jdk "JDK11"
    }

    stages {
        stage('Clean Compile') {
            steps {
                // Get some code from a GitHub repository
                git url: 'https://github.com/matthcol/movieapijava2021.git',
                    branch: 'dev'
                

                // Run Maven on a Unix agent.
                sh "/home/srvadmin/tools/apache-maven-3.8.1/bin/mvn clean compile"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }          
        }
		 stage('Test') {
            steps {              
                // Run Maven on a Unix agent.
                sh "/home/srvadmin/tools/apache-maven-3.8.1/bin/mvn test"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
		 stage('Build') {
            steps {
                // Run Maven on a Unix agent.
                sh "/home/srvadmin/tools/apache-maven-3.8.1/bin/mvn -DskipTests package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
    }
}
