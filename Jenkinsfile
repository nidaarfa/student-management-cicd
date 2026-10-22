pipeline {
    agent any
    stages {
        stage('Install Dependencies') {
            steps {
                bat '''
                    if exist release rmdir /s /q release
                    if exist .pytest_cache rmdir /s /q .pytest_cache
                    for /d /r %%d in (__pycache__) do @if exist "%%d" rmdir /s /q "%%d"
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
                    mkdir release\\app
                    copy app.py release\\app\\
                    copy test_app.py release\\app\\
                    copy requirements.txt release\\app\\
                    powershell Compress-Archive -Path release\\app\\* -DestinationPath release\\student-management.zip -Force
                '''
                archiveArtifacts artifacts: 'release/*.zip', fingerprint: true
            }
        }
    }
}
