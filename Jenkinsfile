/*
** Variables.
*/
def serie = '20.10'
def maintenanceBranch = "${serie}.x"
if (env.BRANCH_NAME.startsWith('release-')) {
  env.BUILD = 'RELEASE'
} else if ((env.BRANCH_NAME == 'master') || (env.BRANCH_NAME == maintenanceBranch)) {
  env.BUILD = 'REFERENCE'
} else {
  env.BUILD = 'CI'
}

/*
** Pipeline code.
*/
stage('Source') {
  node {
    sh 'setup_centreon_build.sh'
    dir('centreon-connector') {
      checkout scm
    }
    sh "./centreon-build/jobs/connector/${serie}/mon-connector-source.sh"
    source = readProperties file: 'source.properties'
    publishHTML([
      allowMissing: false,
      keepAll: true,
      reportDir: 'summary',
      reportFiles: 'index.html',
      reportName: 'Centreon Connector Build Artifacts',
      reportTitles: ''
    ])
    env.VERSION = "${source.VERSION}"
    env.RELEASE = "${source.RELEASE}"
  }
}

try {
  stage('Sonar analysis') {
    node {
      sh 'setup_centreon_build.sh'
      /* unittest.sh does not exist currently. ADD the missing script before restoring this part

      sh "./centreon-build/jobs/connector/${serie}/mon-connector-unittest.sh centos7"
      step([
      $class: 'XUnitBuilder',
        thresholds: [
          [$class: 'FailedThreshold', failureThreshold: '0'],
          [$class: 'SkippedThreshold', failureThreshold: '0']
        ],
        tools: [[$class: 'GoogleTestType', pattern: 'ut.xml']]
      ])
      */

      withSonarQubeEnv('SonarQubeDev') {
        sh "./centreon-build/jobs/connector/${serie}/mon-connector-analysis.sh"
      }
    }
    timeout(time: 10, unit: 'MINUTES') {
      def qualityGate = waitForQualityGate()
      if (qualityGate.status != 'OK') {
        currentBuild.result = 'FAIL'
      }
    }
    if ((currentBuild.result ?: 'SUCCESS') != 'SUCCESS') {
      error("Quality gate failure: ${qualityGate.status}.");
    }
  }

  stage('Package') {
    parallel 'centos7': {
      node {
        sh 'setup_centreon_build.sh'
        sh "./centreon-build/jobs/connector/${serie}/mon-connector-package.sh centos7"
      }
    },
    'centos8': {
      node {
        sh 'setup_centreon_build.sh'
        sh "./centreon-build/jobs/connector/${serie}/mon-connector-package.sh centos8"
      }
    }
    if ((currentBuild.result ?: 'SUCCESS') != 'SUCCESS') {
      error('Package stage failure.');
    }
  }

  if ((env.BUILD == 'RELEASE') || (env.BUILD == 'REFERENCE')) {
    stage('Delivery') {
      node {
        sh 'setup_centreon_build.sh'
        sh "./centreon-build/jobs/connector/${serie}/mon-connector-delivery.sh"
      }
      if ((currentBuild.result ?: 'SUCCESS') != 'SUCCESS') {
        error('Delivery stage failure.');
      }
    }
  }
}
finally {
  buildStatus = currentBuild.result ?: 'SUCCESS';
  if ((buildStatus != 'SUCCESS') && ((env.BUILD == 'RELEASE') || (env.BUILD == 'REFERENCE'))) {
    slackSend channel: '#monitoring-metrology', message: "@channel Centreon Connector build ${env.BUILD_NUMBER} of branch ${env.BRANCH_NAME} was broken by ${source.COMMITTER}. Please fix it ASAP."
  }
}
