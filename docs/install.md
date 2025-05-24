
## Системные требования
|                             |                                                              |
|-----------------------------|--------------------------------------------------------------|
| **ОС**                      | Windows/Linux/MacOS |
| **RAM**             | 8ГБ |
| **HDD/SSD**            | 10ГБ                                 |
| **CPU** | 4 ядра x64              |

## Подготовка к установке
Скачайте и установите python версии 3.11 с [официального сайта](https://www.python.org/downloads/)

## Установка ПО
Рекомендуется сперва создать обособленное python-окружение для избежания риска конфликтов с ранее установленными python-библиотеками.

Для создания python-окружения выполните следующую команду в консоли
```bash
python -m venv your_env
```
`your_env` - наименование окружения

Для активации созданного окружения выполните команду
```bash
your_env\Scripts\activate
```

После в активированном окружении выполните в консоли следующую команду
```bash
pip install /path/to/datapulse/wheel/directory
```
`/path/to/datapulse/wheel/directory` - путь к переданному установочному файлу **Datapulse** (в формате `.whl`)

Будут автоматически скачаны и установлены все зависимости, а также создана папка `.datapulse` в корневой директории пользователя.

## Настройка параметров Datapulse
В файле `.datapulse/config.yaml` укажите следующие параметры: 

<span id="anchor_general"></span>
### Основные


| Параметр                       | Описание                                               | Обязательно                                                       | Примечание                              |
|--------------------------------|--------------------------------------------------------|-------------------------------------------------------------------|-----------------------------------------|
| `dbt_project_path`             | Путь к папке с проектом dbt                            | <div align="center"><input type="checkbox" checked disabled></div> |                                         |
| `dbt_project_name`             | Наименование проекта dbt                               | <div align="center"><input type="checkbox" checked disabled></div> |                                         |
| `dbt_profiles_path`            | Путь к папке с файлом dbt profiles.yml                 | <div align="center"><input type="checkbox" checked disabled></div> |                                         |
| `dpulse_meta_path`             | Путь к папке, где будут храниться метаданные Datapulse | <div align="center"><input type="checkbox" checked disabled></div> | Рекомендуется указать папку `.datapulse` |
| `disable_auth`                 | Отключить аутентификацию                               | <div align="center"><input type="checkbox" disabled></div> |                                         |

<span id="anchor_datavault"></span>
### Модуль "Конструктор DataVault"

| Параметр                       | Описание                            | Обязательно                                                       |
|--------------------------------|-------------------------------------|-------------------------------------------------------------------|
| `stage_schema`                 | Схема DWH для таблиц business layer | <div align="center"><input type="checkbox" checked disabled></div> |
| `hub_schema`                   | Схема DWH для таблиц HUB            | <div align="center"><input type="checkbox" checked disabled></div> |
| `sat_schema`                   | Схема DWH для таблиц SAT            | <div align="center"><input type="checkbox" checked disabled></div> |
| `link_schema`                  | Схема DWH для таблиц LINK           | <div align="center"><input type="checkbox" checked disabled></div> |

### Модуль "Экстрактор данных"
| Параметр                       | Описание                                                               | Обязательно                                                       |
|--------------------------------|------------------------------------------------------------------------|-------------------------------------------------------------------|
| `data_source_table_schema`     | Список схем DWH, в которые можно загружать данные из источников        | <div align="center"><input type="checkbox" checked disabled></div> |
| `dlt_path`                     | Путь к папке dlt (обычно находится в корневой директории пользователя) | <div align="center"><input type="checkbox" checked disabled></div> |
| `pipeline_scripts_folder_path` | Путь к папке, в которой будут храниться файлы ETL-pipeline             | <div align="center"><input type="checkbox" checked disabled></div> |
| `dlt_staging_name`             | Префикс в наименовании для технических таблиц dlt                      | <div align="center"><input type="checkbox" checked disabled></div> |


### Модуль "Чат с DWH"
| Параметр                       | Описание                                         | Обязательно                                                       |
|--------------------------------|--------------------------------------------------|-------------------------------------------------------------------|
| `ai_model`                     | Используемая локальная модель ИИ                 | <div align="center"><input type="checkbox" checked disabled></div> |
| `schema_for_analyze`           | Список схем DWH, таблицы которых может читать ИИ | <div align="center"><input type="checkbox" checked disabled></div> |
| `chroma_path`                  | Путь к папке с ChromaDB                          | <div align="center"><input type="checkbox" checked disabled></div> |

## Работа dbt
### Создание проекта dbt
Чтобы создать проект dbt (если он еще не создан) выполните в консоли следующую команду
```bash
dbt init project_name
```
`project_name` - наименование проекта

После этого будет создана директория со всеми необходимыми файлами.

### profiles.yml
Файл `profiles.yml` предназначен для хранения параметров подключения к DWH через dbt. Обычно располагается либо в корневой папке пользователя, либо в папке проекта dbt.

Файл должен содержать следующие параметры конфигурации.
```yaml
project_name:
  outputs:
    dev:
      database: database_name
      host: host
      pass: your_password
      port: port_number (PostgreSQL default - 5432, Greenplum default - 6543)
      schema: public
      type: postgres/greenplum
      user: user_name
  target: dev
```
<div style="
    border-left: 4px solid #ffd700;
    padding: 12px 16px;
    margin: 16px 0;
    border-radius: 0 4px 4px 0;
">
⚠️ <b>Важно!</b> 
<p>Для корректной работы Datapulse параметры подключения должны быть под тегом dev, не prod (временная мера).</p>
</div>

### dbt_project.yml
Файл `dbt_project.yml` предназначен для хранения общих настроек проекта dbt. 

### Наименование схем
dbt автоматически создаем схему в DWH, если при запуске модели схемы, которая указана в настройках модели, нет.
Но при отсутствии макроса `generate_schema_name.sql` наименование схемы формируется как `defaultschema_schema`, где `defaultschema` - схема по дефолту, указанная в `profiles.yml` в параметре `schema`.
> К примеру, в PostgreSQL, если дефолтная схема - `public`, а в моделе dbt указана схема `dds`, то dbt автоматически создаст схему `public_dds`.

Исправить такое явно некорректное поведение можно с помощью вставки в папку `/macros` файл `generate_schema_name.sql` со следующим наполнением.
```sql
{% macro generate_schema_name(custom_schema_name, node) -%}

    {%- set default_schema = target.schema -%}
    {%- if custom_schema_name is none -%}

        {{ default_schema }}

    {%- else -%}

        {{ custom_schema_name | trim }}

    {%- endif -%}

{%- endmacro %}
```
<span id="anchor_artifacts"></span>
### Установка dbt_artifacts
Datapulse для мониторинга логов dbt использует пакет dbt_artifacts.

Для его установки требуется в файл `packages.yml` внести следующее.

```yaml
packages:
  - package: brooklyn-data/dbt_artifacts
```

А после выполнить в консоли команду
```bash
dbt deps
```

dbt скачает и установит указанный пакет. 
<span id="anchor_dbt_project"></span>
Далее в файле `dbt_project.yml` укажите, в какой схеме DWH вы хотите создать таблицы для хранения логов dbt.
```yaml
models:
  dbt_artifacts:
    +schema: your_schema
    +on_schema_change: <ignore|fail|append_new_columns|sync_all_columns> # поведение, при изменении структуры таблиц с типом incremental
    +compresstype: ZLIB (или другой тип компрессии) # требуется для СУБД - Greenplum, по умолчанию ZSTD

on-run-end: # запускает вставку логов в технические таблицы после окончания работы dbt модели
  - "{{ dbt_artifacts.upload_results(results) }}"
```
После запустите команду
```bash
dbt run --select dbt_artifacts
```
Будут созданы все необходимые для логов таблицы.


## Настройки аутентификации
Если требуется аутентификация и ролевой доступ, то в файле `.datapulse/config.yaml` должен быть указано следующее
```yaml
disable_auth: false
```
Также убедитесь, что в файле `..\путь к папке с datapulse\.streamlit\secrets.toml` указан админ по умолчанию
```toml
[passwords]
dpulse='admin' #логин=пароль
```
Можете сразу скорректировать и указать своего пользователя, убрав значения по умолчанию.

В папке `.datapulse/users.yaml` для всех пользователей из файла `..\путь к папке с datapulse\.streamlit\secrets.toml` должны быть указаны роли.
К примеру:
```yaml
dpulse: admin #логин: роль
```