pipeline{
    environment {
             MVN_SET = credentials('settings-xml')
        }
    agent any

    stages{
        
        stage('PASO A NEXUS'){
            parallel{
                stage('SONAR'){
                  agent any
                  script {
                    // requires SonarQube Scanner 2.8+
                    def scannerHome = tool 'mysonar';

                    withSonarQubeEnv('mysonar') {

                        env.SQ_HOSTNAME = "localhost:9000";
                        env.SQ_PROJECT_KEY = "test-job";

                        sh "${scannerHome}/bin/sonar-scanner \
                                -Dsonar.projectKey=${SQ_PROJECT_KEY} \
                                -Dsonar.sources=src ;
                    }
                    }
                }
                stage('ZIPEO-NEXUS'){
                    agent any
                    steps{
                        checkout([$class: 'GitSCM', branches: [[name: 'master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'jacky', url: 'https://github.com/jacky9595/jenkins-nexus.git']]])
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


