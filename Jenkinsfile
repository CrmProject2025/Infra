pipeline {
    agent any

    environment {
        // Путь к Docker Compose
        DOCKER_COMPOSE = 'docker-compose'

        // Имя целевой ветки
        TARGET_BRANCH = 'develop'

        // Переменные для GitHub API
        GITHUB_REPO = 'CrmProject2025/Infra'
        GITHUB_TOKEN = credentials('github-token') // Токен GitHub (добавьте в Jenkins Credentials)
    }

    stages {
        // Этап 1: Получение кода из репозитория
        // stage('merge') {
        //     steps {
        //       sh '''
        //             git clone https://github.com/CrmProject2025/Infra.git
        //             cd Infra
        //             git checkout develop
        //         '''

        //     }
        // }

        // Этап 2: Мердж ветки feature в develop
        stage('Merge with Develop') {
            steps {
                script {
                    // Переключаемся на ветку develop
                    // sh "git checkout ${TARGET_BRANCH}"
                    // Обновляем локальную ветку develop
                    sh "git pull origin ${TARGET_BRANCH}"
                    // Мерджим feature в develop
                    sh "git merge origin/${env.CHANGE_BRANCH}"
                }
            }
        }

        // Этап 3: Сборка Docker-образов
        stage('Build Docker Images') {
            steps {
                sh "${DOCKER_COMPOSE} build"
            }
        }

        // Этап 4: Запуск тестов
        // stage('Run Tests') {
        //     steps {
        //         sh "${DOCKER_COMPOSE} up -d"
        //         // Пример: запуск тестов для сервиса "user-service"
        //         sh "${DOCKER_COMPOSE} exec -T user-service ./mvnw test"
        //     }
        // }
    }

       post {
        success {
            script {
                // Отправляем комментарий в PR
                sh """
                    curl -X POST \
                    -H "Authorization: token ${GITHUB_TOKEN}" \
                    -H "Accept: application/vnd.github.v3+json" \
                    -d '{"body": "🎉 Все тесты прошли успешно! PR готов к ручному слиянию."}' \
                    "https://api.github.com/repos/${GITHUB_REPO}/issues/${env.CHANGE_ID}/comments"
                """
            }
        }
        failure {
            script {
                // Отправляем комментарий в PR, если тесты не прошли
                sh """
                    curl -X POST \
                    -H "Authorization: token ${GITHUB_TOKEN}" \
                    -H "Accept: application/vnd.github.v3+json" \
                    -d '{"body": "❌ Тесты не прошли. Пожалуйста, проверьте ошибки."}' \
                    "https://api.github.com/repos/${GITHUB_REPO}/issues/${env.CHANGE_ID}/comments"
                """
            }
        }


    // Постобработка

        always {
            // Остановка и удаление контейнеров
            sh "${DOCKER_COMPOSE} down"
        }
        cleanup {
            // Очистка workspace
            cleanWs()
        }
    


}
}
