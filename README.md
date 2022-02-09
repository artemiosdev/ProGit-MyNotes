# Pro Git  
***Scott Chacon, Ben Straub.*** 
Версия 2.1.95-2-g8d45587, от 19.01.2022 года.  
[https://git-scm.com/book/ru/v2](https://git-scm.com/book/ru/v2)

**Licence**. Это произведение распространяется по свободной лицензии Creative Commons Attribution- NonCommercial-ShareAlike 3.0.   Ознакомиться с текстом лицензии вы можете на сайте [https://creativecommons.org/licenses/by-nc-sa/3.0/deed.ru](https://creativecommons.org/licenses/by-nc-sa/3.0/deed.ru)  

## Введение
**Система контроля версий** — это система, записывающая изменения в файл или набор файлов в течение времени и позволяющая вернуться позже к определённой версии.

**Локальные системы контроля версий** страдают от проблемы: когда вся история проекта хранится в одном месте, вы рискуете потерять всё.

**Распределённые системы контроля версий**. Здесь в игру вступают распределённые системы контроля версий (РСКВ). В РСКВ (таких как Git, Mercurial, Bazaar или Darcs) клиенты не просто скачивают снимок всех файлов (состояние файлов на определённый момент времени) — они полностью копируют репозиторий. В этом случае, если один из серверов, через который разработчики обменивались данными, умрёт, любой клиентский репозиторий может быть скопирован на другой сервер для продолжения работы. Каждая копия репозитория является полным бэкапом всех данных.

**История Git**. В 2005 году сообщество разработчиков ядра Linux (а в частности Линус Торвальдс — создатель Linux) разработали свою собственную утилиту.

Подход Git к хранению данных больше похож на набор снимков миниатюрной файловой системы. Каждый раз, когда вы делаете коммит, то есть сохраняете состояние своего проекта в Git, система запоминает, как выглядит каждый файл в этот момент, и сохраняет ссылку на этот снимок. Для увеличения эффективности, если файлы не были изменены, Git не запоминает эти файлы вновь, а только создаёт ссылку на предыдущую версию идентичного файла, который уже сохранён. Git представляет свои данные как, ***поток снимков***.  
Если вы в самолёте или в поезде и хотите немного поработать, вы сможете создавать коммиты без каких-либо проблем (в вашу локальную копию): когда будет возможность подключиться к сети, все изменения можно будет синхронизировать.

В Git для всего вычисляется **хеш-сумма**, и только потом происходит сохранение. В дальнейшем обращение к сохранённым объектам происходит по этой хеш-сумме. Это значит, что невозможно изменить содержимое файла или каталога так, чтобы Git не узнал об этом. Git сохраняет все объекты в свою базу данных не по имени, а по хеш-сумме содержимого объекта.

У Git есть три основных состояния, в которых могут находиться ваши файлы:   
-***Изменён (modified)*** относятся файлы, которые поменялись, но ещё не были зафиксированы.  
-***Индексирован (staged)*** изменённый файл в его текущей версии, отмеченный для включения в следующий коммит.   
-***Зафиксирован (committed)*** файл уже сохранён в вашей локальной базе.

Рабочая копия является снимком одной версии проекта. Эти файлы извлекаются из сжатой базы данных в каталоге Git и помещаются на диск, для того чтобы их можно было использовать или редактировать.   
**Область индексирования** — это файл, обычно находящийся в каталоге Git, в нём содержится информация о том, что попадёт в следующий коммит. Её техническое название на языке Git — «индекс», но фраза «область индексирования» также работает.
Каталог Git — это то место, где Git хранит метаданные и базу объектов вашего проекта. 

### Установка 
По умолчанию уже стоит в системе, но есть и установка на Mac OS, Windows, Linux, установка напрямую свежей версии из исходников  [https://git-scm.com/download/](https://git-scm.com/download/)   
` git --version ` 

### Первоначальная настройка Git
В состав Git входит утилита git config, которая позволяет просматривать и настраивать параметры, контролирующие все аспекты работы Git, а также его внешний вид. Эти параметры могут быть сохранены в трёх местах:   
1. Файл  `[path]/etc/gitconfig` содержит значения, общие для всех пользователей системы и для всех их репозиториев. Если при запуске git config указать параметр `--system`, то параметры будут читаться и сохраняться именно в этот файл. Так как этот файл является системным, то вам потребуются права суперпользователя для внесения изменений в него.
2. Файл ` ~/.gitconfig или ~/.config/git/config ` хранит настройки конкретного пользователя. Этот файл используется при указании параметра ` --global ` и применяется ко всем репозиториям, с которыми вы работаете в текущей системе.
3. Файл config в каталоге Git (т. е. ` .git/config `) репозитория, который вы используете в данный момент, хранит настройки конкретного репозитория. Вы можете заставить Git читать и писать в этот файл с помощью параметра ` --local `, но на самом деле это значение по умолчанию. Неудивительно, что вам нужно находиться где-то в репозитории Git, чтобы эта опция работала правильно.   
Настройки на каждом следующем уровне подменяют настройки из предыдущих уровней, то есть значения в ` .git/config ` перекрывают соответствующие значения в ` [path]/etc/gitconfig `

Чтобы посмотреть все установленные настройки и узнать где именно они заданы, используйте команду:  

```
git config --list --show-origin
```
 
**Можно указать ваше имя и адрес электронной почты:**

```
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```
Eсли указана опция ` --global `, то эти настройки достаточно сделать только один раз, поскольку в этом случае Git будет использовать эти данные для всего, что вы делаете в этой системе. Если для каких-то отдельных проектов вы хотите указать другое имя или электронную почту, можно выполнить эту же команду без параметра ` --global ` в каталоге с нужным проектом.

Можно выбрать текстовый редактор, который будет использоваться, если будет нужно набрать сообщение в Git. По умолчанию Git использует стандартный редактор вашей системы, обычно Vim. Если вы хотите использовать другой текстовый редактор, например, Emacs, можно проделать следующее:

``` 
git config --global core.editor emacs
```
  
### Настройка ветки по умолчанию
Когда вы инициализируете репозиторий командой git init, Git создаёт ветку с именем master по умолчанию. Вы можете задать другое имя для создания ветки по умолчанию.
Например, чтобы установить имя ` main ` для вашей ветки по умолчанию, выполните следующую команду:

```
git config --global init.defaultBranch main
```
   
Если вы хотите проверить используемую конфигурацию, можете использовать команду ` git config --list `, чтобы показать все настройки, которые Git найдёт:

```
flyboroda@MacBook-Air-Artem ~ % git config --list
credential.helper=osxkeychain
init.defaultbranch=main
user.name=artemiosdev
user.email=name@gmail.com
user.password=********
core.excludesfile=~/.gitignore_global
credential.helper=osxkeychain
credential.username=artemiosdev
alias.last=log -1 HEAD   
```

### Как получить помощь?
Если вам нужна помощь при использовании Git, есть три способа открыть страницу руководства по любой команде Git:

```
git help <команда>
git <команда> --help 
man git-<команда>
```
  
## Основы Git
### Создание репозитория в существующем каталоге.  
Команда создаёт в текущем каталоге новый подкаталог с именем **.git**, содержащий все необходимые файлы репозитория — структуру Git репозитория

``` 
git init
```

Если вы хотите добавить под версионный контроль существующие файлы (в отличие от пустого каталога), вам стоит добавить их в индекс и осуществить первый коммит изменений. Добиться этого вы сможете запустив команду ` git add ` (имеет различные вариации использования). Указав индексируемые файлы, затем выполним ` git commit ` чтобы зафиксировать:

```
git add *.md
git add .
git add <file name>
git commit -m 'Message about commit'
```
Git индексирует файл в точности в том состоянии, в котором он находился, когда вы выполнили команду ` git add `. Команда ` git add ` принимает параметром путь к файлу или каталогу, если это каталог, команда рекурсивно добавляет все файлы из указанного каталога в индекс, это многофункциональная команда, она используется для добавления под версионный контроль новых файлов, для индексации изменений, для указания файлов с исправленным конфликтом слияния, «добавить этот контент в следующий коммит».

### Клонирование существующего репозитория    

```
git clone 'https://github.com/artemiosdev/ProGit-MyNotes'
```
В Git реализовано несколько транспортных протоколов, которые вы можете использовать. В предыдущем примере использовался протокол ` https:// `, вы также можете встретить ` git:// ` или ` user@server:path/to/repo.git `, использующий протокол передачи **SSH**.

### Определение состояния файлов   
Основной инструмент, используемый для определения, какие файлы в каком состоянии находятся — это команда
 
```
git status
```

### Игнорирование файлов   
Зачастую, у вас имеется группа файлов, которые вы не только не хотите автоматически добавлять в репозиторий, но и видеть в списках неотслеживаемых. К таким файлам обычно относятся автоматически генерируемые файлы (различные логи, результаты сборки программ и т. п.). В таком случае, вы можете создать файл ` .gitignore `. с перечислением шаблонов соответствующих таким файлам. И при коммите указанные файлы будут проигнорированы. 

Хорошая практика заключается в настройке файла `.gitignore ` до того, как начать серьёзно работать, это защитит вас от случайного добавления в репозиторий файлов, которых вы там видеть не хотите.   
К шаблонам в файле ` .gitignore ` применяются следующие правила:
* Пустые строки, а также строки, начинающиеся с ` # `, игнорируются.
* Стандартные шаблоны являются глобальными и применяются рекурсивно для всего дерева каталогов.
* Чтобы избежать рекурсии используйте символ слеш ` (/) ` в начале шаблона.
* Чтобы исключить каталог добавьте слеш ` (/) ` в конец шаблона.
* Можно инвертировать шаблон, использовав восклицательный знак ` (!) ` в качестве первого символа.

Glob-шаблоны представляют собой упрощённые регулярные выражения, используемые командными интерпретаторами. Символ ` (*) ` соответствует 0 или более символам; последовательность ` [abc]  ` — любому символу из указанных в скобках (в данном примере a, b или c); знак вопроса ` (?) ` соответствует одному символу; и квадратные скобки, в которые заключены символы, разделённые дефисом ` ([0-9]) `, соответствуют любому символу из интервала (в данном случае от 0 до 9). Вы также можете использовать две звёздочки, чтобы указать на вложенные каталоги: ` a/**/z ` соответствует ` a/z, a/b/z, a/b/c/z, ` и так далее.

Пример файла ` .gitignore `:

```   
# Исключить все файлы с расширение .a
*.a

# Но отслеживать файл lib.a даже если он подпадает под исключение выше 
!lib.a

# Исключить файл TODO в корневом каталоге, но не файл в subdir/TODO 
/TODO

# Игнорировать все файлы в каталоге build/ 
build/

# Игнорировать файл doc/notes.txt, но не файл doc/server/arch.txt 
doc/*.txt

# Игнорировать все .txt файлы в каталоге doc/ 
doc/**/*.txt
```

GitHub поддерживает довольно полный список примеров ` .gitignore   ` файлов для множества проектов и языков [https://github.com/github/gitignore](https://github.com/github/gitignore) 


### Просмотр индексированных и неиндексированных изменений
Чтобы увидеть, что же вы изменили, но пока не проиндексировали, наберите ` git diff ` без аргументов

Эта команда сравнивает содержимое вашего рабочего каталога с содержимым индекса. Результат показывает ещё не проиндексированные изменения.
Если вы хотите посмотреть, что вы проиндексировали и что войдёт в следующий коммит, вы можете выполнить ` git diff --staged ` (или ` git diff --cached ` для просмотра проиндексированных изменений (` --staged ` и ` --cached ` синонимы)). Эта команда сравнивает ваши проиндексированные изменения с последним коммитом:

```
git diff --staged
 or
git diff --cached
```

` git diff ` сама по себе не показывает все изменения сделанные с последнего коммита — только те, что ещё не проиндексированы. Такое поведение может сбивать с толку, так как если вы проиндексируете все свои изменения, то git diff ничего не вернёт.

### Коммит изменений
Простейший способ зафиксировать изменения — это набрать ` git commit `
Для ещё более подробного напоминания, что же именно вы поменяли, можете передать аргумент `-v` в команду `git commit`. Это приведёт к тому, что в комментарий будет также помещена дельта/diff изменений, таким образом вы сможете точно увидеть все изменения которые вы совершили.
Вы можете набрать свой комментарий к коммиту в командной строке вместе с командой `commit` указав его после параметра `-m`

```
git commit -m "Message about commit"
```

Запомните, что коммит сохраняет снимок состояния вашего индекса. Всё, что вы не проиндексировали, так и висит в рабочем каталоге как изменённое; вы можете сделать ещё один коммит, чтобы добавить эти изменения в репозиторий. Каждый раз, когда вы делаете коммит, вы сохраняете снимок состояния вашего проекта, который позже вы можете восстановить или с которым можно сравнить текущее состояние.

### Пропуск индексации
Если у вас есть желание пропустить этап индексирования (т.е добавления файлов командой `git add`), Git предоставляет простой способ. Добавление параметра `-a` в команду `git commit` заставляет Git автоматически индексировать каждый уже отслеживаемый на момент коммита файл, позволяя вам обойтись без `git add`:

```
git commit -a -m 'Message about commit'
```

В данном случае перед коммитом вам не нужно выполнять `git add` для файла, потому что флаг `-a` включает все файлы. Это удобно, но будьте осторожны: флаг `-a` может включить в коммит нежелательные изменения.

### Удаление файлов
Для того чтобы удалить файл из Git, вам необходимо удалить его из отслеживаемых файлов (точнее, удалить его из вашего индекса) а затем выполнить коммит. Это позволяет сделать команда `git rm`, которая также удаляет файл из вашего рабочего каталога, так что в следующий раз вы не увидите его как «неотслеживаемый».

Если вы изменили файл и уже проиндексировали его, вы должны использовать принудительное удаление с помощью параметра `-f`. Это сделано для повышения безопасности, чтобы предотвратить ошибочное удаление данных, которые ещё не были записаны в снимок состояния и которые нельзя восстановить из Git.
Другая полезная штука, которую вы можете захотеть сделать — это удалить файл из индекса, оставив его при этом в рабочем каталоге. Другими словами, вы можете захотеть оставить файл на жёстком диске, но перестать отслеживать изменения в нём. Это особенно полезно, если вы забыли добавить что-то в файл `.gitignore` и по ошибке проиндексировали, например, большой файл с логами, или кучу промежуточных файлов компиляции. Чтобы сделать это, используйте опцию ` --cached`:

```
git rm --cached <file name>
```

В команду `git rm` можно передавать файлы, каталоги или шаблоны. 

```
git rm log/\*.log
```
Обратите внимание на обратный слеш ` \ ` перед ` * `. Он необходим из-за того, что Git использует свой собственный обработчик имён файлов вдобавок к обработчику вашего командного интерпретатора. Эта команда удаляет все файлы, имеющие расширение `.log` и находящиеся в каталоге `log/`.    
Или же вы можете сделать вот так:

```
git rm \*~
```
Эта команда удаляет все файлы, имена которых заканчиваются на ` ~ `.

### Перемещение файлов


  







