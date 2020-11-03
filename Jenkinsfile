pipeline{
    environment{
        repository = "https://github.com/SubZeroX13/cdn-dns-controller"
        repositoryCredentials = "Igor"
        imageName = "cdn-dns-controller" 
    }
    agent any
    options {
        timeout (time:1, unit: 'Hours')
    }
    stages {
        stage('git clone') {
            steps{
                git credentials: ${repositoryCredentials}
                    url: ${repository}
            }
        }
        stage('tests'){
            steps {
                sh 'python3 -m pytest tests/'
            }
        }
        stage('create docker image')
            steps{
                sh 'docker build -t cdn-dns-controller -f docker/Dockerfile .'
            }
        }
        stage('run docker') {
            steps{
                sh 'docker run -it -d -p 5000:5000 --rm --name cdn-dns-controller cdn-dns-controller'
            }
        }
        stage('sanity yest'){
            steps{
                sh 'curl localhost:5000/'
            }
        }
        stage('Cleanup'){
            steps{
                sh 'docker rm -f cdn-dns-controller'
            }
        }
    }
}