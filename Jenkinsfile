pipeline {
    agent any

    tools {
        nodejs "node"
    }

    stages {

        stage('increment version') {
            steps {
                script {
                    dir("app") {
                        sh "npm version minor --no-git-tag-version"
                        def packageJson = readJSON file: 'package.json'
                        def version = packageJson.version
                        env.IMAGE_NAME = "${version}-${BUILD_NUMBER}"
                    }
                }
            }
        }

        stage('Run tests') {
            steps {
                script {
                    dir("app") {
                        sh "npm install"
                        sh "npm run test"
                    }
                }
            }
        }

        stage('Build and Push docker image') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'docker-hub-repo',
                        usernameVariable: 'USER',
                        passwordVariable: 'PASS'
                    )
                ]) {
                    dir("app") {
                    sh "docker build -t ndubuisip/demo-app:${IMAGE_NAME} ."
                    }
                    sh 'echo "$PASS" | docker login -u "$USER" --password-stdin'
                    sh "docker push ndubuisip/demo-app:${IMAGE_NAME}"
                }
            }
        }

        stage('deploy to EC2') {
            steps {
                script {
                    def shellCmd = "bash ./server-cmds.sh ${IMAGE_NAME}"
                    def ec2Instance = "ec2-user@34.207.209.148"

                    sshagent(['ec2-key-server']) {
                        sh "scp -o StrictHostKeyChecking=no server-cmds.sh ${ec2Instance}:/home/ec2-user"
                        sh "scp -o StrictHostKeyChecking=no docker-compose.yaml ${ec2Instance}:/home/ec2-user"
                        sh "ssh -o StrictHostKeyChecking=no ${ec2Instance} '${shellCmd}'"
                    }
                }
            }
        }

        stage('commit version update') {
            when {
                expression { return env.BRANCH_NAME == 'jenkins-ci/cd' }
            }
            steps {
                script {
                    withCredentials([
                        usernamePassword(
                            credentialsId: 'GitHub-Credentials',
                            usernameVariable: 'USER',
                            passwordVariable: 'PASS'
                        )
                    ]) {
                        sh 'git config user.email "ndu2okoma@gmail.com"'
                        sh 'git config user.name "OkomaNdu"'

                        sh """
                            git remote set-url origin https://${USER}:${PASS}@github.com/OkomaNdu/AWS-Service-NodeJs-Project.git
                        """

                        sh 'git add app/package.json'
                        sh 'git commit -m "ci: version bump" || echo "No changes to commit"'
                        sh "git push origin HEAD:${BRANCH_NAME}"
                    }
                }
            }
        }
    }
}

