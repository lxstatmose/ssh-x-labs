# Дальнейшие действия с Git

Учебный репозиторий: https://github.com/lxstatmose/ssh-x-labs. Папка с перенесёнными файлами: /Users/atmose/Downloads/ssh-x-labs. На момент переноса в папке отсутствовал .git: это ещё не локальная Git-копия. Автор комплекта никаких Git-команд не выполнял.

Публичный GitHub API вернул 404; это не доказывает отсутствие репозитория: он может быть приватным. Откройте его в своём аккаунте и посмотрите, есть ли уже main и исходные файлы.

## Если в новой репе уже есть main с исходниками

Выполните самостоятельно:

```bash
cd /Users/atmose/Downloads/ssh-x-labs
git init -b lab-1
git remote add origin https://github.com/lxstatmose/ssh-x-labs.git
git fetch origin
git branch -r
```

Убедитесь, что список содержит origin/main. Если основной бранч называется иначе, ниже используйте его имя. При ошибке авторизации сначала настройте доступ к своему GitHub.

Далее привяжите текущую ветку lab-1 к исходному main, сохранив перенесённые файлы:

```bash
git reset --mixed origin/main
git status --short
git diff --stat
```

В данном случае reset --mixed нужен для подключения папки без .git к истории удалённого репозитория. Он сохраняет содержимое рабочих файлов и сбрасывает только индекс/указатель ветки. Не заменяйте его на --hard. Если в папке уже появился .git или вы начали собственные коммиты, остановитесь и сначала проверьте состояние: эта последовательность рассчитана на исходную папку без .git.

В docs/00-project.md указаны Любченко Никита Тариэлович, группа БПИ2303. После этого сохраните подготовленную лабораторную:

```bash
git diff --check
git add README.md README.ru.md README-LAB-1.md CHANGELOG.md package-lock.json .nvmrc docs tests handin evidence
git diff --cached --stat
git commit -m "docs(lab-1): prepare SSH-X project passport and semester plan"
git push -u origin lab-1
```

Посмотрите на результат git status и diff перед коммитом. Убедитесь, что не включены пароли, приватные ключи, userData и сборочные артефакты. Существующие LICENSE, .gitignore, исходный код, package.json и release workflow сохранены; README содержит учебный блок, .nvmrc уточнён, lock-файл изменил только версию корневого пакета 1.0.0 → 1.0.1.

## Если новая репа пустая

Для PR нужна базовая main с исходным проектом. Сначала распакуйте предоставленный ssh-x-labs-baseline.zip в отдельный каталог. Архив содержит исходное состояние вашей папки до переноса лабораторных файлов.

Откройте терминал в распакованном ssh-x-labs-baseline и выполните:

```bash
git init -b main
git remote add origin https://github.com/lxstatmose/ssh-x-labs.git
git add .
git commit -m "chore: import original SSH-X as course baseline"
git push -u origin main
```

Затем выполните предыдущий раздел в /Users/atmose/Downloads/ssh-x-labs. Лабораторные файлы попадут в lab-1, а исходный проект будет базой main. Если main уже существовал, этот раздел не нужен. При отказе push не используйте force: проверьте исходное состояние удалённой репы.

## Далее в GitHub

1. Создайте метки review, bug, change-request, question.
2. Создайте product и минимум три issues по handin/ISSUES.md. Product должен оставаться открытым до согласования проекта и плана преподавателем.
3. Откройте pull request lab-1 → main. Текст — handin/PULL-REQUEST.md. Здесь main означает main учебной репы, не основного ssh-x. Если GitHub предложит исходный проект как base, явно выберите lxstatmose/ssh-x-labs.
4. Проверьте CI. Исходная матрица использует Node.js 20/22, локальная проверка выполнена на 26.12.0; фактический результат нужно подтвердить. Исходный CI в рамках переноса не менялся.
5. Защитите main от прямых push, разрешив изменения через PR; предоставьте преподавателю доступ к коду, issues и PR. В индивидуальной работе настройте обязательное одобрение с учётом доступного рецензента.
6. Проверка на другом Mac M3 от 17.09.2026 уже описана в handin/ACCEPTANCE.md со слов пользователя. Для текущей учебной версии повторите инструкцию с фиксацией точной среды. SIGABRT относится к отдельному прогону Codex и сохраняется в журнале.
7. Получите одобрение преподавателя, закройте product, слейте PR в main учебного репозитория.
8. Поставьте тег на итоговый main после merge:

```bash
git fetch origin
git switch main
```

Если локальной main ещё нет, вместо git switch main используйте git switch -c main --track origin/main. Затем:

```bash
git pull --ff-only
git tag lab-1
git push origin refs/tags/lab-1
```

Если тег уже существует, не перезаписывайте его. npm version не нужен: он создаёт другие коммиты/теги и может запустить релиз.
