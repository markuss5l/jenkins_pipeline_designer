pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        sh '''def scmVars = checkout changelog: false, poll: false, scm: [$class: \'SubversionSCM\', additionalCredentials: [], excludedCommitMessages: \'\', excludedRegions: \'\', excludedRevprop: \'\', excludedUsers: \'\', filterChangelog: false, ignoreDirPropChanges: false, includedRegions: \'\', locations: [[cancelProcessOnExternalsFail: false, credentialsId: \'91f59001-ca5d-4eb3-aec7-abd492f8364d\', depthOption: \'infinity\', ignoreExternalsOption: false, local: \'.\', remote: \'https://subver.sulzer.de/svn/rep/MANtec/code/trunk@HEAD\']], quietOperation: true, workspaceUpdater: [$class: \'UpdateUpdater\']]
environment { 
    SVN_REV_NUMBER = scmVars.SVN_REVISION
}
'''
      }
    }
    stage('Build') {
      steps {
        bat '"${tool \'MSBuild Tools 2017\'}" ManTec.sln /m'
      }
    }
    stage('Test') {
      parallel {
        stage('Test') {
          steps {
            bat '"C:\\\\Program Files (x86)\\\\NUnit 3.8\\\\nunit-console\\\\nunit3-console.exe" .\\\\Test\\\\eu.man.mantec.test\\\\bin\\\\Debug\\\\eu.man.mantec.test.dll --result=TestResults.xml;format=nunit3'
          }
        }
        stage('') {
          steps {
            bat 'powershell -executionpolicy RemoteSigned -file C:\\\\Deploy\\\\Publish.ps1 -Deploy development -SkipBackup $false -Settings C:\\\\Deploy\\\\configs\\\\mantec.xml -Function CreatePackage -Version 4_0_${env.BUILD_ID}'
          }
        }
      }
    }
  }
}