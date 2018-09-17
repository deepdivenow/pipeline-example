#!groovy
// Global Variables


node {

    if (env.BRANCH_NAME ~= '^feature') {
        checkout()
        build()
        unittest()
        codestyletest()
        codereview()
    }
    if (env.BRANCH_NAME == 'devel') {
        checkout()
        build()
        unitTest()
//        allCodeQualityTests()
        deploy_to_devel()
    }
    if (env.BRANCH_NAME == 'release') {
        checkout()
        build()
        allTests()
//        allCodeQualityTests()
        deploy_to_stage()
    }
    if (env.BRANCH_NAME == 'master') {
        checkout()
        build()
        allTests()
//        allCodeQualityTests()
        deploy_to_production()
    }
}


def checkout () {
    stage 'Checkout code'
    checkout scm
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
