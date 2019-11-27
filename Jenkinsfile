pipeline{
    environment {
             MVN_SET = credentials('settings-xml')
        }
    agent any

    stages{
        
        stage('PASO A NEXUS'){
            parallel{
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
