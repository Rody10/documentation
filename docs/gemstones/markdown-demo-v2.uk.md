---
title: Markdown Demo
author: Steven Spencer
contributors: Wale Soyinka, Tony Guntharp
tested_with: 8.5
tags:
  - зразок
  - crowdin
  - markdown
---

# Огляд

## Необхідні умови:

- Використання [Markdown](https://daringfireball.net/projects/markdown).
- Знання Markdown.

Цей посібник демонструє популярні теги Markdown, які використовуються на [https://docs.rockylinux.org](https://docs.rockylinux.org), і містить тег admonitions, який не є частиною стандартних тегів Markdown.

## Демо

> Це приклад цитати. Гарно відформатований.

Іноді ви побачите _це_.

Як щодо маленького **bold face**

У більшості випадків це прямий текст.

Іноді потрібно показати `команду`

Або кілька команд:

```bash
dnf install my_stapler
dnf update my_pencil
dnf remove my_notepad
systemctl enable my_stapler
```

В інших випадках вам потрібні марковані або пронумеровані списки:

- Думаю, у вас є мій степлер
- Якщо подумати, я навіть не можу знайти свій компас
- Якщо ні, будь ласка, принаймні поверніть мені олівець
- Мені це точно потрібно

1. Я не знав, що тобі це потрібно
2. Ось твій зламаний олівець
3. Заточувати його марно

І вам може знадобитися попередження:

### Попередження

Застереження є чудовим вибором для включення додаткового вмісту без значного переривання потоку документів. Матеріал для MkDocs надає кілька типів попереджень і дозволяє включати та вкладати довільний вміст.

!!! TIP "ПІДКАЗКА"

    Олівці та степлери - це стара школа.

#### Використання

Застереження мають простий синтаксис: блок починається з `!!!`, за яким йде ключове слово, яке використовується як кваліфікатор типу. Вміст блоку наводиться в наступному рядку з відступом на чотири інтервали:

!!! note "Примітка"

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

#### Зміна назви

За замовчуванням заголовок дорівнюватиме кваліфікатору типу в регістрі заголовка. Однак його можна змінити, додавши рядок у лапках, що містить дійсну позначку (включно з посиланнями, форматуванням тощо) після кваліфікатора типу:

!!! note "Phasellus posuere in sem ut cursus"

    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod
    nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor
    massa, nec semper lorem quam in massa.

Часто, коли команда має кілька параметрів або вам потрібно вказати особливості, ви можете використовувати таблицю для визначення речей:

| Інструмент | Використання                            | Додаткова інформація                                                                  |
| ---------- | --------------------------------------- | ------------------------------------------------------------------------------------- |
| олівець    | написання або друк                      | часто замінюють пером                                                                 |
| перо       | написання або друк                      | часто замінюють стилусом                                                              |
| стилус     | запис або друк на електронному пристрої | іноді замінюється клавіатурою                                                         |
| клавіатура | запис або друк на електронному пристрої | жодного разу не замінювали - використовували до повного наповнення харчовими крихтами |
| блокнот    | робити нотатки                          | замінити інколи несправну пам’ять                                                     |
