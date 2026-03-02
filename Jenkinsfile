pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh '''
                set -e
                echo "Building Java project..."
                echo "Listing workspace contents:"
                ls

                cd "Password Protection"

                mkdir -p build

                # Compile source files
                javac -d build src/*.java

                echo "Build successful"
                '''
            }
        }
stage('Test') {
    steps {
        sh '''
        set -e
        echo "Running JUnit tests for File-Encrypter..."

        cd "Password Protection"

        # Always remove old corrupted jar
        rm -f junit-platform-console-standalone.jar

        echo "Downloading JUnit..."
        curl -L -o junit-platform-console-standalone.jar \
        https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.10.0/junit-platform-console-standalone-1.10.0.jar

        mkdir -p test-build

        javac -cp junit-platform-console-standalone.jar:build -d test-build test/*.java

        java -jar junit-platform-console-standalone.jar \
        --class-path build:test-build \
        --scan-class-path

        echo "JUnit tests executed successfully"
        '''
    }
}

        stage('Deploy') {
            steps {
                sh '''
                set -e
                echo "Deploying (Packaging) File-Encrypter Application..."

                cd "Password Protection"

                # Create executable artifact (JAR)
                jar cf FileEncrypter.jar -C build .

                echo "Deployment successful - Artifact ready"
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
