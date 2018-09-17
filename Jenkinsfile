#!groovy

stage 'Dev'
node {
    checkout scm
    mvn 'clean package'
    dir('webapp/target') {stash name: 'war', includes: 'webapp.war'}
    echo ${BRANCH_NAME}
}

def mvn(args) {
    sh "${tool 'localMaven'}/bin/mvn ${args}"
}