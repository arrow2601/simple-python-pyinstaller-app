// pipeline {
//     agent none
//     options {
//         skipStagesAfterUnstable()
//     }
//     stages {
//         stage('Build') {
//             agent {
//                 docker {
//                     image 'python:2-alpine'
//                 }
//             }
//             steps {
//                 sh 'python -m py_compile sources/add2vals.py sources/calc.py'
//                 stash(name: 'compiled-results', includes: 'sources/*.py*')
//             }
//         }
//         stage('Test') {
//             agent {
//                 docker {
//                     image 'qnib/pytest'
//                 }
//             }
//             steps {
//                 sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
//             }
//             post {
//                 always {
//                     junit 'test-reports/results.xml'
//                 }
//             }
//         }
//         stage('Deliver') { 
//             agent any
//             environment { 
//                 VOLUME = '$(pwd)/sources:/src'
//                 IMAGE = 'cdrx/pyinstaller-linux:python2'
//             }
//             steps {
//                 dir(path: env.BUILD_ID) { 
//                     unstash(name: 'compiled-results') 
//                     sh "docker run --rm -v ${VOLUME} ${IMAGE} 'pyinstaller -F add2vals.py'" 
//                 }
//             }
//             post {
//                 success {
//                     archiveArtifacts "${env.BUILD_ID}/sources/dist/add2vals" 
//                     sh "./${env.BUILD_ID}/sources/dist/add2vals 25 25"
                    
//                     sh "docker run --rm -v ${VOLUME} ${IMAGE} 'rm -rf build dist'"
//                 }
//             }
//         }
//     }
// }


node {

    stage('Checkout') {
        git 'https://github.com/arrow2601/simple-python-pyinstaller-app.git'
     }

    stage('Build') {
        docker.image('python:2-alpine').inside {
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            stash(name: 'compiled-results', includes: 'sources/*.py*')
        }
    }

    stage('Test') {
        docker.image('qnib/pytest').inside {
            sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
        }
        junit 'test-reports/results.xml'
    }

    stage('Deliver') {
        def VOLUME = "${pwd()}/sources:/src"
        def IMAGE = 'cdrx/pyinstaller-linux:python2'

        dir("${env.BUILD_ID}") {
            unstash(name: 'compiled-results')
            sh "docker run --rm -v ${VOLUME} ${IMAGE} 'pyinstaller -F add2vals.py'"
        }
        sh "./sources/dist/add2vals 25 26"
        archiveArtifacts artifacts: "sources/dist/add2vals", fingerprint: true
        sh "sleep 1m"
        sh "docker run --rm -v ${VOLUME} ${IMAGE} 'rm -rf build dist'"
    }
   
}


// node {
//     stage('Checkout') {
//         git 'https://github.com/arrow2601/simple-python-pyinstaller-app.git'
//     }

//     stage('Build') {
//         docker.image('python:2-alpine').inside {
//             sh 'python -m py_compile sources/add2vals.py sources/calc.py'
//         }
//     }
//         stage('Test') {
//         docker.image('qnib/pytest').inside {
//             sh 'python -m py_compile sources/add2vals.py sources/calc.py'
//             sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            
//         }
//     }

//     stage('Mannual Approval') {
//         input message: 'Lanjutkan ke tahap Deploy?'
//     }

//      stage('Deliver') {
//         def VOLUME = "${pwd()}/sources:/src"
//         def IMAGE = 'cdrx/pyinstaller-linux:python2'

//         dir("${env.BUILD_ID}") {
//             unstash(name: 'compiled-results')
//             sh "docker run --rm -v ${VOLUME} ${IMAGE} 'pyinstaller -F add2vals.py'"
//         }

//         archiveArtifacts "${env.BUILD_ID}/sources/dist/add2vals"
//         sh "docker run --rm -v ${VOLUME} ${IMAGE} 'rm -rf build dist'"
//     }
//     sh ""
//         }

    //     stage('Deploy') {
    //     docker.image('cdrx/pyinstaller-linux:python2').inside {
    //         sh 'pyinstaller --onefile sources/add2vals.py'
    //     }
    //     sh 'sleep 1m'
    // }



// pipeline {
//     agent none
//     stages {
//         stage('Build') {
//             agent {
//                 docker {
//                     image 'python:2-alpine'
//                 }
//             }
//             steps {
//                 sh 'python -m py_compile sources/add2vals.py sources/calc.py'
//             }
//         }
//         stage('Test') {
//             agent {
//                 docker {
//                     image 'qnib/pytest'
//                 }
//             }
//             steps {
//                 sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
//             }
//             post {
//                 always {
//                     junit 'test-reports/results.xml'
//                 }
//             }
//         }
//         stage('Deliver') {
//             agent {
//                 docker {
//                     image 'cdrx/pyinstaller-linux:python2'
//                 }
//             }
//             steps {
//                 sh 'pyinstaller --onefile sources/add2vals.py'
//             }
//             post {
//                 success {
//                     archiveArtifacts 'dist/add2vals'
//                 }
//             }
//         }
//     }
// }


