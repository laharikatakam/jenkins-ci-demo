pipeline {
  agent any
  options { timestamps() }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/laharikatakam/jenkins-ci-demo.git'
      }
    }

    stage('Build') {
      steps {
        bat '''
          python -m venv .venv
          call .venv\\Scripts\\activate
          pip install --upgrade pip
          if exist requirements.txt pip install -r requirements.txt
        '''
      }
    }

    stage('Test') {
      steps {
        bat '''
          call .venv\\Scripts\\activate
          pytest -q
        '''
      }
    }

    stage('Deploy') {
      steps {
        bat '''
          if not exist deploy mkdir deploy
          powershell -NoProfile -Command ^
            "Copy-Item -Path * -Destination deploy -Recurse -Force -Exclude @('.venv','.git','deploy','__pycache__')"
        '''
        archiveArtifacts artifacts: 'deploy/**', onlyIfSuccessful: true
      }
    }
  }
}
