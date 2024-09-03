# Домашнее задание к занятию "`Ветвления в Git`" - `Тен Денис`


### Цель задания

В процессе работы над заданием вы потренеруетесь делать merge и rebase. В результате вы поймете разницу между ними и научитесь решать конфликты.   

Обычно при нормальном ходе разработки выполнять `rebase` достаточно просто. 
Это позволяет объединить множество промежуточных коммитов при решении задачи, чтобы не засорять историю. Поэтому многие команды и разработчики предпочитают такой способ.   


### Инструкция к заданию

1. В личном кабинете отправьте на проверку ссылку на network графика вашего репозитория.
2. Любые вопросы по решению задач задавайте в чате учебной группы.


### Дополнительные материалы для выполнения задания

1. Тренажёр [LearnGitBranching](https://learngitbranching.js.org/), где можно потренироваться в работе с деревом коммитов и ветвлений. 

------

## Задание «Ветвление, merge и rebase»  

**Шаг 1.** Предположим, что есть задача — написать скрипт, выводящий на экран параметры его запуска. 
Давайте посмотрим, как будет отличаться работа над этим скриптом с использованием ветвления, merge и rebase. 

Создайте в своём репозитории каталог `branching` и в нём два файла — `merge.sh` и `rebase.sh` — с 
содержимым:

```bash
#!/bin/bash
# display command line options

count=1
for param in "$*"; do
    echo "\$* Parameter #$count = $param"
    count=$(( $count + 1 ))
done
```

Этот скрипт отображает на экране все параметры одной строкой, а не разделяет их.

**Шаг 2.** Создадим коммит с описанием `prepare for merge and rebase` и отправим его в ветку main. 

```bash
git add branching/
git commit -m "prepare for merge and rebase"
git push
```
![image](https://github.com/user-attachments/assets/592b3b5a-0e80-4a31-af48-9e1bc4eaaa20)


#### Подготовка файла merge.sh 
 
**Шаг 1.** Создайте ветку `git-merge`. 

```bash
git switch -c git-merge
```

**Шаг 2**. Замените в ней содержимое файла `merge.sh` на:

```bash
#!/bin/bash
# display command line options

count=1
for param in "$@"; do
    echo "\$@ Parameter #$count = $param"
    count=$(( $count + 1 ))
done
```

**Шаг 3.** Создайте коммит `merge: @ instead *`, отправьте изменения в репозиторий.  

```bash
git add branching/
git commit -m "merge: @ instead *"
git push origin git-merge
```

![image](https://github.com/user-attachments/assets/3eaeb7a2-a689-46b6-8df5-49662d1c0bb5)


**Шаг 4.** Разработчик подумал и решил внести ещё одно изменение в `merge.sh`:
 
```bash
#!/bin/bash
# display command line options

count=1
while [[ -n "$1" ]]; do
    echo "Parameter #$count = $1"
    count=$(( $count + 1 ))
    shift
done
```

Теперь скрипт будет отображать каждый переданный ему параметр отдельно. 

**Шаг 5.** Создайте коммит `merge: use shift` и отправьте изменения в репозиторий. 

```bash
git add branching/
git commit -m "merge: use shift"
git push
```
![image](https://github.com/user-attachments/assets/d15206de-29bd-4b7b-bb17-be551739218e)

![image](https://github.com/user-attachments/assets/a60bad53-33fe-4a66-866c-c58cd0a56fab)



#### Изменим main  

**Шаг 1.** Вернитесь в ветку `main`. 

```bash
git checkout main
```
![image](https://github.com/user-attachments/assets/df9faf83-cb75-487c-a34e-8f466db61fd0)

**Шаг 2.** Предположим, что пока мы работали над веткой `git-merge`, кто-то изменил `main`. Для этого
изменим содержимое файла `rebase.sh` на:

```bash
#!/bin/bash
# display command line options

count=1
for param in "$@"; do
    echo "\$@ Parameter #$count = $param"
    count=$(( $count + 1 ))
done

echo "====="
```

В этом случае скрипт тоже будет отображать каждый параметр в новой строке. 

**Шаг 3.** Отправляем изменённую ветку `main` в репозиторий.

```bash
git add branching/
git commit -m "update rebase.sh"
git push origin main
```
![image](https://github.com/user-attachments/assets/9d8c6507-4812-4abf-806a-29dd1b679e56)


#### Подготовка файла rebase.sh  

**Шаг 1.** Предположим, что теперь другой участник нашей команды не сделал `git pull` либо просто хотел ответвиться не от последнего коммита в `main`, а от коммита, когда мы только создали два файла
`merge.sh` и `rebase.sh` на первом шаге.  
Для этого при помощи команды `git log` найдём хеш коммита `prepare for merge and rebase` и выполним `git checkout` на него так:
`git checkout 8baf217e80ef17ff577883fda90f6487f67bbcea` (хеш будет другой).

```bash
git log --oneline
git checkout e447118
```
![image](https://github.com/user-attachments/assets/568773df-4551-472b-aa15-e5f3081eb862)


**Шаг 2.** Создадим ветку `git-rebase`, основываясь на текущем коммите.

```bash
git switch -c git-rebase
```

![image](https://github.com/user-attachments/assets/86db6bc3-15cd-4793-ad67-88fbe1a478a7)
 
**Шаг 3.** И изменим содержимое файла `rebase.sh` на следующее, тоже починив скрипт, но немного в другом стиле:

```bash
#!/bin/bash
# display command line options

count=1
for param in "$@"; do
    echo "Parameter: $param"
    count=$(( $count + 1 ))
done

echo "====="
```

**Шаг 4.** Отправим эти изменения в ветку `git-rebase` с комментарием `git-rebase 1`.

```bash
git add branching/
git commit -m "git-rebase 1"
git push origin git-rebase
```
![image](https://github.com/user-attachments/assets/ed7fb946-1d7d-4a52-9bc4-58c7648080c2)


**Шаг 5.** И сделаем ещё один коммит `git-rebase 2` с пушем, заменив `echo "Parameter: $param"` на `echo "Next parameter: $param"`.

```bash
git add branching/
git commit -m "git-rebase 2"
git push origin git-rebase
```

![image](https://github.com/user-attachments/assets/1bd21da1-1789-46a2-b16b-2464bf7c454a)


#### Промежуточный итог  

Мы сэмулировали типичную ситуации в разработке кода, когда команда разработчиков работала над одним и тем же участком кода, и кто-то из разработчиков 
предпочитает делать `merge`, а кто-то — `rebase`. Конфликты с merge обычно решаются просто, 
а с rebase бывают сложности, поэтому давайте смержим все наработки в `main` и разрешим конфликты. 

Если всё было сделано правильно, то на странице `network` в GitHub, находящейся по адресу 
`https://github.com/ВАШ_ЛОГИН/ВАШ_РЕПОЗИТОРИЙ/network`, будет примерно такая схема:
  
![Созданы обе ветки](https://github.com/netology-code/sysadm-homeworks/blob/devsys10/02-git-03-branching/img/01.png)

#### Проверка

![image](https://github.com/user-attachments/assets/15ce6906-4b6b-41bf-ba35-dde440d1b240)


#### Merge

Сливаем ветку `git-merge` в main и отправляем изменения в репозиторий, должно получиться без конфликтов:

```bash
$ git merge git-merge
Merge made by the 'recursive' strategy.
 branching/merge.sh | 5 +++--
 1 file changed, 3 insertions(+), 2 deletions(-)
$ git push
#!/bin/bash
Enumerating objects: 1, done.
Counting objects: 100% (1/1), done.
Writing objects: 100% (1/1), 223 bytes | 223.00 KiB/s, done.
Total 1 (delta 0), reused 0 (delta 0), pack-reused 0
```  

```bash
git merge git-merge
```
![image](https://github.com/user-attachments/assets/36776b2c-6b49-418f-b7cc-0f8db7c45ba8)

В результате получаем такую схему:
  
![Первый мерж](https://github.com/netology-code/sysadm-homeworks/blob/devsys10/02-git-03-branching/img/02.png)


#### Проверка

![image](https://github.com/user-attachments/assets/92e8ef78-a9ce-45bb-ae58-6bda9004ac95)


#### Rebase

**Шаг 1.** Перед мержем ветки `git-rebase` выполним её `rebase` на main. Да, мы специально создали ситуацию с конфликтами, чтобы потренироваться их решать. 
**Шаг 2.** Переключаемся на ветку `git-rebase` и выполняем `git rebase -i main`. 
В открывшемся диалоге должно быть два выполненных коммита, давайте заодно объединим их в один, 
указав слева от нижнего `fixup`. 
В результате получаем:

```bash
$ git rebase -i main
Auto-merging branching/rebase.sh
CONFLICT (content): Merge conflict in branching/rebase.sh
error: could not apply dc4688f... git 2.3 rebase @ instead *
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
Could not apply dc4688f... git 2.3 rebase @ instead *
``` 

```bash
git switch git-rebase
git rebase -i main
```

![image](https://github.com/user-attachments/assets/754a0563-aae9-46cd-a23f-1076318da3b9)


Если посмотреть содержимое файла `rebase.sh`, то увидим метки, оставленные Git для решения конфликта:

```bash
cat rebase.sh
#!/bin/bash
# display command line options
count=1
for param in "$@"; do
<<<<<<< HEAD
    echo "\$@ Parameter #$count = $param"
=======
    echo "Parameter: $param"
>>>>>>> dc4688f... git 2.3 rebase @ instead *
    count=$(( $count + 1 ))
done
```

**Шаг 3.** Удалим метки, отдав предпочтение варианту:

```bash
echo "\$@ Parameter #$count = $param"
```
```



**Шаг 4.** Сообщим Git, что конфликт решён `git add rebase.sh` и продолжим rebase `git rebase --continue`.

```bash
git add branching/rebase.sh
git rebase --continue
```

![image](https://github.com/user-attachments/assets/b6c71998-f28c-4dbe-9b41-bf5a5ff0e61e)


**Шаг 5.** Опять получим конфликт в файле `rebase.sh` при попытке применения нашего второго коммита. Давайте разрешим конфликт, оставив строчку `echo "Next parameter: $param"`.

```
#!/bin/bash
# display command line options

count=1
for param in "$@"; do
    echo "Next parameter: $param"
    count=$(( $count + 1 ))
done

echo "====="
```

**Шаг 6.** Далее опять сообщаем Git о том, что конфликт разрешён — `git add rebase.sh` — и продолжим rebase — `git rebase --continue`.

```bash
git add branching/rebase.sh
git rebase --continue
```
В результате будет открыт текстовый редактор, предлагающий написать комментарий к новому объединённому коммиту:

```
# This is a combination of 2 commits.
# This is the 1st commit message:

Merge branch 'git-merge'

# The commit message #2 will be skipped:

# git 2.3 rebase @ instead * (2)
```

Все строчки, начинающиеся на `#`, будут проигнорированны. 

После сохранения изменения Git сообщит:

```
Successfully rebased and updated refs/heads/git-rebase
```

### Проверка

![image](https://github.com/user-attachments/assets/9351616b-34d1-476f-b960-9849988cb932)


**Шаг 7.** И попробуем выполнить `git push` либо `git push -u origin git-rebase`, чтобы точно указать, что и куда мы хотим запушить. 

Эта команда завершится с ошибкой:

```bash
git push
To github.com:andrey-borue/devops-netology.git
 ! [rejected]        git-rebase -> git-rebase (non-fast-forward)
error: failed to push some refs to 'git@github.com:andrey-borue/devops-netology.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Это произошло, потому что мы пытаемся перезаписать историю. 

```bash
git push -u origin git-rebase
```
![image](https://github.com/user-attachments/assets/e70cd765-b00b-4c91-af9f-9b5cd3d2d795)


**Шаг 8.** Чтобы Git позволил нам это сделать, добавим флаг `force`:

```bash
git push -u origin git-rebase -f
Enumerating objects: 10, done.
Counting objects: 100% (9/9), done.
Delta compression using up to 12 threads
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 443 bytes | 443.00 KiB/s, done.
Total 4 (delta 1), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To github.com:andrey-borue/devops-netology.git
 + 1829df1...e3b942b git-rebase -> git-rebase (forced update)
Branch 'git-rebase' set up to track remote branch 'git-rebase' from 'origin'.
```

**Шаг 9**. Теперь можно смержить ветку `git-rebase` в main без конфликтов и без дополнительного мерж-комита простой перемоткой: 

```
$ git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.

$ git merge git-rebase
Updating 6158b76..45893d1
Fast-forward
 branching/rebase.sh | 3 +--
 1 file changed, 1 insertion(+), 2 deletions(-)
```

*В качестве результата работы по всем заданиям приложите ссылку на .md-файл в вашем репозитории.*
 
----

### Правила приёма домашнего задания

В личном кабинете отправлена ссылка на network графика вашего репозитория.

### Критерии оценки

Зачёт:

* выполнены все задания;
* ответы даны в развёрнутой форме;
* приложены соответствующие скриншоты и файлы проекта;
* в выполненных заданиях нет противоречий и нарушения логики.

На доработку:

* задание выполнено частично или не выполнено вообще;
* в логике выполнения заданий есть противоречия и существенные недостатки.
