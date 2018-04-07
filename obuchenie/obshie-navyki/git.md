# Git

## Обучающие курсы

1. Для новичков рекомендуется пройти небольшой обучающийся курс [Git How To](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/githowto.com/ru/README.md).
2. Разбираемся в том, как устроена работа в команде - [Github Flow](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/guides.github.com/introduction/flow/README.md)

## Полезные ресурсы

### Разбираемся с Git

* [Git How To](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/githowto.com/ru/README.md) — это интерактивный тур, который познакомит вас с основами Git. Тур создан с пониманием того, что лучшим способом научиться чему-нибудь — сделать это своими руками.
* [Команды git](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/git-scm.com/book/commands/README.md) - полный список команд на официальном сайте
* [git - the simple guide](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/rogerdudler.github.io/git-guide/README.md)
* [Ежедневная работа с Git](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/habrahabr.ru/post/174467/README.md) - статья на Хабре
* [Что нам стоит Git настроить!](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/habrahabr.ru/post/164297/README.md) - статья на Хабре

### Видео-уроки

* [Git & GitHub Tutorials](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/www.youtube.com/playlist?list=PLEACDDE80A79CE8E7/README.md)

### Книги

* [Pro Git](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/git-scm.com/book/ru/v2/README.md) - официальная книга Git
* [Магия Git](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/dl.dropboxusercontent.com/u/281916/delete/book.pdf)

### Шпаргалки

* [GitHub Cheatsheet](https://github.com/goldencode/developer-handbook/tree/1c894cbe8266a31e1c1456a54e018f217baa6508/raw.githubusercontent.com/github/training-kit/master/downloads/github-git-cheat-sheet.pdf)

## Работаем с Git

### Стартуем проект с нуля:

* `git init` - инициализируем Git.
* `git add -A` - индексируем все файлы.
* `git commit -m 'chore(project): init project'` - коммитим и комментируем в соответствии с соглашением по коммитоименованию.
* `git remote add origin <url>` - добавляем удалённый репозиторий, где `<url>` - ссылка на git-репозиторий.
* `git push origin master` - заливаем проект в удалённый репозиторий в ветку `master`.
* Если залить не удалось, проект уже не пустой и может содержать `readme.md`, поэтому нужно сделать `rebase`:

  ```text
    git pull --rebase origin master
  ```

* Если есть конфликты, то см. [разрешаем конфликты](git.md#conflicts).
* Заливаем снова.

### Разворачиваем уже имеющийся проект

* `git clone <url>` - клонируем проект.

### Повседневная работа с проектом

* `git pull --rebase origin master` - обновляем ветку `master` с ключём `--rebase`, чтобы избежать промежуточных коммитов.
* Если конфликтов не произошло, то пропускаем этот пункт. Если есть, то [правим руками](git.md#conflicts).
* Делаем изменения в коде, например, добавляем главную страницу.
* `git add -A` - индексируем изменения.
* `git commit -m 'feat(main): add main page'` - закоммитили в соответствии с соглашением по коммитоименованию.
* `git push origin master` - заливаем изменения в удалённый репозиторийй в ветку `master`.

### Разрешаем конфликты {#conflicts}

* Правим руками файлы, содержащие конфликты \(посмотреть их можно через `git status`\):

```text
<<<<<<< HEAD
// 1 секция: Код текущей ветки
=======
// 2 секция: Наш изменения в коде
>>>>>>> master
```

Чтобы разрешить конфликт, нужно _внимательно_ сравнить изменения в 1 секции и добавить недостающий \(новый\) код в свою \(2 секцию\). После этого удаляем всё, кроме 2 секции.

```text
// 2 секция: Наш изменения в коде
```

* После этого индексируем изменения.

  ```text
  git add -A
  ```

* Далее нужно продолжить `rebase` с помощью команды:

  ```text
  git rebase --continue
  ```

* Если конфликтов больше нет и `reabase` завершился, то на это всё, если конфликты есть, то повторить итерацию.

### Работаем в отдельной ветке

#### Создаём ветку

* `git pull --rebase origin master` - обновляем ветку `master` с ключём `--rebase`, чтобы избежать промежуточных коммитов.
* `git checkout -b <name>` - создаём ветку, где `<name>` - название фичи.
* Делаем изменения в коде, например, добавляем главную страницу.
* `git add -A` - индексируем изменения.
* `git commit -m 'feat(main): add main page'` - закоммитили в соответствии с соглашением по коммитоименованию.
* `git push origin <name>` - заливаем нашу ветку в удалённый репозиторий.

#### Сливаем изменения в `master`.

* Переходим в ветку `master` и сливаем в неё фичу:

  ```text
    git merge <name>
  ```

* Разрешаем конфликты, если они возникли
* `git push origin master` - заливаем итоговые изменения в удалённый репозиторий.

### Полезные команды на всякий случай

* `git commit --amend` - позволяет изменить название коммита. Если нужно включить ещё и изменённые файлы, то перед этим проиндексировать файлы.
* `git reset --hard HEAD^` - полное удаление последнего коммита.

