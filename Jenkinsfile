#!groovy

stage 'Dev'
node {
    checkout scm
    mvn 'clean package'
    dir('webapp/target') {stash name: 'war', includes: 'webapp.war'}
    sh 'printenv'
}

def mvn(args) {
    sh "${tool 'localMaven'}/bin/mvn ${args}"
}