pipeline {
  agent any
  stages {
    stage('git scm update') {
      steps {
        git url: 'https://github.com/beomtaek/cicdtest.git', branch: 'main'
      }
    }
    stage('docker build and push') {
      steps {
        sh '''
        sudo docker build -t blowswhl/cicdtest:green .
        sudo docker push blowswhl/cicdtest:green
        '''
      }
    }
    stage('deploy and service') {
      steps {
        sh '''
        ansible master -m command -a 'kubectl create deploy web-green --replicas=3 --image=blowswhl/cicdtest:green'
        ansible master -m command -a 'kubectl expose deploy web-green --type=LoadBalancer --port=80 --target-port=80 --name=web-green-svc'
        '''
      }
    }
  }
}
