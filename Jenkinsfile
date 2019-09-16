#!/usr/bin/env groovy
import java.net.URL
node{
    stage('git checkout'){
        git 'https://github.com/PramodhKumarM98/Jenkins_Pipeline_Example_Linux.git'
    }
    stage('Compile'){
        withMaven(maven:'MyMaven'){
            sh 'mvn compile'
        }
    }
    stage('Code Review'){
        try {
            withMaven(maven:'MyMaven'){
                sh 'mvn pmd:pmd'   
            }
        } finally {
            pmd canComputeNew: false, defaultEncoding: '', healthy: '', pattern: 'target/pmd.xml', unHealthy: ''
        }
    }
	stage('Test Code Compile'){
            withMaven(maven:'MyMaven'){
                sh 'mvn test-compile'   
            }
    }
    stage('Code Testing'){
        try {
            withMaven(maven:'MyMaven'){
                sh 'mvn test'   
            }
        } finally {
            junit 'target/surefire-reports/*xml'
        }
    }
     stage('Coverage Checks'){
        try {
            withMaven(maven:'MyMaven'){
                sh 'mvn cobertura:cobertura -Dcobertura.report.format=xml'   
            }
        } finally {
            cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false
        }
    }
    stage('Package'){
        withMaven(maven:'MyMaven'){
            sh 'mvn package'
        }
    }
}
