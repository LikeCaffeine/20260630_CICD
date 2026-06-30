pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // 깃허브에서 test.jmx 소스 가져오기
                git url: 'https://github.com/LikeCaffeine/20260630_CICD', branch: 'main'
            }
        }
        
        stage('Run Performance Test') {
            steps {
                // 기존 결과 파일 및 폴더 제거
                bat 'if exist results.jtl del /q results.jtl'
                bat 'if exist reports rmdir /s /q reports'
                
                // JMeter Non-GUI 실행 (HTML 리포트 생성 포함)
                bat 'jmeter -n -t test.jmx -l results.jtl -e -o reports'
            }
        }
        
        stage('Publish Performance Report') {
            steps {
                // 퍼포먼스 플러그인을 호출하여 JTL 파일 파싱
                perfReport sourceDataFiles: '**/results.jtl'
            }
        }
    }
    
    post {
        always {
            echo 'Archiving HTML report...'
            // 생성된 HTML 리포트 폴더 전체를 젠킨스 빌드 아티팩트에 보관
            archiveArtifacts artifacts: 'reports/**', allowEmptyArchive: true
        }
    }
}