# Работа с репозиторием 

### Описание веток репозитория
Основная ветка - master . master - ветка с текущим состоянием проект. Работы в этой ветке не ведутся никогда вообще.

Остальные ветки - личные ветки разработчиков


### Регламент работы над тасками

Работа над тасками производится по следующему алгоритму:

0. Перед началом работы необходимо выполнить git config --global core.filemode false (достаточно поменять конфиг один раз)
1. Переходим в master. Не забываем подтянуть из origin свежую версию
```
git checkout master
git fetch origin
git pull origin master (если возник мердж коммит - git reset --hard origin/master)
```
2. Создаём новую локальную ветку для работы над таском. Имя ветки должно содержать номер таска и краткую ремарку о самом таске. Например 7-storage_web_api_first_step соответственно номеру таска в trello
```
git checkout -b 7-storage_web_api_first_step
```

3. Разработка таска ведётся в этой новой ветке.
4. Необходимо не забывать регулярно коммитить существенные изменения. Комментарий должен содержать осмысленное описание того, какие изменения были сделаны в данном коммите. Комментарии необходимо оставлять строго на английском (как и весь свой код, собственно)
```
git commit -am '<message>'
```

5. Необходимо не забывать регулярно подтягивать изменения из master. По крайней мере один раз в день перед началом работы в ветке
```
# обновляем сведения об удалённом репозитории origin:
git fetch origin

# перебазируем коммиты на свежую версию ветки master, подробнее о происходящем можно прочитать тут: https://git-scm.com/book/ru/v1/%D0%92%D0%B5%D1%82%D0%B2%D0%BB%D0%B5%D0%BD%D0%B8%D0%B5-%D0%B2-Git-%D0%9F%D0%B5%D1%80%D0%B5%D0%BC%D0%B5%D1%89%D0%B5%D0%BD%D0%B8%D0%B5
git rebase origin/master
```

6. Когда вся разработка в ветке завершена, пушим таску в origin
```
git push origin 7-storage_web_api_first_step
```

7. отправляем таску на ревью в гитлабе (как мерж-реквест)
8. После ревью исправляем все указанные недочёты и отправляем на повторное ревью, если правок было много
9. После того, как таск готов и прошел ревью, необходимо объединить все коммиты в один и переименовать этот коммит в номер таска, типа "7-storage_web_api_first_step"

### Про объединение коммитов

1. Посмотрим разницу в коммитах между своей веткой 420-some_text и master (находять в своей ветке 420-some_text)
```
# сами коммиты
git log master..420-some_text --pretty=oneline
# их количество
git log master..420-some_text --pretty=oneline | wc -l
```
2. Объединим все коммиты в один (пусть требуется объединить 3 коммита)
```
git rebase -i HEAD~3
```
**коммиты должны быть последовательными, иначе их нельзя объединять**

**Пример, когда нельзя объединять коммиты:**
```
хеш-код : коммит для объединения 1
хеш-код : левый коммит 1
хеш-код : левый коммит 2
хеш-код : коммит для объединения 2
хеш-код : коммит для объединения 3
```

3. Открылся вим, здесь можно увидеть все 3 коммита в порядке их добавления (в логе у них обратный порядок)

```
pick 62b1bc8 Touch (myself) some file
pick 31ae721 Add some code
pick 14233b1 Do some more stupid things

# Rebase 000f46c..14233b1 onto 000f46c (3 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

4. Необходимо поменять у всех коммитов (кроме первого) тег pick на squash, squash означает, что коммит будет слит в предыдущий, таким образом мы и получим один коммит, как слияние всех трех.

```
pick 62b1bc8 Touch (myself) some file
s 31ae721 Add some code
s 14233b1 Do some more stupid things
```

5. Сохраняем и закрываем (esc :wq), автоматически запустится окно редактирования коммит сообщения:
```
# This is a combination of 3 commits.
# This is the 1st commit message:

Touch (myself) some file

# This is the commit message #2:

Add some code

# This is the commit message #3:

Do some more stupid things

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Sat Aug 18 23:34:49 2018 +0300
#
# interactive rebase in progress; onto 000f46c
# Last commands done (3 commands done):
#    s 31ae721 Add some code
#    s 14233b1 Do some more stupid things
# No commands remaining.
# You are currently rebasing branch '420-some_text' on '000f46c'.
#
# Changes to be committed:
#       new file:   asd
```

6. В открывшемся окне вима закоментить все названия коммитов, добавить строку с новым комментарием для коммита, комментарий должен содержать номер таска и его сабж 420-some_text, затем опять выходим и сохраняем esc :wq, выход без сохранения :q!
```
# This is a combination of 3 commits.
# This is the 1st commit message:

# Touch (myself) some file

# This is the commit message #2:

# Add some code

# This is the commit message #3:

# Do some more stupid things

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# Date:      Sat Aug 18 23:34:49 2018 +0300
#
# interactive rebase in progress; onto 000f46c
# Last commands done (3 commands done):
#    s 31ae721 Add some code
#    s 14233b1 Do some more stupid things
# No commands remaining.
# You are currently rebasing branch '420-some_text' on '000f46c'.
#
# Changes to be committed:
#       new file:   asd
420-some_text
```

### Пересоздание ветки и перенос коммита
1. Объединим в старой ветке все коммиты в один, как описано выше
2. Посмотрим git log. Вверху должен быть наш объединенный коммит, нам нужен его хеш
3. Сохраним его хеш (он выглядит примерно так f77ec403c0816a908305f3f7e7ea499f2ea0926e)
4. Переходим в master и создаем новую ветку от мастера
5. Подтягиваем в новую ветку наш коммит  git cherry-pick f77ec403c0816a908305f3f7e7ea499f2ea0926e
6. Теперь новая ветка содержит все коммиты мастера и коммит с тестами из старой ветки

Еще более подробное описание на гите - [перезапись истории](https://git-scm.com/book/ru/v1/%D0%98%D0%BD%D1%81%D1%82%D1%80%D1%83%D0%BC%D0%B5%D0%BD%D1%82%D1%8B-Git-%D0%9F%D0%B5%D1%80%D0%B5%D0%B7%D0%B0%D0%BF%D0%B8%D1%81%D1%8C-%D0%B8%D1%81%D1%82%D0%BE%D1%80%D0%B8%D0%B8)

