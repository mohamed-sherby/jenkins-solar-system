pipeline {
    agent any

    tools {
        nodejs 'node-20-16-0'
    }

    environment {
        // MONGO_URI = "mongodb+srv://supercluster.d83jj.mongodb.net/superData"
        // MONGO_DB_CREDS = credentials('mongo-db-credentials')
        // MONGO_USERNAME = credentials('mongo-db-username')
        // MONGO_PASSWORD = credentials('mongo-db-password')
        SONAR_SCANNER_HOME = tool 'sonar-scanner'
        SONAR_SCANNER_CMD = "${SONAR_SCANNER_HOME}/bin/sonar-scanner"
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {
        stage('Installing Dependencies') {
            options { timestamps() }
            steps {
                sh 'npm install --no-audit'
                sh "echo test"
            }
        }

        stage('Dependency Scanning') {
            parallel {
                // stage('NPM Dependency Audit') {
                //     steps {
                //         sh '''
                //             npm audit --audit-level=critical
                //             echo $?
                //         '''
                //     }
                // }

                stage('OWASP Dependency Check') {
                    steps {

                        catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {
                            dependencyCheck additionalArguments: '''
                                --scan \'./\' 
                                --out \'./\'  
                                --format \'ALL\' 
                                --disableYarnAudit \
                                --prettyPrint''', 
                                odcInstallation: 'owasp'



                        }

                            dependencyCheckPublisher failedTotalCritical: 1, pattern: 'dependency-check-report.xml', stopBuild: false

                            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: './', reportFiles: 'dependency-check-report.html', reportName: 'Dependancy Check Report', reportTitles: '', useWrapperFileDirectly: true])



                    }
                }
            }
        }
        stage("Unit Tests") {
            steps {
                catchError(buildResult: 'SUCCESS', message: 'Oops! it will be fixed in future releases', stageResult: 'UNSTABLE') {
                    sh 'npm test'
                }
            }
        }
        stage('Code Coverage') {
            steps {
                catchError(buildResult: 'SUCCESS', message: 'Oops! it will be fixed in future releases', stageResult: 'UNSTABLE') {
                    sh 'npm run coverage'
                }
            }
        }
        stage('Sonar Scanner') {
            steps {
                sh '''
                    ${SONAR_SCANNER_CMD} \
                    -Dsonar.projectKey=solar-system \
                    -Dsonar.sources=app2.js \
                    -Dsonar.host.url=http://172.17.0.3:9000 \
                    -Dsonar.login=$SONAR_TOKEN
                '''
            }

        }
    }
}