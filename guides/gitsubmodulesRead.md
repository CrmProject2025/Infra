Как создать проект с Git Submodules и подключить к нему репозитории
Git submodules позволяют добавлять в репозиторий другие репозитории как вложенные зависимости. Это удобно, если у тебя есть основной проект и несколько связанных модулей.

🔹 1. Создание главного репозитория
Допустим, у нас есть основной проект (например, main-project).
Создай новый репозиторий:

mkdir main-project && cd main-project
git init
git remote add origin https://github.com/your-user/main-project.git


Создай .gitignore и добавь туда стандартные файлы для исключения:

echo "node_modules/" >> .gitignore
echo "target/" >> .gitignore  # для Java/Spring Boot
echo ".idea/" >> .gitignore   # если используешь IntelliJ IDEA


🔹 2. Подключение подмодулей
Допустим, у тебя есть два подмодуля:
backend-repo 👉 бэкенд
frontend-repo 👉 фронтенд
Чтобы добавить их в main-project, выполняем:

git submodule add https://github.com/your-user/backend-repo.git backend
git submodule add https://github.com/your-user/frontend-repo.git frontend

После этого структура будет такой:

main-project/
│── backend/  (содержит backend-repo)
│── frontend/ (содержит frontend-repo)


🔹 3. Коммиты и отправка в удаленный репозиторий
Зафиксируем изменения:

git add .
git commit -m "Добавлены submodules: backend и frontend"
git push origin main


Git создаст файл .gitmodules, где будут храниться ссылки на подмодули.

[submodule "backend"]
    path = backend
    url = https://github.com/your-user/backend-repo.git
[submodule "frontend"]
    path = frontend
    url = https://github.com/your-user/frontend-repo.git



🔹 4. Клонирование проекта с подмодулями
При обычном клонировании git clone подмодули не подтягиваются автоматически.
Чтобы скачать репозиторий вместе с подмодулями, используй:

git clone --recurse-submodules https://github.com/your-user/main-project.git

Или если уже клонировал без подмодулей, подтяни их вручную:

git submodule update --init --recursive


🔹 5. Работа с подмодулями
✅ Обновление всех подмодулей
Если в подмодуле появились новые коммиты, подтянуть их можно командой:

git submodule update --remote --merge

✅ Удаление подмодуля
Если нужно удалить подмодуль (например, frontend), делаем:

git submodule deinit -f frontend
rm -rf .git/modules/frontend
git rm -f frontend
git commit -m "Удален submodule frontend"

