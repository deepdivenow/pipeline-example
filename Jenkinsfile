#!groovy

stage 'Dev'
node ('docker-cloud') {
    checkout scm
    mvn 'clean package'
    dir('target') {stash name: 'war', includes: '*.war'}
}

def mvn(args) {
    sh "${tool 'localMaven'}/bin/mvn ${args}"
}