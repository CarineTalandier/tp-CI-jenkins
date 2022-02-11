pipeline {
    agent any

    stages {
		stage('Clean Jenkins Workspace before starting'){
            steps{
                cleanWs()
            }
        }
        stage('Pull') {
			steps {
				git([url:'https://github.com/CarineTalandier/tp-CI-jenkins', branch:'main'])
			}
		}
		stage('Run integration test'){
			steps {
				bat 'cd back && npm i && npm test && cd ..'
			}
		}
		stage('Update Back-end Docker image to latest'){
			steps {
				bat 'docker-compose pull back'
			}
		}
		stage('Update Front-end Docker image to latest'){
			steps {
				bat 'docker-compose pull front'
			}
		}
		stage('Deploy services'){
			steps {
				bat 'docker-compose up --remove-orphans --build --no-start'
			}
		}
    }
	post{
		always{
			bat 'docker-compose down --rmi "all" -v --remove-orphans'
		}
	}
}