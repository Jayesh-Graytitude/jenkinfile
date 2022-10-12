pipeline {
    agent any
    parameters {
      string description: 'Enter the name for new Git repo', name: 'RepoName', trim: true
      string defaultValue: '/home/ubuntu',description: 'Workspace on USS', name: 'Workspace', trim: true
      string defaultValue: 'Jayesh-Graytitude',description: 'Enter your git username', name: 'USER', trim: true
      password defaultValue: '', description: 'Enter TOKEN for your Git repository', name: 'TOKEN'
	  choice choices: ['true', 'false'], name: 'PublicRepo'
    }
    stages {
        stage('Create New Repo') {
            steps {
                echo "Creating new repo ${RepoName}"
// Jayesh - Update the script to use credentials as secret from Jenkins and not display the same in logs				
                    sh '''
                       curl -X POST -u ${USER}:${TOKEN} https://api.github.com/user/repos \
                       -d '{"name": "'$RepoName'","description":"Creating new repository '$RepoName'", \
					   "auto_init":"true","public":"'$PublicRepo'"}' | grep -m 1 clone \
					   | grep -Eo "(http|https)://[a-zA-Z0-9./?=_%:-]*" > temp.txt
                    '''
            }	
        }
        stage('Clone Repo to USS') {
// Jayesh - Update the script to change the user who can access the desired path on USS from Jenkins credentials
// Also add logic to clone private repos using the credentials
            steps {
                script {
                    env.Newurl = readFile 'temp.txt'
                }
                echo "${env.Newurl}"
		        sh "pwd"
                dir('/tmp/jenkins-temp') {
                    sh "pwd"
		            sh "git clone ${env.Newurl}"
		            sh "ls -l"
//              }
                sh "pwd"
            }
        }
        stage('Migrate from Mainframe') {
// Jayesh - Switch the path to migrate file and trigger migration from MF to USS
// Test this by creating a sepearate shell script
            steps {
                echo 'Deploying....'
            }
        }
    }
}