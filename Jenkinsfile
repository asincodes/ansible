pipeline {
agent any
stages {
stage('Clone Repository') {
steps {
git branch: 'main',
url: 'https://github.com/asincodes/ansible'
}
stage('Check Maven') {
steps { sh 'mvn -version' }
stage('Clean Project') {
steps { sh 'mvn clean' }
}
}
}
stage('Build WAR') {
steps { sh 'mvn package' }
}
stage('Show WAR Location') {
steps {
sh 'pwd'
sh 'ls -lh target/'
}
}
stage('Deploy Using Ansible') {
steps {
sh '''
ansible-playbook \
-i ansible/hosts \
ansible/deploy.yml
'''
}
}
stage('Verify Deployment') {
steps {
sh '''
sleep 15
curl http://localhost:9090/maven-webapp/
'''
}
}
stage('Success') {
steps {
echo 'WAR deployed successfully'
}
}
}
post {
success { echo 'PIPELINE SUCCESS' }
failure { echo 'PIPELINE FAILED' }
always { echo 'PIPELINE FINISHED' }
}
}
