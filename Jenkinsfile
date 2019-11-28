pipeline{
    environment {
             MVN_SET = credentials('settings-xml')
        }
    agent any

    stages{
        
        stage('PASO A NEXUS'){
            parallel{
                stage('SonarQube analysis') {
                    steps{
                           checkout([$class: 'GitSCM', branches: [[name: 'master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'jacky', url: 'https://github.com/jacky9595/jenkins-nexus.git']]])
                           withSonarQubeEnv('mysonar') {
                            env.SQ_HOSTNAME = SONAR_HOST_URL;
                            env.SQ_AUTHENTICATION_TOKEN = SONAR_AUTH_TOKEN;
                            sh "/var/jenkins_home/tools/hudson.plugins.sonar.SonarRunnerInstallation/mysonar/bin/sonar-scanner \
                                -Dsonar.projectKey='test-job' \
                                -Dsonar.sources=./src ";
                           } // SonarQube taskId is automatically attached to the pipeline context
                    }
                }
                stage('ZIPEO-NEXUS'){
                    agent any
                    steps{
                                   sh 'mvn -s $MVN_SET clean install'
                                   sh 'mvn -s $MVN_SET deploy -DskipTests -Dmaven.install.skip=true'

                    }
                }
                    
                }

            }
        }



    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure{
            echo 'fallo'
         
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
        }
//       cleanup{
//                 deleteDir()
//         }
    }
}


