#!groovy

stage 'Dev'
node {
    checkout scm
    mvn 'clean package'
    dir('target') {stash name: 'war', includes: '*.war'}
    echo ${BRANCH_NAME}
}

def mvn(args) {
    sh "${tool 'localMaven'}/bin/mvn ${args}"
}