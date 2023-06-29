node {
    stage('Build') {
        docker.image('python:2-alpine').inside {
            checkout scm
            sh 'pwd'
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    stage('Test') {
        docker.image('qnib/pytest').inside {
            checkout scm
            sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            junit 'test-reports/results.xml'
        }
    }
    stage('Deliver') {
        input message: 'Proceed to Deliver stage?'
        docker.image('cdrx/pyinstaller-linux:python2').inside {
            checkout scm
            sh 'pyinstaller --onefile sources/add2vals.py'
            archiveArtifacts 'dist/add2vals'
        }
    }
}