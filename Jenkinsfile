pipeline{
    agent any
        tools {
            maven "maven"
            jdk "java"
        }

    environment {
        SNAP_REPO = 'tynybay-snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = '123'
        RELEASE_REPO = 'tynybay-release'
        CENTRAL_REPO = 'tynybay-central'
        NEXUSIP = '172.31.27.143'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'tynybay-group'
        NEXUS_LOGIN = 'nexus-id'
        SONARSERVER = 'sonar'
        SONARSCANNER ='sonarscan'

    }    

    

        stages {
            stage('Build'){
                steps {
                    sh 'mvn -s settings.xml -DskipTests install'
                }
                post {
                    success {
                        echo "now achiving"
                        archiveArtifacts artifacts: '**/*.war'
                    } 
                }
            }
            stage('Test'){
                steps {
                    sh 'mvn -s settings.xml test'
                }
            }
            stage('Checkstyle Analysis'){
                steps {
                    sh 'mvn -s settings.xml checkstyle:checkstyle'
                }
            }
            stage('sonar analysis'){
                environment {
                    scannerHome = tool "${SONARSCANNER}"

                }
                steps{
                    withSonarQubeEnv("${SONARSERVER}") {
                        sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                        -Dsonar.projectName=vprofile-repo \
                        -Dsonar.projectVersion=1.0 \
                        -Dsonar.sources=src/ \
                        -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                        -Dsonar.junit.reportsPath=target/surefire-reports/ \
                        -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                        -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml''' 
                    }
                }
            }

        
        }
    
}