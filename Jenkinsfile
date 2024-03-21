pipeline{

    agent any

    parameters {
        choice (name: 'BROWSER', choices: ['chrome', 'firefox', 'edge'], description: 'Select the browser.')

    }



    stages{

        stage('Start Grid'){
            steps{
                echo "BROWSER=${BROWSER}"
                bat "docker-compose -f grid.yaml up --scale ${params.BROWSER}=2 -d"
            }
        }

        stage('Run Selenium Test'){
            steps{
                bat "docker-compose -f test_suites.yaml up --pull always"
                script {
                    if(fileExists("./output/flight-reservation/testng-failed.xml") || fileExists("./output/vendor-portal/testng-failed.xml")){
                        error("failed tests found")
                    }
                }
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