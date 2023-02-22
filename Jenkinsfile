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
        }
    
}