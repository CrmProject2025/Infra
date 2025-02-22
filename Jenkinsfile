pipeline {
    agent any

    environment {

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
        stage('Check Branch') {
            steps {
                script {
                    // Проверяем, что это ветка develop или PR в develop
                    if (env.BRANCH_NAME != 'develop' && env.CHANGE_TARGET != 'develop') {
                        echo "Pipeline запущен для ветки ${env.BRANCH_NAME}. Пропускаем."
                        currentBuild.result = 'SUCCESS' // Пропускаем сборку
                        return
                    }
                }
            }
        }

        stage('Fetch Branches') {
            steps {
                bat "git fetch origin" // Обновляем информацию о ветках
            }
        }

        // Этап 2: Мердж ветки feature в develop
        stage('Merge with Develop') {
            steps {
                script {
                    // Переключаемся на ветку develop
                    // sh "git checkout ${TARGET_BRANCH}"
                    // Обновляем локальную ветку develop
                    bat "git pull origin ${TARGET_BRANCH}"
                    // Мерджим feature в develop
                    bat "git merge origin/${env.CHANGE_BRANCH}"
                }
            }
        }

        // Этап 3: Сборка Docker-образов
        stage('Build Docker Images') {
            steps {
                bat "docker-compose up -d --build"
            }
        }

        // Этап 4: Запуск тестов
        // stage('Run Tests') {
        //     steps {
        //         sh "${DOCKER_COMPOSE} up -d"
        //         // Пример: запуск тестов для сервиса "user-service"
        //         sh "${DOCKER_COMPOSE} exec -T user-service ./mvnw test"
        //     }s
        // }
    }

       post {
       


    // Постобработка

        always {
            // Остановка и удаление контейнеров
            bat "docker-compose down"
        }
        cleanup {
            // Очистка workspace
            cleanWs()
        }
    


}
}
