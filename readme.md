## **MySQL (InnoDB) 5.7**
| **Problem**             | **READ UNCOMMITTED** | **READ COMMITTED** | **REPEATABLE READ** | **SERIALIZABLE** |
|:------------------------|:--------------------:|:------------------:|:-------------------:|:----------------:|
| **Dirty Read**          |          ❌           |         ✅          |          ✅          |        ✅         |
| **Non-Repeatable Read** |          ❌           |         ❌          |          ✅          |        ✅         |
| **Phantom Read**        |          ❌           |         ❌          |          ❌          |        ✅         |

* Рівень ізоляції SERIALIZABLE:
  * транзакції 2 чекає поки закінчиться транзакція 1, щоб вибрати рядок. Бо операція update заблокувала цей рядок.
  * транзакції 2 не може порахувати кількість рядків в таблиці поки вставка даних в таблицю не закінчиться в транзакції 1. 

`Default isolation level: REPEATABLE READ`

## **MariaDB 10.11**
| **Problem**             | **READ UNCOMMITTED** | **READ COMMITTED** | **REPEATABLE READ** | **SERIALIZABLE** |
|:------------------------|:--------------------:|:------------------:|:-------------------:|:----------------:|
| **Dirty Read**          |          ❌           |         ✅          |          ✅          |        ✅         |
| **Non-Repeatable Read** |          ❌           |         ❌          |          ✅          |        ✅         |
| **Phantom Read**        |          ❌           |         ❌          |          ✅          |        ✅         |
  
* Phantom Read немає у Repeatable Read, це відрізняється від MySQL.
* Якщо транзакція 1 вибрала рядок, то транзакція 2 також може вибрати цей рядок, але не може його оновити поки транзакція 1 не закінчиться.

`Default isolation level: REPEATABLE READ`

## **PostgreSQL 15**
| **Problem**             | **READ UNCOMMITTED** | **READ COMMITTED** | **REPEATABLE READ** | **SERIALIZABLE** |
|:------------------------|:--------------------:|:------------------:|:-------------------:|:----------------:|
| **Dirty Read**          |          ✅           |         ✅          |          ✅          |        ✅         |
| **Non-Repeatable Read** |          ❌           |         ❌          |          ✅          |        ✅         |
| **Phantom Read**        |          ❌           |         ❌          |          ✅          |        ✅         |


* READ UNCOMMITED та READ COMMITED однаково
* Як працює SERIALIZABLE порівняно з MySQL:
  * дозволяє читати рядок транзакції 2, навіть якщо транзакція 1 наразі оновлює його.
  * дозволяє вибрати кількість рядків в таблиці поки вставка даних в таблицю не закінчиться в транзакції 1.

`Default isolation level: READ COMMITTED`

## **Vertica 24.1.0-0**
| **Problem**             | **READ UNCOMMITTED** | **READ COMMITTED** | **REPEATABLE READ** | **SERIALIZABLE** |
|:------------------------|:--------------------:|:------------------:|:-------------------:|:----------------:|
| **Dirty Read**          |          -           |         ✅          |          -          |        ✅         |
| **Non-Repeatable Read** |          -           |         ❌          |          -          |        ✅         |
| **Phantom Read**        |          -           |         ❌          |          -          |        ✅         |

* У Vertica немає рівня ізоляції READ UNCOMMITTED та REPEATABLE READ: 
  * при спробі встановити рівень ізоляції READ UNCOMMITTED - автоматично встановлюється рівень ізоляції READ COMMITTED
  * при спробі встановити рівень ізоляції REPEATABLE READ - автоматично встановлюється рівень ізоляції SERIALIZABLE

* Serilizable - не дозволяє транзакції 1 оновити рядок, якщо транзакція 2 до цього його читала, прям як в MariaDB.
* Якщо зробити count(*) в транзакції 1, то транзакція 2 не зможе вставити новий рядок/або оновити існуючий поки транзакція 1 не закінчиться.

`Default isolation level: READ COMMITTED`