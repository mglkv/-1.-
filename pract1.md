# Практическое занятие 1
## Задача 1: Вывести отсортированный в алфавитном порядке список имен пользователей в файле passwd

### Код
```
grep '^[^:]*' /etc/passwd | cut -d: -f1 | sort
```
#### Вывод
![image](https://github.com/mglkv/-1.-/blob/main/pract1/task1.png)

## Задача 2: Вывести данные /etc/protocols в отформатированном и отсортированном порядке для 5 наибольших портов

### Код
```
cat /etc/protocols | awk '{print $2, $1}' | sort -nr | head -n 5
```
#### Вывод
![image](https://github.com/mglkv/-1.-/blob/main/pract1/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-16%20%D0%B2%2003.07.17.png)

## Задача 3: Написать программу banner средствами bash для вывода текстов 

### Код
```
#!/bin/bash
if [ "$#" -ne 1 ];  then
        echo "Использование: $0 \"Ваш текст\""
        exit -1
fi

text="$1"
length=${#text}

frame_length=$((length + 2))

printf '+%s+\n' "$(printf '%*s' "$frame_length" '' | tr ' ' '-')"
printf '| %s |\n' "$text"
printf '+%s+\n' "$(printf '%*s' "$frame_length" '' | tr ' ' '-')"
```
#### Вывод
![image]

## Задача 4: Написать программу для вывода всех идентификаторов 

### Код
```
#!/bin/bash

if [ "$#" -ne 1 ]; then
	echo "Используйте: $0 имя_файла"
	exit 1
fi

file="$1"

grep -oE '\b[a-zA-Z_][a-zA-Z0-9_]*\b' "$file" | sort -u
```
#### Вывод
![image](https://github.com/mglkv/-1.-/blob/main/pract1/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-16%20%D0%B2%2004.21.00.png)

## Задача 5: Написать программу для реализации пользовательской команды

### Код
```
#!/bin/bash

if [ "$#" -ne 1 ]; then
	echo "Используйте: $0 имя команды."
	exit 1
fi

command_name="$1"
command_path="./$command_name"

if [ ! -f "$command_path" ]; then
	echo "Ошибка: файл '$command_path' не найден."
	exit 1
fi

sudo chmod +x "$command_name"
sudo cp "$command_path" /usr/local/bin/

if [ $? -ne 0 ]; then
	echo "Ошибка: не удалось скопировать файл."
	exit 1
fi

sudo chmod 775 /usr/local/bin/"$command_name"

if [ $? -ne 0 ]; then
	echo "Ошибка: не удалось установить права для '$command_name'."
	exit 1
fi

echo "Команда '$command_name' успешно зарегестрирована."
```
#### Вывод
![image](https://github.com/mglkv/-1.-/blob/main/pract1/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-16%20%D0%B2%2004.58.43.png)

## Задача 6: Написать программу для проверки наличия комментария в первой строке файлов с расширением c,js и py

### Код
```
# -*- coding: utf-8 -*-
import os
import sys

def check_comment_in_first_line(file_path):
    with open(file_path, 'r') as file:
        first_line = file.readline().strip()
        if first_line.startswith('#') or first_line.startswith('//') or first_line.startswith('/*'):
            return True
    return False

def check_files_in_directory(directory):
    extensions = ('.c', '.js', '.py')
    for filename in os.listdir(directory):
        if filename.endswith(extensions):
            file_path = os.path.join(directory, filename)
            if check_comment_in_first_line(file_path):
                print(f"Комментарий найден в файле: {filename}")
            else:
                print(f"Комментарий не найден в файле: {filename}")

if name == 'main':
    if len(sys.argv) != 2:
        print("Использование: python3 main.py ~/Desktop/питон")
        sys.exit(1)

    directory = sys.argv[1]
    if not os.path.isdir(directory):
        print(f"{directory} не является директорией.")
        sys.exit(1)

    check_files_in_directory(directory)
```
#### Вывод
![image](https://github.com/mglkv/-1.-/blob/main/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-16%20%D0%B2%2005%3A10%3A12.png)

## Задача 7: Написать программу для нахождения файлов-дубликатов 

### Код
```
#!/bin/bash

if [[ $# -ne 1 ]]; then
	echo "Использование: $0 /путь/к/каталогу."
	exit 1
fi

directory=$1

if [[ ! -d $directory ]]; then
	echo "Ошибка: каталог $directory не найден."
	exit 1
fi

declare -A file_hashes

while IFS= read -r -d '' file; do
	hash=$(sha256sum "$file" | awk '{ print $1 }')
	file_hashes["$hash"]+="file"$'\n'
done < <(find "$directory" -type f -print0)

found_duplicates=false

for hash in "${!file_hashes[@]}"; do
	files="${file_hashes[$hash]}"
	files_count=$(echo -e "files" | wc -l)


	if [[ $files_count -gt 1 ]]; then
		found_duplicates=true
		echo "Найдены дубликаты:"
		echo -e "$files"
	fi
done

if ! $found_duplicates; then
	echo "Дубликатов не найдено."
	exit 1
fi
```
#### Вывод
![image](https://github.com/mglkv/-1.-/blob/main/pract1/image.png)

## Задача 8: Написать программу, которая находит все файлы в данном каталоге с расширеннием, указанным в качестве аргумента и архивирует все эти файлы в архив tar

### Код
```

```
#### Вывод
![image]()

## Задача 9: Написать программу, которая заменяет в файле последовательности из 4 пробелов на символ табуляции. Входной и выходной файл задаются аргументами

### Код
```
#!/bin/bash

if [ "$#" -ne 2 ]; then
	echo "Использование: $0 <входной_файл> <выходной_файл>"
	exit 1
fi

input_file="$1"
output_file="$2"

if [ ! -f "$input_file" ]; then
	echo "Ошибка: '$input_file' не найден."
	exit 1
fi

sed 's/    /\t/g' "$input_file" > "$output_file"

echo "Заменено в файле: '$output_file'"
```
#### Вывод
![image](https://github.com/mglkv/-1.-/blob/main/pract1/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-16%20%D0%B2%2006.37.30.png)

## Задача 10: Написать программу, которая выводит названия всех пустых текстовых файлов в указанной директории

### Код
```
#!/bin/bash

if [ "$#" -ne 1 ]; then
	echo "Использование: $0 /путь/к/каталогу."
	exit 1
fi

directory="$1"

if [ ! -d "$directory" ]; then
	echo: "Ошибка: указанный каталог '$directory' не существует."
	exit 1
fi

find "$directory" -type f -name "*.txt" -empty -exec basename {} \;
```
#### Вывод
![image](https://github.com/mglkv/-1.-/blob/main/pract1/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202024-09-16%20%D0%B2%2006.51.06.png)
