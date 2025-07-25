---
title: Setting up a Django development environment
slug: Learn_web_development/Extensions/Server-side/Django/development_environment
---

{{LearnSidebar}}

{{PreviousMenuNext("Learn/Server-side/Django/Introduction", "Learn/Server-side/Django/Tutorial_local_library_website", "Learn/Server-side/Django")}}

Теперь, когда вы знаете, что такое Django, мы покажем вам, как настроить и протестировать среду разработки Django для Windows, Linux (Ubuntu) и Mac OS X - какую бы операционную систему вы не использовали, эта статья должна дать вам все, что необходимо для возможности начать разрабатывать приложения Django.

| Требования: | Знание как открыть терминал / командную строку, как устанавливать программные пакеты в операционной системе вашего компьютера. |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------ |
| Задача:     | Создать среду разработки для Django (1.10) и запустить её на вашем компьютере.                                                 |

## Обзор среды разработки Django

Django упрощает настройку собственного компьютера, чтобы вы могли начать разработку веб-приложений. В этом разделе объясняется, что входит в состав среды разработки, и даётся обзор некоторых параметров настройки и конфигурации. В оставшейся части статьи объясняется рекомендуемый метод установки среды разработки Django на Ubuntu, Mac OS X и Windows, и как вы можете её протестировать.

### Что такое среда разработки Django?

Среда разработки - это установка Django на вашем локальном компьютере, которую вы можете использовать для разработки и тестирования приложений Django до их развёртывания в производственной среде.

Основными инструментами, которые предоставляет сам Django, является набор скриптов Python для создания и работы с проектами Django, а также простой веб-сервер _разработки_, который можно использовать для тестирования локальных (то есть на вашем компьютере, а не на внешнем веб-сервере) веб-приложений Django на веб-браузере вашего компьютера.

Существуют и другие периферийные инструменты, являющиеся частью среды разработки, которые мы не будем освещать здесь. К ним относятся такие вещи, как текстовый редактор или IDE для редактирования кода, и инструмент управления исходным кодом, например Git, для безопасного управления различными версиями вашего кода. Мы предполагаем, что у вас уже установлен текстовый редактор.

### Какие есть разновидности установки Django ?

Django очень гибок с точки зрения способа и места установки и настройки. Django может быть:

- установлен на различных операционных системах,
- установлен из исходного кода, из Python Package Index (PyPi) и во многих случаях из любого менеджера пакетов,
- настроен на использование различных баз данных, которые должны быть установлены и настроены отдельно,
- запущен в основной системе окружения Python или в отдельном виртуальном окружении Python.

Каждый из этих вариантов требует немного разной настройки и установки. Следующие подразделы объяснят некоторые аспекты вашего выбора. Далее мы покажем вам, как установить Django на некоторые операционные системы, и эта установка будет предполагаться на всём протяжении данного модуля.

