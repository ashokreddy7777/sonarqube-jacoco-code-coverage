#!groovy
@Library('JSL@feature/sonar')_ 

pipeline {
    agent any
    options {
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '7', numToKeepStr: '5')
    }
    parameters {
        booleanParam defaultValue: true, description: 'execute sonarqube analysis. ', name: 'sonar_analysis'
    }
    stages {
        stage('gradle_build') {
            environment {
                gradleHome = tool 'gradle'
            }
            steps {
                sh "${gradleHome}/bin/gradle build"
            }
        }
        stage('sonar_analysis') {
            when { environment name: 'sonar_analysis', value: 'true'}
            steps {
                runSonar()
            }
        }
        stage('cleanup') {
            steps {
                cleanWs()
            }
        }
    }
    post {
        failure {
            sendNotifications("${currentBuild.currentResult}", "ashokvardhanreddy96@gmail.com")
        }
    }
}