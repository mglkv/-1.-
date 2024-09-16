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
![image]
