pipeline {
    agent any
    stages {
        stage('Install Dependencies') {
            steps {
                bat '''
                    python -m venv .venv
                    .venv\\Scripts\\python.exe -m pip install -r requirements.txt
                '''
            }
        }
        stage('Test') {
            steps {
                bat '''
                    .venv\\Scripts\\python.exe -m pytest -v
                '''
            }
        }
        stage('Run Application') {
            steps {
                bat '''
                    .venv\\Scripts\\python.exe app.py
                '''
            }
        }
        stage('Package Release') {
            steps {
                bat '''
                    if exist release rmdir /s /q release
                    mkdir release
                    xcopy /E /I /Y . release\\app /EXCLUDE:.gitignore
                    powershell Compress-Archive -Path release\\app\\* -DestinationPath release\\student-management.zip -Force
                '''
                archiveArtifacts artifacts: 'release/*.zip', fingerprint: true
            }
        }
    }
}
