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

    }

    stages {
        stage('Installing Dependencies') {
            options { timestamps() }
            steps {
                sh 'npm install --no-audit'
                sh "echo test"
            }
        }
    }
}