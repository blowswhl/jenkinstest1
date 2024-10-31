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
        docker login -u blowswhl -p dhaprkahs1!
        docker build -t blowswhl/cicdtest:green .
        docker push blowswhl/cicdtest:green
        '''
      }
    }
    stage('docker image pull') {
      steps {
        sh '''
        ansible-playbook -i /etc/ansible/hosts /home/user1/node-docker.yml -u user1
        '''
      }
    }
    stage('deploy and service') {
      steps {
        sh '''
        ansible master -m command -a 'sudo kubectl create deploy web-green --replicas=3 --image=blowswhl/cicdtest:green' -u user1
        ansible master -m command -a 'sudo kubectl expose deploy web-green --type=LoadBalancer --port=80 --target-port=80 --name=web-green-svc' -u user1
        '''
      }
    }
  }
}
