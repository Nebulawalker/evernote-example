# evernote-example
Упражнение на чтение кода курсов [Devman](https://dvman.org/).

Набор скриптов для работы с заметками Evernote. Реализован запрос списка блокнотов, запрос и отображение текста заметок указанного блокнота, а также создание новой заметки по шаблонной заметке.

Скрипты предназначены для работы на тестовом сервисе [Evernote Sandbox](https://sandbox.evernote.com/).

## Как установить и запустить
1. Тестировалось на Python 3.10.0.

2. Cклонировать репозиторий.

3. Aктивировать виртуальное окружение:

```bash
python -m venv env
source ./env/bin/activate
```
4. Установить зависимости:
```bash
pip install -r requirements.txt
```
5. Зарегистрироваться в сервисе [Evernote Sandbox](https://sandbox.evernote.com/Registration.action).
6. Получить токен разработчика: https://dev.evernote.com/doc/articles/dev_tokens.php
7. Создать в сервисе Блокнот.
8. Создать заметку - шаблон. В заголовок можно добавить '{date}' - дата и '{dow}' - день недели.
Пример заголовка:
```text
Шаблонная заметка {date} {dow}
```
9. Заменить .env_example на .env:
```bash
mv .env_exapmle .env
```
10. Токен полученный в п.6 записать в .env.

Пример .env:
```text
EVERNOTE_PERSONAL_TOKEN='Ваш токен' 
INBOX_NOTEBOOK_GUID=''
JOURNAL_TEMPLATE_NOTE_GUID=''
JOURNAL_NOTEBOOK_GUID=''
```
Остальные значения пока оставить без изменений.

11. Запустить скрипт list_notebooks.py:
```bash
python list_notebooks.py
```
Пример вывода скрипта:
```bash
3f0c9949-6378-4351-a562-074747479a1d - Первый блокнот
```
* Скрипт выводит GUID всех блокнотов, которые есть в Вашей учетной записи.

12. Скопировать GUID из вывода предыдущего шага и добавить в .env.
Пример .env:
```text
EVERNOTE_PERSONAL_TOKEN='Ваш токен' 
INBOX_NOTEBOOK_GUID='3f0c9949-6378-4351-a562-074747479a1d'
JOURNAL_TEMPLATE_NOTE_GUID=''
JOURNAL_NOTEBOOK_GUID=''
```
* Из данного блокнота будут выгружаться заметки, в том числе ранее созданная шаблонная заметка.

13. Запустите скрипт dump_inbox.py:
```bash
python dump_inbox.py
```
Пример вывода скрипта:
```bash

--------- 1 ---------
Note guid: ea978253-1294-4f2c-b6f0-4c89284cfc0e


Это шаблон

```
Скопируйте вывод Note guid Вашей шаблонной заметки и вставьте в .env.
Пример .env:
```text
EVERNOTE_PERSONAL_TOKEN='Ваш токен' 
INBOX_NOTEBOOK_GUID='3f0c9949-6378-4351-a562-074747479a1d'
JOURNAL_TEMPLATE_NOTE_GUID='ea978253-1294-4f2c-b6f0-4c89284cfc0e'
JOURNAL_NOTEBOOK_GUID=''
```
* Скрипт выводит все заметки блокнота указанного в INBOX_NOTEBOOK_GUID и их GUID. Может принимать параметр -number количество выводимых заметок (по умолчанию 10)
Например:
```bash
python dump_inbox.py -number 1
```
Выведет только 1 заметку.

14. Добавьте в .env параметр JOURNAL_NOTEBOOK_GUID. Это блокнот, в котором будут создаваться заметки по шаблону в следующем шаге. Можно указать тот же блокнот, что и в INBOX_NOTEBOOK_GUID, либо создать еще 1 блокнот.

15. Запустите скрипт add_note2journal.py:
```bash
python add_note2journal.py
```
Пример вывода:
```text
Title Context is:
{
    "date": "2022-08-21",
    "dow": "воскресенье"
}
Note created: Шаблонная заметка 2022-08-21 воскресенье
Done
```
Скрипт создаст заметку с заголовком 'Шаблонная заметка 2022-08-21 воскресенье' в блокноте указанном в JOURNAL_NOTEBOOK_GUID.
Скрипт по умолчанию подставляет текущую дату и день недели, но может принимать параметр -date YYYY-MM-DD, в этом случае в заголовок попадет указанная дата.
Например:
```bash
python add_note2journal.py -date 3000-01-01
```
Вывод:
```text
Title Context is:
{
    "date": "3000-01-01",
    "dow": "среда"
}
Note created: Шаблонная заметка 3000-01-01 среда
Done
```
