#!groovy
// Global Variables


node {
    println "Branch: ${env.BRANCH_NAME}"

    if (env.BRANCH_NAME ==~ /feature.*/) {
        try {
            checkout()
            build()
            unitTest()
            styleTest2() // For code style test example must by moved to allCodeQualityTests in future
            mergerequest() //PR
        } catch (e) { // Execute on fail
            echo 'This will run only if failed'
            // Since we're catching the exception in order to report on it,
            // we need to re-throw it, to ensure that the build is marked as failed
            throw e
        } finally { // Always execute
            notifyTeamLead()  // Notify Team lead in any negative situation
        }
    }
    if (env.BRANCH_NAME == 'dev') {
        checkout()
        build()
        unitTest()
//        allCodeQualityTests()
        deploy_to_dev()
        notifyQA()
        notifyTeamLead()
    }
    if (env.BRANCH_NAME == 'release') {
        checkout()
        build()
        allTests()
//        allCodeQualityTests()
        deploy_to_stage()
        notifyTeamLead()
        notifySecurity()
        deploy_to_preprod()
        notifyTeamLead()
        notifyCustomer()
    }
    if (env.BRANCH_NAME == 'master') {
        checkout()
        build()
        allTests()
//        allCodeQualityTests()
        deploy_to_production()
        notifyTeamLead()
    }
}

def notifyTeamLead()  {
// Notify Team lead in any negative situation
    stage 'notify TeamLead'
}

def notifyQA()  {
// Notify QA in any negative situation
    stage 'notify QA'
}

def notifySecurity() {
// Notify Security if Release ready for security test
    stage 'notify Security'
}

def notifyCustomer() {
// Notify Customer if Release ready for security test
    stage 'notify Customer'
}

def mergerequest() {
    stage 'Merge Request'
    // Merge Request to devel branch
    println 'Merge Request to dev branch'

}

def deploy_to_production() {
// Deploy to PROD
    stage 'Deploy to PROD'
    println 'Deploy to PROD'
}

def deploy_to_preprod(){
// Deploy to Pre-PROD
    stage 'Deploy to Pre-PROD'
}

def deploy_to_stage() {
// Deploy to Stage
    stage 'Deploy to Stage'
}

def deploy_to_dev() {
// Deploy to Devel
    stage 'Deploy to DEV'
}

def checkout () {
    stage 'Checkout code'
    checkout scm
// !!! Need correct section about update branch status to gitlab in all sections !!!
//    setBuildStatus ("${context}", 'Checking out completed', 'SUCCESS')
}

def build () {
    stage 'Build'
    mvn 'clean package -DskipTests=true -Dmaven.javadoc.skip=true -Dcheckstyle.skip=true -B -V'
}


def unitTest() {
    stage 'Unit tests'
    mvn 'test -B -Dmaven.javadoc.skip=true -Dcheckstyle.skip=true'
    if (currentBuild.result == "UNSTABLE") {
        sh "exit 1"
    }
}

def styleTest() {
   stage('Test CodeStyle')
   // Run the maven codestyle
   mvn '-Dmaven.test.failure.ignore checkstyle:checkstyle'
   step([$class: 'CheckStylePublisher',
      canRunOnFailed: true,
      defaultEncoding: '',
      healthy: '',
      pattern: '',
      unHealthy: '',
      useStableBuildAsReference: true
    ])
//   checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
}

def allTests() {
    stage 'All tests'
    // don't skip anything
    mvn 'test -B'
    step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
    if (currentBuild.result == "UNSTABLE") {
        // input "Unit tests are failing, proceed?"
        sh "exit 1"
    }
}

def mvn(args) {
    sh "${tool 'localMaven'}/bin/mvn ${args}"
}

/*  Example for testing code

def allCodeQualityTests() {
    stage 'Code Quality'
    lintTest()
    coverageTest()
    styleTest()
}

def lintTest() {
    context="continuous-integration/jenkins/linting"
    setBuildStatus ("${context}", 'Checking code conventions', 'PENDING')
    lintTestPass = true

    try {
        mvn 'verify -DskipTests=true'
    } catch (err) {
        setBuildStatus ("${context}", 'Some code conventions are broken', 'FAILURE')
        lintTestPass = false
    } finally {
        if (lintTestPass) setBuildStatus ("${context}", 'Code conventions OK', 'SUCCESS')
    }
}

def coverageTest() {
    context="continuous-integration/jenkins/coverage"
    setBuildStatus ("${context}", 'Checking code coverage levels', 'PENDING')

    coverageTestStatus = true

    try {
        mvn 'cobertura:check'
    } catch (err) {
        setBuildStatus("${context}", 'Code coverage below 90%', 'FAILURE')
        throw err
    }

    setBuildStatus ("${context}", 'Code coverage above 90%', 'SUCCESS')

}

*/
