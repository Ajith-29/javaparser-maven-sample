pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven"
    }

    stages {
        stage('hello') {
            steps {
                echo'hello world'
            }
        }
        stage('build') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/Ajith-29/javaparser-maven-sample.git']]])
                // Get some code from a GitHub repository
                //git 'https://github.com/jglick/simple-maven-project-with-tests.git'

                // Run Maven on a Unix agent.
                //sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                 bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            //post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                //success {
                    //junit '**/target/surefire-reports/TEST-*.xml'
                    //archiveArtifacts 'target/*.jar'
                }
                stage('test') {
                    steps {
                        bat'mvn test -Dmaven.test.failure.ignore=true'
                    }
                }
                stage('nexus') {
                    steps {
                        nexusArtifactUploader artifacts: [[artifactId: 'pom.javaparser-core', classifier: '', file: 'pom.xml', type: 'pom']], credentialsId: 'github', groupId: 'pom.com.github.javaparser', nexusUrl: 'localhost:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'nexusproject', version: 'pom.3.23.1'
                    }
                }
            }
        }
