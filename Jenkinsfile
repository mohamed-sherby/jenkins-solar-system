pipeline {
    agent any

    tools {
        nodejs 'node-20-16-0'
    }

    // environment {
    //     // MONGO_URI = "mongodb+srv://supercluster.d83jj.mongodb.net/superData"
    //     // MONGO_DB_CREDS = credentials('mongo-db-credentials')
    //     // MONGO_USERNAME = credentials('mongo-db-username')
    //     // MONGO_PASSWORD = credentials('mongo-db-password')

    // }

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
                stage('NPM Dependency Audit') {
                    steps {
                        sh '''
                            npm audit --audit-level=critical
                            echo $?
                        '''
                    }
                }

                stage('OWASP Dependency Check') {
                    steps {
                        dependencyCheck additionalArguments: '''
                            --scan \'./\' 
                            --out \'./\'  
                            --format \'ALL\' 
                            --disableYarnAudit \
                            --prettyPrint''', odcInstallation: 'OWASP-DepCheck-10'

                        dependencyCheckPublisher failedTotalCritical: 1, pattern: 'dependency-check-report.xml', stopBuild: false
                    }
                }
            }
        }
    }
}