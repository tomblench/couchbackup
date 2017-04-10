stage('Build') {
    // Checkout, build
    node {
        checkout scm
        sh 'npm install'
        stash name: 'built'
    }
}

stage('QA') {
    node {
        // Unstash the built content
        unstash name: 'built'
        // run tests using creds
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'clientlibs-test', usernameVariable: 'DB_USER', passwordVariable: 'DB_PASSWORD']]) {
            // run unit tests
            sh 'npm test'
            // backup animaldb
            sh './bin/couchbackup.bin.js --url https://$DB_USER:$DB_PASSWORD@$DB_USER.cloudant.com --db animaldb  > /tmp/out'
        }
    }
}
