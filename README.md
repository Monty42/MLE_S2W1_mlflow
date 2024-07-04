# Вебинар "Интеграция MLflow в процесс разработки моделей машинного обучения"

## Курс "Инженер машинного обучения", Яндекс Практикум

>### Описание кейса

Ваш бывший коллега ушел в отпуск. А заказчик требует результатов уже на этой неделе. Сегодня вам предстоит:

1. Посмотреть на результаты работы коллеги
2. Разобраться в этих материалах
3. Зарегистрировать модель на удаленном сервере MLFlow
4. Подготовить инференс-выгрузку для заказчика

> Напоминаю: Дедлайн был вчера, поэтому быстрее за работу)

>### Немного о данных

1. Данные лежат в папке `data`
2. Данные представляют собой выгрузки для классификации на поиск сарказма
3. Файл `Sarcasm_Headlines_Dataset_v2.json` -- предназначен для обучения модели
4. Файл `Sarcasm_Headlines_Dataset.json` -- предназначен для инференса модели

>### Как работать с проектом?

>#### 1. Скачивание и установка зависимостей

1. Чтобы редактировать репозиторий, необходимо сделать его копию. Это делается по кнопке `fork`.

2. Клонируем репозиторий на своё локальное устройство (но лучше все-таки на виртуальное устройство):

~~~
git clone <адрес вашей копии репозитория>
~~~

3. Создаём новую ветку:

~~~
git checkout -b <имя новой ветки>
~~~

4. Установка нужных библиотек и окружения:

~~~
python -m venv <имя папки для хранения окружения>
source <имя папки для хранения окружения>/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
~~~

P.S. Некоторые зависимости могут мешать установить ту или иную библиотеку. Для избежания этого можно удалить библиотеки, которые вылетают с ошибкой, в отдельный файл, после чего с помощью флага `--no-deps` установить эти библиотеки. Тогда команда примет вид:

~~~
pip install -r requirements_2.txt --no-deps
~~~

5. Переименуйте `.env_template` файл в `.env`. Так как вы будете запускать и хранить все артефакты на удаленном устройстве, вам нужно сохранить те параметры, которые указаны в этом файле.

6. Готово! Можно проверить работоспособность окружения.

>#### 2. Что же на данный момент готово?

Ваш коллега успел обучить несколько простых пайпланов, а также провел подбор гиперпараметров.
![image](pictures/Experiments.png)

В ходе экспериментов коллега максимизировал определенную метрику качества. Но не совсем понятно что именно это за метрика и почему он выбрал именно ее.
![image](pictures/Metrics.png)

Радует тот факт, что он оставил хотя бы какую-то информацию, и нам не придется начинать сначала.

>#### 3. Практическое задание

1. Зайдите на сервер MLflow (нужно постучаться по этому адресу <http://84.201.139.14:5000>)
2. Выберите эксперимент, получите информацию о моделях, которые в нём содержатся.
    - Создайте ноутбук с любым названием для работы.
    - Импортируйте модель
    - Попробуйте выполнить инференс модели на любом кусочке данных
3. Создайте файл MLproject. Сделайте эксперимент воспроизводимым, добавив несколько entry_points (примеры entry_points представлены в файле)
    - Для реализации отправки параметров через терминал можете использовать модуль `click` и декораторы `@click.option()` & `@click.command()`
4. Выполните запуск эксперимента через MLproject, измените статус полученной модели на «production»
    - Измените название запуска (в качестве имени предлагаю использовать иницалы)
    - В качестве команды запуска используйте `mlflow run . --entry-point <entry-point name>`
    - Попробуйте запустить только 2 основных этапа `prepare_data` & `baseline_model`
    - Если будете запускать этап optuna помните, что 10 итераций выполняется от 2 до 12 минут
    - В качестве файла с данными используйте `Sarcasm_Headlines_Dataset.json`
5. Измените `staging` моделей
6. Получите модель, которую зарегистрировали на предыдущем шаге, и сделайте инференс файла `Sarcasm_Headlines_Dataset_2.json`
7. Полученный результат сохраните в формате .csv и добавьте в отслеживаемые файлы Git. После чего сделайте PR в main

>#### 4. Шпаргалка для работы с гитом

1. Добавляем изменения в индекс:

~~~
git add .
~~~

2. Создаём коммит:

~~~
git commit -m "Описание того, что было изменено"
~~~

3. Отправляем изменения в удалённый репозиторий:

~~~
git push
~~~

Если вы локально создали новую ветку, эта команда не сработает, но git сам подскажет, как её модернизировать, чтобы добавить ветку. Можно просто скопировать :)

>#### 5. Архитектура проекта

==============================

    ├── README.md 
    │         
    ├── data               <- Папка c локальными данными
    │
    ├── notebooks          <- Ноутбук с разведочным анализом данных
    │
    ├── src                <- Здесь хранятся заготовки под скрипты
    │
    ├── requirements.txt   <- Файл с зависимостями
    │
    ├── python_venv.yaml   <- Окружение проекта для MLflow
    │
    ├── .env_template      <- Файл с параметрами вашего окружения. Необходимо создать файл `.env` или переименовать этот файл и добавить в него свои параметры.
    │
    └── MLProject          <- Сущность для бесперебойной работы моделей            

-----------------

>#### 6. В случае ошибок

Так как ошибки неизбежны, предлагаю выполнять следующие действия:

Если действие происходит на вебинаре:

1. Не стесняйтесь задать вопрос. Вероятно кто-то столкнулся с такой же проблемой, вместе мы сможем решить проблему быстрее.
2. Желательно скинуть скрины кода, который вы написали и место с ошибкой, если такое имеется.

Если вы попрактиковаться за пределами вебинара:

1. Погуглите, скорее всего кто-то уже пытался это решить.

2. Если не понимаете, что нагуглили, или не нагуглили вовсе, предлагаю 2 доступных варианта:

    - создать тему с обсуждением на GitHub и приложить свою ошибку, мы постараемся вам помочь.
    - Создайте PullRequest в `main` и внутри описания изменений напишите свою ошибку, тогда мы сможем помочь оперативнее. Мы смотрим ваши пулреквесты.

> ## Мы будем рады, если данный код поможет вам попрактиковать свои навыки в работе с MLflow и запуском сторонних репозиториев 🥸
