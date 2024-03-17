pipeline{

    agent any

    environment {
        DATE = new Date().format('yy.M')
        TAG = "${DATE}.${BUILD_NUMBER}"
    }


    stages{

        stage('Run Tests'){
            steps{
                bat 'docker-compose up'
            }
        }

        stage('Selenium down'){
            steps{
                bat 'docker-compose down'
            }
        }



    }



}
