int j=3  //number of iteration

for (int i=0; i < j; i++) {
pipeline {
    agent {
		label 'master'
	}
        parameters {
            string(name: 'buildName', 
               defaultValue: 'Nightly Build',
               description: 'A customized name of your build')
            booleanParam(name: 'DB_RESTORE',
               defaultValue: true,
               description: 'Deselect this button if you DO NOT need database restore for your test')
            booleanParam(name: 'Warmup_and_Formal_Test',
               defaultValue: true,
               description: 'Deselect this button if you DO NOT need to run any test')
            booleanParam(name: 'Server_Reboot',
               defaultValue: true,
               description: 'Deselect this button if you DO NOT need to reboot APP servers')
        }
        stages {
            stage('reboot APP servers') {
                when {
                    expression {
                        return params.Server_Reboot
                    }
                }
                steps {
                    sh 'echo " reboot APP servers for Build Number $BUILD_NUMBER "'
                }
            }
            stage('stop tomcat servers') {
                steps {
                    sh 'echo " stop tomcat servers for Build Number $BUILD_NUMBER "'
                    script {
                        currentBuild.displayName = "#${BUILD_NUMBER}-${params.buildName}"
                    }
                }
            }
            stage('cleanup app server logs') {
                steps {
                    sh 'echo " cleanup app server logs for Build Number $BUILD_NUMBER "'
                }
            }
            stage('restart SQL server') {
                steps {
                    sh 'echo " restart SQL server for Build Number $BUILD_NUMBER "'
                }
            }
            stage('restore databases') {
                when {
                    expression {
                        return params.DB_RESTORE
                    }
                }
                steps {
                    sh 'echo " rrestore databases for Build Number $BUILD_NUMBER "'
                }
                post {
                    success {
                        echo "Succeeded."
                        
                    }
                    failure {
                        echo "A failure happened."
                       
                    }
                }
            }
            stage('start Tomcat servers') {
                steps {
                    sh 'echo " start Tomcat servers for Build Number $BUILD_NUMBER "'
                }
                post {
                    failure {
                        echo "At least one of the Tomcat server(s) was not started properly. Please rerun the test."
                    }
                }
            }
            stage('run warmup and formal test') {
                when {
                    expression {
                        return params.Warmup_and_Formal_Test
                    }
                }
                steps {
                    build job: 'demo1-1'
                    sh 'echo " run warmup and formal test Build Number $BUILD_NUMBER "'
                }
                post {
                    success {
                        echo "Succeeded."
                    }
                    failure {
                        echo "A failure happened."
                    }
                }
            }
        }

    }
}
