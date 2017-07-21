pipeline {
  agent any
  triggers {
    cron('H/30 3-22 * * 0-7')
  }
  stages {
    stage('Initialize') {
      steps {
        echo 'Intializing Client Sync Build'
        ws('D:\\ifs\\SYNC_Pipleline\\Client\\Pub') {
          checkout poll: false, scm: [$class: 'SubversionSCM', additionalCredentials: [[credentialsId: 'svncredentials', realm: '<http://cmbsvn01:80> SVN Repo']], excludedCommitMessages: '', excludedRegions: '', excludedRevprop: '', excludedUsers: '', filterChangelog: false, ignoreDirPropChanges: false, includedRegions: '', locations: [[credentialsId: 'svncredentials', depthOption: 'infinity', ignoreExternalsOption: false, local: 'Checkout', remote: 'http://cmbsvn01/svn/ifsapp/projects/core-9.0/builds/sync-client']], workspaceUpdater: [$class: 'UpdateUpdater']]
        }        
      }
    }
    stage('Build') {
      steps {
        echo 'Starting MSBuild'
        ws('D:\\ifs\\SYNC_Pipleline\\Client\\Pub') {
          bat "if not exist ${WORKSPACE}\\Logs\\CompileClient md ${WORKSPACE}\\Logs\\CompileClient"
          bat "\"${tool 'MSBuild_40'}\" ${WORKSPACE}\\Checkout\\fndbas\\source\\default.build /P:Languages=en /FL /FLP:LogFile=${WORKSPACE}\\Logs\\CompileClient\\CompileClient.log;Verbosity=normal"
        }
      }
    }
    stage('Report') {
      steps {
        echo 'Publishing Artificats'
        ws('D:\\ifs\\SYNC_Pipleline\\Client\\Pub') {
          archiveArtifacts allowEmptyArchive: true, artifacts: 'Logs/CompileClient/CompileClient.log'
        }
      }
    }    
  }
}
