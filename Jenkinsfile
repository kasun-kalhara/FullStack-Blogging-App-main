pipeline {
    agent any
    
    tools{
        jdk "jdk17"
        maven "maven3"
    }
    
    environment {
        SCANNER_HOME = tool "sonar-scanner"
    }

    stages {
        stage('git checkout') {
            steps {
                git branch: 'main', credentialsId: 'git-cred', url: 'https://github.com/kasun-kalhara/FullStack-Blogging-App-main.git'
            }
        }
        stage('compile') {
            steps {
                sh "mvn compile"
            }
        }
        stage('test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('trivy fs scan') {
            steps {
                sh 'trivy fs --format table -o fs.html .'
            }
        }
        stage('sonarqube analyzis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                     sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Bloggind-app1 -Dsonar.projectKey=Blogging-app1 \
                            -Dsonar.java.binaries=. '''
                }
            }
        }
        stage('build') {
            steps {
                sh 'mvn package'
            }
        }
        stage('publish artifacts') {
            steps {
                withMaven(globalMavenSettingsConfig: 'maven-settings', jdk: 'jdk17', maven: 'maven3', mavenSettingsConfig: '', traceability: true) {
                    sh "mvn deploy"
                }
            }
        }
        stage('docker build & tag') {
            steps {
                script{
                   withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                      sh "docker build -t kasunkalharabamunuarachchi123/blogging-app:latest ."
                   }
                }
            }
        }
        stage('docker push') {
            steps {
                script{
                   withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                      sh "docker push kasunkalharabamunuarachchi123/blogging-app:latest"
                   }
                }
            }
        }
        stage('k8-deploy') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: ' devopsshack-cluster', contextName: '', credentialsId: 'k8-cred', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://6CC139CC399004A9C88AC7AB4131E29E.gr7.us-east-1.eks.amazonaws.com') {
                    sh "kubectl apply -f deployment-service.yml"
                }
            }
        }
        stage('verifiy deployment') {
            steps {
                withKubeConfig(caCertificate: '', clusterName: ' devopsshack-cluster', contextName: '', credentialsId: 'k8-cred', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://6CC139CC399004A9C88AC7AB4131E29E.gr7.us-east-1.eks.amazonaws.com') {
                    sh "kubectl apply -f deployment-service.yml"
                    sh "kubectl get pods"
                    sh "kubectl get svc"
                }
            }
        }
        
    }
    post {
    always {
        script {
            def jobName = env.JOB_NAME
            def buildNumber = env.BUILD_NUMBER
            def pipelineStatus = currentBuild.result ?: 'UNKNOWN'
            def bannerColor = pipelineStatus.toUpperCase() == 'SUCCESS' ? 'green' : 'red'

            def body = """
                <html>
                <body>
                <div style="border: 4px solid ${bannerColor}; padding: 10px;">
                <h2>${jobName} - Build ${buildNumber}</h2>
                <div style="background-color: ${bannerColor}; padding: 10px;">
                <h3 style="color: white;">Pipeline Status: ${pipelineStatus.toUpperCase()}</h3>
                </div>
                <p>Check the <a href="${BUILD_URL}">console output</a>.</p>
                </div>
                </body>
                </html>
            """

            emailext (
                subject: "${jobName} - Build ${buildNumber} - ${pipelineStatus.toUpperCase()}",
                body: body,
                to: 'kalharakasun430@gmail.com',
                from: 'jenkins@example.com',
                replyTo: 'jenkins@example.com',
                mimeType: 'text/html',
                attachmentsPattern: 'trivy-image-report.html'
            )
        }
    }
 }
}
