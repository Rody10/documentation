---
title: Вступ
author: Steven Spencer
contributors: Ezequiel Bruni, Ganna Zhyrnova
tested_with: 8.8, 9.2
tags:
  - lxd
  - enterprise
---

# Створення повноцінного сервера LXD

??? warning "Поточний статус LXD на Rocky Linux!"

    Майже рік тому в списку розсилки lxc-users з’явилося таке оголошення:
    
    > Компанія Canonical, творець і головний учасник проекту LXD, вирішила, що після більш ніж 8 років роботи в спільноті Linux Containers цей проект тепер краще обслуговувати безпосередньо в рамках власного набору проектів Canonical.
    
    Одним із вирішальних факторів стали відставки деяких провідних розробників LXD, які потім продовжили форк LXD на Incus, оголосивши про форк у серпні 2023 року. Версія випуску (0.1) вийшла в жовтні 2023 року, і з тих пір розробники швидко створили цю версію, випускаючи поетапно до 0.7 (березень 2024). Після відновлення версії 0.7 4 квітня 2024 року з’явилася версія довгострокової підтримки 6.0 LTS.
    
    Протягом усього процесу вважалося, що Cannonical продовжуватиме підтримувати посилання на зображення контейнерів, які надає Linux Containers, але через [зміну ліцензії](https://stgraber.org/2023/12/12/lxd-now- re-licensed-and-under-a-cla/) стало неможливо для контейнерів Linux продовжувати пропонувати образи контейнерів у LXD. Це означає, що LXD матиме зображення контейнерів, але вони не будуть зображеннями контейнерів, які ви зараз можете очікувати. Контейнери Linux продовжують розміщувати та підтримувати свої зображення, якщо ви використовуєте Incus. 
    
    У цьому документі використовується LXD, а не Incus, АЛЕ ми маємо намір переписати процедуру для Incus. Ми сподівалися, що RPM-версія Incus буде випущена в EPEL, і поки вона ще працює, вона ще не готова. Це означає, що для того, щоб переписати цю процедуру для Incus, нам потрібно зосередити наші інтереси на установці вихідного пакета та процедурі перетворення. Причина цього довгого попередження полягає в тому, що ми не хочемо, щоб ви витратили час на встановлення за допомогою цієї процедури, а потім виявили, що образи контейнерів (наприклад, Rocky Linux) недоступні в LXD. 
    
    Слідкуйте за змінами тут!

## Вступ

LXD найкраще описано на [офіційному сайті](https://documentation.ubuntu.com/lxd/en/latest/), але розглядайте його як контейнерну систему, яка надає переваги віртуальних серверів у контейнері.

Він дуже потужний, і з правильним апаратним забезпеченням і налаштуваннями його можна використовувати для запуску багатьох екземплярів сервера на одному апаратному забезпеченні. Якщо ви об’єднаєте це з сервером snapshot, у вас також буде набір контейнерів, які можна розкрутити майже негайно, якщо ваш основний сервер вийде з ладу.

(Не слід сприймати це як традиційне резервне копіювання. Вам усе ще потрібна якась звичайна система резервного копіювання, наприклад [rsnapshot](../../guides/backup/rsnapshot_backup.md).)

Крива навчання для LXD може бути трохи крутою, але ця книга спробує дати вам велику кількість знань, щоб допомогти вам розгорнути та використовувати LXD на Rocky Linux.

Для тих, хто хоче використовувати LXD як лабораторне середовище на власних ноутбуках або робочих станціях, перегляньте [Додаток A: налаштування робочої станції](30-appendix_a.md).

## Передумови та припущення

* Один сервер Rocky Linux, гарно налаштований. Вам слід розглянути окремий жорсткий диск для дискового простору ZFS (вам потрібно, якщо ви використовуєте ZFS) у робочому середовищі. І так, ми припускаємо, що це чистий сервер, а не VPS.
* Це слід вважати розширеною темою, але ми зробили все можливе, щоб зробити її максимально зрозумілою для всіх. Тим не менш, знання кількох основних речей про керування контейнерами займе у вас довгий шлях.
* Ви маєте добре володіти командним рядком на своїй машині (машинах) і вільно володіти редактором командного рядка. (У цьому прикладі ми використовуємо _vi_, але ви можете замінити його но свій улюблений редактор.)
* Ви повинні бути непривілейованим користувачем для більшості процесів LXD. Якщо не зазначено, вводьте команди LXD як непривілейований користувач. Ми припускаємо, що ви ввійшли як користувач з іменем "lxdadmin" для команд LXD. Більшість налаштувань _виконується_ від імені користувача root, доки ви не пройдете ініціалізацію LXD.
* Для ZFS переконайтеся, що безпечне завантаження UEFI НЕ ввімкнено. В іншому випадку вам доведеться підписати модуль ZFS, щоб змусити його завантажити.
* Здебільшого ми використовуємо контейнери на основі Rocky Linux.

## Короткий Огляд

* **Розділ 1: Встановлення та налаштування** стосується встановлення основного сервера. Загалом, правильний спосіб зробити LXD у виробництві — мати як основний сервер, так і сервер знімків.
* **Розділ 2: Налаштування ZFS** розповідає про налаштування та налаштування ZFS. ZFS — це диспетчер логічних томів і файлова система з відкритим вихідним кодом, створена Sun Microsystems спочатку для своєї операційної системи Solaris.
* **Розділ 3: Ініціалізація LXD і налаштування користувача** стосується базової ініціалізації та параметрів, а також налаштування непривілейованого користувача, якого ви використовуватимете протягом більшої частини решти процесу
* **Розділ 4: Налаштування брандмауера** стосується параметрів налаштування як `iptables`, так і `firewalld`, але ми рекомендуємо використовувати `firewalld`для всіх поточних версій Rocky Linux.
* **Розділ 5: Налаштування образів і керування ними** описує процес встановлення образів ОС до контейнера та їх налаштування.
* **Розділ 6: Профілі** розповідає про додавання профілів і їх застосування до контейнерів, а також охоплює macvlan і його важливість для IP-адресації у вашій LAN або WAN
* **Розділ 7: Параметри конфігурації контейнера** коротко описує деякі основні параметри конфігурації для контейнерів і пропонує деякі плюси та мінуси для зміни параметрів конфігурації.
* **Розділ 8: Знімки контейнерів** детально описує процес створення знімків для контейнерів на основному сервері.
* **Розділ 9. Сервер Snapshot** розповідає про налаштування та конфігурацію сервера snapshot, а також про те, як створити симбіотичний зв’язок між основним і сервером snapshot.
* **Розділ 10: Автоматизація Snapshots** розповідає про автоматизацію створення snapshot і заповнення сервера snapshot новими snapshots.
* **Додаток A: налаштування робочої станції** технічно не є частиною документів робочого сервера, але пропонує рішення для людей, які хочуть у простий спосіб створити лабораторію контейнерів LXD на своїх особистих ноутбуках або робочіх станціях.

## Висновки

Ви можете використовувати ці розділи для ефективного налаштування основного сервера LXD на рівні підприємства та знімків. У процесі ви дізнаєтесь багато про LXD, і ми *розглянемо* багато функцій. Просто майте на увазі, що ще багато чого потрібно дізнатися, і розглядайте ці документи як відправну точку.

Найбільша перевага LXD полягає в тому, що його економічно використовувати на сервері, він дозволяє швидко інсталювати ОС, його можна використовувати для кількох автономних серверів додатків, що працюють на одному «голому металі», і все це належним чином використовує це обладнання для максимального використання.
