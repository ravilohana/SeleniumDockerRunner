pipeline{

    agent any

    environment {
        DATE = new Date().format('yy.M')
        TAG = "${DATE}.${BUILD_NUMBER}"
    }


    stages{

        stage('Start Grid'){
            steps{
                bat 'docker-compose -f grid.yaml up -d'
            }
        }

        stage('Run Selenium Test'){
            steps{
                bat 'docker-compose -f test_suites.yaml up'
            }
        }



    }
    post{
        always{
            bat 'docker-compose -f grid.yaml down'
            bat 'docker-compose -f test_suites.yaml down'
            archiveArtifacts artifacts: 'output/flight-reservation/emailable-report.html', followSymlinks: false
            archiveArtifacts artifacts: 'output/vendor-portal/emailable-report.html', followSymlinks: false
        }
    }



}
