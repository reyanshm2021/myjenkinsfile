pipeline {
    agent any

    stages {
        stage('SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/reyanshm2021/wanderlust-main.git'
            }
        }
    }
}
