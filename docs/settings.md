
## Пользователи
В Datapulse предусмотрено многопользовательская работа и аутентификация. 

Для задания пользователей и их ролей сперва установите параметр `disable_auth: false` в конфигурационном файле Datapulse.
Подробнее [здесь](install.md#anchor_general). При изменении параметров требуется перезагрузить Datapulse.

После на странице **Настройки** у вас появится раздел **Пользователи** (он доступен только администраторам).
![Пользователи](images/users.png)

В Datapulse предусмотрены три роли. У каждой роли есть разные привилегии.

| Модуль                  | ADMIN | DEVELOPER | ANALYST |
|-------------------------|-------|-----------|---------|
| Модели dbt              | <div align="center"><input type="checkbox" checked disabled></div> |<div align="center"><input type="checkbox" checked disabled></div>          | <div align="center"><input type="checkbox" checked disabled></div>      |
| Конструктор DataVault   | <div align="center"><input type="checkbox" checked disabled></div> |<div align="center"><input type="checkbox" checked disabled></div>          | <div align="center"><input type="checkbox" disabled></div>       |
| Экстрактор данных       | <div align="center"><input type="checkbox" checked disabled></div> |<div align="center"><input type="checkbox" checked disabled></div>          | <div align="center"><input type="checkbox"  disabled></div>      |
| Качество данных         | <div align="center"><input type="checkbox" checked disabled></div> |<div align="center"><input type="checkbox" checked disabled></div>          | <div align="center"><input type="checkbox" checked disabled></div>      |
| Чат с DWH               | <div align="center"><input type="checkbox" checked disabled></div> |<div align="center"><input type="checkbox" checked disabled></div>          | <div align="center"><input type="checkbox" checked disabled></div>      |
| Логи и статистика       | <div align="center"><input type="checkbox" checked disabled></div> |<div align="center"><input type="checkbox" checked disabled></div>          | <div align="center"><input type="checkbox" checked disabled></div>      |
| Настройка пользователей | <div align="center"><input type="checkbox" checked disabled></div> |<div align="center"><input type="checkbox" disabled></div>           | <div align="center"><input type="checkbox" disabled></div>       |