> [!NOTE]
> Другие возможные способы установки можно найти в официальной документации Django. Мы ссылаемся на [соответствующие документы](#furtherreading).

#### Какие операционные системы поддерживаются?

Веб-приложения Django можно запускать почти на любых машинах, которые поддерживают язык программирования Python 3, среди прочих: Windows, Mac OS X, Linux/Unix, Solaris. Почти любой компьютер имеет необходимую производительность для запуска Django во время разработки.

В этой статье мы предоставляем инструкции для Windows, Mac OS X, and Linux/Unix.

#### Какую версию Python стоит использовать?

Мы рекомендуем использовать самую последнюю доступную версию - на момент написания статьи это Python 3.6.

> [!NOTE]
> Python 2.7 не может быть использован вместе с Django 2.0 (последние поддерживаемые серии для Python 2.7 - Django 1.11.x).

#### Откуда можно скачать Django?

Для загрузки Django можно воспользоваться 3 источниками:

- The Python Package Repository (PyPi), при помощи инструмента _pip_. Это лучший способ получения последней стабильной версии Django.
- Использование версии из менеджера пакетов вашего компьютера. Такие дистрибутивы Django, собранные для конкретных операционных систем, предлагают знакомый механизм установки. Однако обратите внимание на то, что пакетные версии могут быть достаточно старыми и установлены только в системную среду Python (что может отличаться от ваших желаний).
- Установка из исходного кода. Вы можете получить и установить последний выпуск Django из исходного кода. Этот способ не рекомендован для новичков, но необходим в случае, когда вы готовы начать вносить собственный вклад в проект Django.

Данный материал описывает способ установки Django из PyPi с целью получения последней стабильной версии.

#### Какую базу данных выбрать?

Django поддерживает 4 основных базы данных (PostgreSQL, MySQL, Oracle и SQLite), также есть публичные библиотеки, которые предоставляют разные уровни поддержки других SQL и NoSQL баз данных. Мы рекомендуем вам выбрать одинаковую БД для обеих рабочей и разрабатываемой сред (несмотря на то, что Django нивелирует множество различий баз данных при помощи Object-Relational Mapper (ORM), всё равно возможны потенциальные [проблемы](https://docs.djangoproject.com/en/2.0/ref/databases/), которых лучше избегать.

Для данной статьи (и большей части модуля) мы будем использовать базу данных _SQLite_, которая сохраняет свои данные в файл. SQLite предназначен для использования в качестве облегчённой базы данных и не может поддерживать высокий уровень параллелизма. Это, однако, отличный выбор для приложений, которые в основном предназначены только для чтения.

> [!NOTE]
> Django сконфигурирован для использования SQLite по умолчанию, при создании вашего проекта с использованием стандартных инструментов (django-admin). Это отличный выбор для начала работы, потому что он не требует дополнительной настройки.

#### Глобальная установка или установка в виртуальную среду Python?

Когда вы устанавливаете Python 3 на свой компьютер, вы получаете единую глобальную среду (набор установленных пакетов) для вашего кода Python, которая доступна любому коду на компьютере. Вы можете установить любые пакеты Python, необходимые вам в этой среде, но только одну конкретную версию конкретного пакета.

> [!NOTE]
> Установленные в глобальную среду приложения Python потенциально могут конфликтовать друг с другом (т.е. если они зависят от разных версий одного и того же пакета).

Если вы устанавливаете Django в среду по умолчанию (глобальную), то будете способны сфокусироваться на одной версии Django на вашем компьютере. Это может быть проблемой в случае, если вы захотите создать новые веб-сайты (при помощи новой версии Django) во время поддержки веб-сайтов со старой версией.

По этой причине опытные разработчики Python / Django часто предпочитают вместо этого запускать свои приложения Python в независимых _виртуальных средах Python_. Это позволяет разработчикам иметь несколько разных сред Django на одном компьютере. Команда разработчиков Django сама рекомендует использовать виртуальные среды Python!

Этот модуль предполагает вашу установку Django в виртуальную среду, и мы покажем, как это сделать.

## Установка Python 3

Для использования Django вам необходимо установить _Python 3_ на свою операционную систему. Вам также понадобится инструмент [Python Package Index](https://pypi.python.org/pypi) — _pip3_ — который используется для управления (установка, обновление и удаление) библиотек/пакетов Python, используемых Django и другими вашими приложениями Python.

Этот раздел коротко описывает то, как вы можете проверить имеющиеся версии и при необходимости установить новые для Ubuntu Linux 16.04, Mac OS X, and Windows 10.

> [!NOTE]
> В зависимости от платформы, вы можете иметь возможность установки Python/pip из собственного менеджера пакетов операционной системы или при помощи других инструментов. Для большинства платформ вы можете скачать необходимые установочные файлы из <https://www.python.org/downloads/> и установить их при помощи соответствующего специфичного для платформы метода.

### Ubuntu 16.04

Ubuntu Linux включает в себя Python 3 по умолчанию. Вы можете удостовериться в этом, выполнив следующую команду в терминале:

```
python3 -V
 Python 3.5.2
```

Однако, инструмент Python Package Index, при помощи которого вам нужно будет установить пакеты для Python 3 (включая Django), по умолчанию **не установлен**. Вы можете установить pip3 через терминал bash при помощи:

```
sudo apt-get install python3-pip
```

### Mac OS X

Mac OS X "El Capitan" не включает Python 3. Вы можете удостовериться в этом, выполнив следующую команду в терминале:

```
python3 -V
 -bash: python3: command not found
```

Вы можете легко установить Python 3 (вместе с инструментом _pip3_) с [python.org](https://www.python.org/):

1. Скачайте нужный установочный файл:
   1. Перейдите в <https://www.python.org/downloads/>
   2. Нажмите на кнопку **Скачать Python 3.6.4** (точная основная версия может отличаться).

2. Найдите файл при помощи _Finder_, дважды кликните по нему и следуйте подсказкам по установке.

Удостовериться в успешной установке вы можете проверкой на наличие _Python 3_, как показано ниже:

```
python3 -V
 Python 3.5.20
```

Подобным образом вы можете проверить установку _pip3_, отобразив список доступных пакетов:

```
pip3 list
```

### Windows 10

Windows не включает Python по умолчанию, но вы можете легко установить его (вместе с инструментом _pip_) с [python.org](https://www.python.org/):

1. Скачайте нужный установочный файл:
   1. Перейдите в <https://www.python.org/downloads/>
   2. Нажмите на кнопку **Скачать Python 3.6.4** (точная основная версия может отличаться).

2. Установите Python, дважды кликнув на скачанный файл и следуя инструкциям по установке.

После этого вы сможете подтвердить успешную установку Python путём выполнения следующего текста в командной строке:

```
py -3 -V
 Python 3.5.2
```

Установщик Windows включает в себя _pip3_ (менеджер пакетов Python) по умолчанию. Вы можете отобразить список установленных пакетов, как показано далее:

```
pip list
```

> [!NOTE]
> Установщик должен сделать все, что необходимо для корректной работы указанной команды. Однако, если вы видите сообщение о том, что Python не может быть найден, вам может потребоваться добавить его в системный путь.

## Использование Django внутри виртуальной среды Python

Для создания виртуальных сред мы будем использовать библиотеки [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/index.html) (Linux и macOS X) и [virtualenvwrapper-win](https://pypi.python.org/pypi/virtualenvwrapper-win) (Windows), которые в свою очередь обе используют инструмент [virtualenv](https://github.com/mdn/archived-content/tree/main/files/en-us/mozilla/virtualenv). Инструмент обёртки предоставляет совместимый интерфейс для управления интерфейсами на всех платформах.

### Установка ПО виртуальной среды

#### Установка виртуальной среды для Ubuntu

После установки Python и pip вы можете установить _virtualenvwrapper_ (который включает в себя _virtualenv_). Вы можете либо воспользоваться официальной инструкций по установке [отсюда](http://virtualenvwrapper.readthedocs.io/en/latest/install.html), либо следовать следующим инструкциям:

Установите инструмент при помощи pip3:

```
sudo pip3 install virtualenvwrapper
```

Затем добавьте следующие строки в конец файла загрузки программной оболочки (shell) (это скрытый файл в вашей домашней директории с именем **.bashrc**). Они устанавливают расположение виртуальных сред, расположение каталога разрабатываемого проекта и расположение установленного с этим пакетом скрипта.

```
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export VIRTUALENVWRAPPER_VIRTUALENV_ARGS=' -p /usr/bin/python3 '
export PROJECT_HOME=$HOME/Devel
source /usr/local/bin/virtualenvwrapper.sh
```

Затем перезагрузите файл загрузки, выполнив в терминале следующую команду:

```
source ~/.bashrc
```

В этот момент вы должны увидеть запуск группы скриптов, как показано ниже:

```
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/premkproject
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/postmkproject
...
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/preactivate
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/postactivate
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/get_env_details
```

Теперь вы можете создать новую виртуальную среду при помощи команды `mkvirtualenv`.

#### Установка виртуальной среды для macOS X

Установка _virtualenvwrapper_ на macOS X почти идентична Ubuntu (и снова вы можете воспользоваться либо [официальными](http://virtualenvwrapper.readthedocs.io/en/latest/install.html), либо следующими инструкциями).

Установите инструмент при помощи pip3:

```
sudo pip3 install virtualenvwrapper
```

Затем добавьте следующие строки в конец вашего файла загрузки программной оболочки:

```
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
export PROJECT_HOME=$HOME/Devel
source /usr/local/bin/virtualenvwrapper.sh
```

> [!NOTE]
> Переменная `VIRTUALENVWRAPPER_PYTHON` указывает на обычное расположение Python3. Если virtualenv не работает во время тестирования, то вам следует проверить, находится ли интерпретатор Python в нужном расположении (и затем поменять его соответствующим образом в значении переменной).

Эти строки такие же, как в случае с Ubuntu, но файл загрузки в вашей домашней директории назван иначе - **.bash_profile**.

> [!NOTE]
> Если вы не можете найти и изменить **.bash_profile** при помощи Finder, то можно также открыть его при помощи редактора терминала _nano_.
>
> Команды в этом случае выглядят примерно так:
>
> ```
> cd ~ # Navigate to my home directory
> ls -la #List the content of the directory. You should see .bash_profile
> nano .bash_profile # Open the file in the nano text editor, within the terminal
> # Scroll to the end of the file, and copy in the lines above
> # Use Ctrl+X to exit nano, Choose Y to save the file.
> ```

После этого перезагрузите файл загрузки путём выполнения следующей команды в терминале:

```
source ~/.bash_profile
```

В этот момент вы должны увидеть запуск группы скриптов (те же скрипты, что и в случае установки на Ubuntu).

Теперь вы должны иметь возможность создания новой виртуальной среды при помощи команды `mkvirtualenv`.

#### Установка виртуальной среды для Windows 10

Установка [virtualenvwrapper-win](https://pypi.python.org/pypi/virtualenvwrapper-win) ещё более проста, чем установка _virtualenvwrapper_, потому что вам не нужно настраивать расположения сохранения информации о виртуальной среде инструментом (эти значения заданы по умолчанию). Все, что вам нужно сделать, это запустить следующую команду в командной строке:

```
pip3 install virtualenvwrapper-win
```

Теперь вы можете создать новую виртуальную среду при помощи команды `mkvirtualen`.

### Создание виртуальной среды

После установки _virtualenvwrapper_ и _virtualenvwrapper-win_ работа с виртуальными средами становится одинаковой для всех платформ.

Теперь вы можете создать новую виртуальную среду при помощи команды `mkvirtualenv`. Во время запуска команды вы увидите установку виртуальной среды (конкретные результаты команды очень зависят от платформы). После выполнения команды активируется новая виртуальная среда — заметить это вы можете по тому, что началом ввода будет название виртуальной среды в круглых скобках (как показано ниже).

```
$ mkvirtualenv my_django_environment
Running virtualenv with interpreter /usr/bin/python3 ...
virtualenvwrapper.user_scripts creating /home/ubuntu/.virtualenvs/t_env7/bin/get_env_details
(my_django_environment) ubuntu@ubuntu:~$
```

Теперь вы находитесь внутри виртуальной области и можете установить Django и начать разработку.

> [!NOTE]
> С этого момента в этой статье (и всем модуле) пожалуйста учитывайте, что любые команды запускаются в виртуальной среде Python, как та, что мы показали выше.

### Использование виртуальной среды

Есть ещё несколько полезных команд, которые вам следует знать (в документации по инструменту их гораздо больше, но эти вы будете использовать регулярно):

- `deactivate` — Выход из текущей виртуальной среды Python
- `workon` — Список доступных виртуальных сред
- `workon name_of_environment` — Активация конкретной виртуальной среды Python
- `rmvirtualenv name_of_environment` — Удаление конкретной виртуальной среды.

## Установка Django

После создания виртуальной среды и вызова `workon` для входа в неё вы можете использовать _pip3_ для установки Django.

```
pip3 install django
```

Вы можете проверить установку Django, выполнив следующую команду (она просто проверяет, что Python может найти модуль Django):

```
# Linux/Mac OS X
python3 -m django --version
 1.10.10

# Windows
py -3 -m django --version
 1.10.10
```

> [!NOTE]
> Для Windows вы запускаете скрипты _Python 3_ с префиксом команды `py -3`, в то время как для Linux/Mac OSX префикс - `python3`.

> **Предупреждение:** **Важно**: В оставшейся части материала используется вариант команды _Linux_ для вызова Python 3 (`python3`) . Если вы работаете в _Windows,_ то просто замените этот префикс на: `py -3`

## Проверка вашей установки

Указанная выше проверка работает, но не представляет особого интереса.Более интересная проверка заключается в создании шаблона проекта и проверки его работы. Для её выполнения перейдите в командной строке/терминале в место, где планируете сохранять приложения Django. Создайте папку для теста и перейдите в неё.

```bash
mkdir django_test
cd django_test
```

Затем вы можете создать шаблон сайта "_mytestsite_" при помощи инструмента **django-admin**. После создания сайта вы можете перейти в папку, где найдёте основной скрипт для управления проектами с именем **manage.py**.

```bash
django-admin startproject mytestsite
cd mytestsite
```

Мы можем запустить веб-сервер разработки из этой папки при помощи **manage.py** и команды `runserver`, как показано ниже.

```bash
$ python3 manage.py runserver
Watching for file changes with StatReloader
Performing system checks…

System check identified no issues (0 silenced).

You have 18 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.
March 01, 2022 - 01:19:16
Django version 4.0.2, using settings 'mytestsite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

> [!NOTE]
> Указанная команда демонстрирует выполнение для Linux/Mac OS X. В настоящий момент вы можете проигнорировать предупреждения о "13 непримененных миграциях"!

Как только сервер запущен, вы можете посмотреть сайт, перейдя по следующему адресу в вашем браузере: `http://127.0.0.1:8000/`. Вы должны увидеть, что сайт выглядит следующим образом:

![Django Skeleton App Homepage](django_skeleton_website_homepage.png)

## Заключение

Теперь у вас на компьютере установлена и запущена среда разработки Django.

В разделе проверки вам коротко был показан способ создания нового сайта на Django при помощи `django-admin startproject` и его запуск в вашем браузере при помощи веб-сервера разработки (**`python3 manage.py runserver`**). В следующей статье мы подробнее рассмотрим этот процесс создания простого, но полноценного веб-приложения.
