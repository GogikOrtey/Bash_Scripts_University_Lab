## Задание 2: bash-скрипты

### Написать bash-скрипт с интерактивным меню, который может выполнять следующие функции: 

1. Выводит информацию о текущем пользователе и текущей версии ОС 
2. Выводит список всех файлов (в том числе скрытых) с правами доступа домашней директории текущего пользователя (указать в скрипте просто название директории не подойдёт используйте переменные окружения). 
3. Создаёт файл с заданными правами в заданной директории. Скрипт отдельно просит ввести имя файла и права к файлу.
4. Отправляет n количество пакетов с помощью ping на заданный ресурс. Необходимо реализовать проверку на пустую строку. 

---

### Все команды, которые я вводил, по порядку:

Создание файла:

```bash
mkdir bashScr 
cd bashScr 
touch scr_01
nano scr_01
chmod 777 scr_01
```

Где лежит файл:

```bash
cd /home/kali/bashScr
```

Запуск файла:

```
./scr_01
```

---

## bash-скрипт

```bash
#!/bin/bash

# Функции главного меню
show_menu() {
    echo "1. Выводит информацию о текущем пользователе и текущей версии ОС."
    echo "2. Выводит список всех файлов (в том числе скрытых) с правами доступа домашней директории текущего пользователя."
    echo "3. Создает файл с заданными правами в заданной директории."
    echo "4. Отправляет n количество пакетов с помощью ping на заданный ресурс."
    echo "5. Выход"
}

# 1. Вывод информации о пользователе
user_info() {
    echo "Текущий пользователь: $USER"
    echo "Текущая версия ОС: $(lsb_release -d | awk '{print $2,$3,$4}')"
}

# 2. Список всех файлов в домашней директории
display_files() {
    echo "Список всех файлов (в том числе скрытых) с правами доступа домашней директории текущего пользователя:"
    ls -la $HOME
}

# 3. Создание файла с заданными правами
create_file() {
    echo "Введите имя файла:"
    read filename
    echo "Введите права к файлу:"
    read permissions
    echo "Введите директорию, в которой нужно создать файл:"
    read directory
    touch $directory/$filename
    chmod $permissions $directory/$filename
    echo "Файл $filename успешно создан в директории $directory с правами $permissions."
}

# 4. Отправка ping на указанный адрес
send_ping() {
    echo "Введите адрес ресурса:"
    read address
    echo "Введите количество пакетов:"
    read count
    if [ -z "$address" ]
    then
        echo "Адрес ресурса не может быть пустым."
    else
        ping -c $count $address
    fi
}

# Основная процедура выбора функции
read_choice() {
    local choice
    read -p "Введите номер функции, которую нужно выполнить: " choice
    case $choice in
        1) user_info ;;
        2) display_files ;;
        3) create_file ;;
        4) send_ping ;;
        5) exit 0 ;;
        *) echo "Неверный выбор. Попробуйте еще раз." && sleep 2 ;;
    esac
}

# Основная петля
while true
do
    show_menu
    read_choice
done

```

---

### Описание используемых ключевых слов:

- `echo`: Это команда, которая выводит строку на стандартный вывод или файл. Вы можете использовать её для отображения текста, переменных или результатов других команд.
- `$USER` и `$HOME`: Это переменные окружения, которые содержат имя пользователя и домашнюю директорию соответственно. Другие часто используемые переменные окружения включают `$PATH` (путь к исполняемым файлам), `$PWD` (текущий рабочий каталог), и другие
- `read`: Это команда, которая читает одну или несколько строк из стандартного ввода и присваивает их переменным.
- `touch`: Это команда, которая создаёт новый пустой файл или обновляет время последнего доступа к файлу.
- `chmod`: Это команда, которая изменяет права доступа к файлу или каталогу.
- `local`: Это ключевое слово, которое используется в функциях для объявления локальных переменных.
- `sleep`: Это команда, которая приостанавливает выполнение скрипта на заданное количество секунд.

---

### Вот пример ввода для шага 3:

```
Введите имя файла: test.txt
Введите права к файлу: 777
Введите директорию, в которой нужно создать файл: /tmp
```

Права 777 - всем всё доступно

Права 644 - владелец может читать и редактировать, а все остальные - только читать

---

### Пример адреса для шага 4:

```
8.8.8.8
или 
1.1.1.1
```

Не вводите слишком большое количество запросов (не больше 10)
