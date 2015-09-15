# TARS-CLI

[![Build Status](https://travis-ci.org/tars/tars-cli.svg?branch=master)](https://travis-ci.org/tars/tars-cli) [![Dependency Status](https://david-dm.org/tars/tars-cli.svg)](https://david-dm.org/tars/tars-cli)

TARS-CLI — Command Line Interface для сборщика верстки [TARS](https://github.com/tars/tars).

Основная проблема при разработке верстки с помощью TARS — необходимость каждый раз устанавливать все npm-зависимости. Каждый проект в результате занимает больше 200 МБ. Чтобы упростить процедуру инициализации проекта и облегчить работу с TARS в целом был создан TARS-CLI. Вся основная документация по TARS находится в оригинальном репозитории [TARS](https://github.com/tars/tars).

TARS-CLI — это интерфейс к основному сборщику, который позволяет:

* Инициализировать проект.
* Запустить dev-сборку с перезагрузкой браузера и открытием тунеля во внешний веб.
* Запустить build-сборку с минифицированными файлами или в режиме release.
* Добавить модуль с различным набором файлов.
* Добавить страницу, как пустую, так и копию существующей.

## Установка

Для корректной работы необзодимо установить gulp глобально:

`npm i -g gulp`

Затем устанавливаем TARS-CLI, также глобально:

`npm i -g tars-cli`

Возможно потребуются права суперюзера. Но желательно настроить систему так, чтобы этого не требовалось.

## Команды TARS-CLI

Все команды запускаются по шаблону:

`tars` + `command-name` + `flags`

В любой момент можно запустить `tars --help` или `tars -h` или просто `tars`, без дополнительных комманд и флагов. Данная команда выведет информацию о всех доступных командах. Также можно добавить ключ `--help` или `-h` к любой команде, чтобы получить наиболее полное описание этой команды.

`tars -v` или `tars --version` выведет текущую установленную версию TARS-CLI.

### tars init

Данная команда позволяет инициализировать TARS в текущей директории. Важно, чтобы директория была пуста на момент инициализации. Запускает команду `gulp init` в TARS.

#### Доступные флаги

* `-s`, `--source`: по умолчанию init скачает из репозитория TARS последнюю врсию сборщика и распакует в текущей директории. С помощью флага `-s` можно определить, откуда скачивать zip-архив с TARS.
* `--silent`: запускает re-init без диалога об опциях.
    
#### Пример использования команды

````bash
tars init

tars init --silent

tars init -s http://url.to.tars.zip

tars init --silent --source http://url.to.tars.zip
````

### tars re-init

Даннай команда позволяет реинициализировать TARS с новыми настройками (шаблонизатор, препроцессор). Запускает команду `gulp re-init` в TARS.

#### Доступные флаги

* `--silent`: запускает re-init без диалога об опциях.

#### Пример использования команды

````bash
tars re-init

tars re-init --silent
````

### tars dev

Данная команда запускает dev-сборку, с запуском вотчеров. Запускает команду `gulp dev` в TARS.

#### Доступные флаги

* `-l`, `--livereload`, `--lr`: запускает лайврелоад в браузере.
* `-t`, `--tunnel`: инициализация проекта с расшариванием верстки во внешний веб.
* `--ie8`: включить в сборку стили для ie8.

#### Пример использования команды

````bash
tars dev

tars dev -l

tars dev --tunnel --ie8
````

### tars build

Данная команда запускает dev-сборку, с запуском вотчеров. Запускает команду `gulp dev` в TARS.

#### Доступные флаги

* `-m`, `--min`: в html подключаются минимизированные файлы.
* `-r`, `--release`: в html подключаются минимизированные файлы, в названии которых есть hash. Данный режим полезен, если вы напрямую выкладываете верстку на сервер. 
* `--ie8`: включить в сборку стили для ie8.

#### Пример использования команды

````bash
tars build

tars build -m

tars build --release --ie8
````

### tars add-module <moduleName>

Команда добавляет модуль в проект. В качестве параметра принимает имя модуля. Если модуль уже существует, будет выдана соответствующая ошибка. По умолчанию создается просто папка под модуль. Чтобы создать модуль с готовыми файлами — необходимо пользоваться флагами.

#### Доступные флаги

* `-f`, `--full`: добавляет модуль со всеми папками и файлами, которые могут быть в модуле:  папка для assets, ie, data + файл выбранного шаблонизатора, js и выбранного препроцессора.
* `-i`, `--ie`: добавляет все основные файлы и папку для стилей для IE.
* `-a`, `--assets`: добавляет все основные файлы и папку для assets.
* `-d`, `--data`: добавляет все основные файлы и папку для data. Также создается файл с данными в виде:
````javascript
moduleName: {}
````
* `--basic`, `-b`: добавляет только основные файлы.

#### Пример использования команды

````bash
tars add-module sidebar

tars add-module sidebar -a

tars add-module sidebar --full
````
    
### tars add-page <pageName>

Команда добавляет новую страницу в markup/pages. В качестве параметра принимает имя страницы. Если странциа уже существует, будет выдана соответствующая ошибка. Есть возможность добавить пустую страницу, так и копию шаблонной (по умолчанию это _template.{html, jade, hbs}). Вы можете создать свой _template.{html, jade, hbs}, чтобы TARS-CLI копировал именно эту страницу.

#### Доступные флаги

* `-e`, `--empty`: добавит пустую страницу.

#### Пример использования команды

````bash
tars add-page inner

tars add-page inner.html

tars add-page inner -e
````

###  tars update

Обновит текущую версию TARS-CLI до последней доступной. Под капотом запускает npm update -g tars-cli.

#### Пример использования команды

````bash
tars update
````

По всем вопросам и предложениям сюда: [tars.builder@gmail.com](tars.builder@gmail.com)

