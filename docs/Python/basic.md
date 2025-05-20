---
title: 基本知識
tags:
  - python
sidebar_position: 1
---

REF: [Python 6 小時初學者課程 (2023)](https://www.youtube.com/watch?v=lvH4-4iYjgs)

## util

### 說明

如果對某方法或物件不了解，可以透過 help 函式讀取介紹

例子，介紹字串方法

```python
help(str)

class str(object)
 |  str(object='') -> str
 |  str(bytes_or_buffer[, encoding[, errors]]) -> str
 |
 |  Create a new string object from the given object. If encoding or
 |  errors is specified, then the object must expose a data buffer
 |  that will be decoded using the given encoding and error handler.
 |  Otherwise, returns the result of object.__str__() (if defined)
 |  or repr(object).
 |  encoding defaults to 'utf-8'.
 |  errors defaults to 'strict'.
```

### print

```python
x = 5
y = 6
print(x+y) # 11

# Template literals
is_online = True
print(f"is online {is_online}")

# end: 列印後輸出的值，預設是換行符號，這邊改空格，所以輸出 1 2 3 ... 9
for x in range(1,10):
  print(x, end=" ")
```

### 取得型別

```python
x = 5
y = 6
print(type(x+y)) # <class 'int'>
```

### 取得使用者輸入

```python
name = input('enter ur name ')
print(f'ur name is {name}')

price = float(input('enter price'))
print(f'value is {name}')
```

### 等待秒數

```python
import time
# 相當於同步等待 200 ms
time.sleep(0.2)
```

### 隨機數字

```python
import random

help(random)

# 指定隨機數字範圍 例子 1 ~ 10
print(random.randint(1, 10)) # 9

# 列表中隨機一個元素
options = ['A', 'B', 'C']
print(random.choice(options)) # A

# 列表亂數排列
cards = ["J", "K", "Q", "1", "2"]
print(random.shuffle(cards)) # None
print(cards) # ['1', 'K', 'Q', '2', 'J']
```

猜數字

```python
import random

num_to_guess = random.randint(1, 10)

while True:
  print(f"要猜的數字{num_to_guess} ")
  guess = int(input('輸入數字'))
  if (guess == num_to_guess):
    print('正確')
    break
  elif (guess > num_to_guess):
    print('太大')
  else:
    print('太小')

```

## 變數 variable

```python
age = 25
# ❌ 字串不能跟數字合併
print('my age' + age)
# ✔️
print('my age' + str(age))
```

## 基本型別

```python
# 整數 integer
age = 30
# 浮點數 float
gpa = 3.3
# 字串 string
name = 'Peter'
# boolean
is_online = True
```

### 顯式型別轉換

```python
name = 'John'
age = 21
gpa = 1.9
student = False

age = float(age)
print(age, type(age)) # <class 'float'>

gpa = int(gpa)
print(gpa, type(gpa)) #<class 'int'>

student = str(student)
print(student, type(student)) #<class 'str'>
```

### 隱式型別轉換

不用特定聲明就能轉換型別

```python
age = 21
gpa = 1.9
print(age, type(age + gpa)) # <class 'float'>
```

### 基本運算

#### 加減乘除

共用同樣的模式

```python
num = 1

num = num + 1
print(num) # 2

num += 1
print(num) # 3
```

指數也是

```python
num = 3
result = num ** 2
print(result) # 9

result **= 2
print(result) # 81
```

餘數

```python
# mod

# 10 mod 3 ➡️ 3 * 3 + 1
print(10 % 3)

# 11 mod 3 ➡️ 3 * 3 + 2
print(11 % 3)

# 12 mod 3 ➡️ 3 * 4 + 0
print(12 % 3)
```

```python
import math

print(math.floor(9.5)) # 9
print(math.ceil(9.5)) # 10
print(math.pi) # 3.141592653589793

# 有些方法不一定是從 math module 呼叫
print(max(9.5, 9, 100, 999)) # 999
```

一個 / 符號 代表用浮點數的方式 2 除以 6
兩個 / 符號 代表用整數的方式 2 除以 6

```python
# 0
print(2 // 6)
# 0.3333333333333333
print(2 / 6)
```

取總和

```python
number = [60, 80]
print(sum(number)) # 140
```

## 條件語句

```python
value = int(input('enter number '))

if value > 100:
  print('value ')
  print('大於 100')
elif age > 50:
  print('value 大於 50')
elif age > 0:
  print('value 大於 0')
else :
  print('value 不可以小於 0')
  exit()
```

### and or not

```python
temp = int(input('輸入溫度: '))

if temp > 24 and temp < 30:
  print('適宜溫度')
else :
  print('不適宜溫度')
```

```python

```

## 字串方法

### 基本方法

```python
# 尋找
string = 'dog want a cat'
print('cat' in string) # True
print(string.find('cat')) # 11

box = 'box'
# 第一字母大寫
print(box.capitalize()) # Box
# 大寫
a1 = box.upper()
print(a1, box) # box box
# 小寫
a2 = box.lower()
print(a2, box) # BOX box

# 數量
string_to_find = 'cat search a cat which can say cat'
print(string_to_find.count('cat')) # 3

# 取代
print(string_to_find.replace('cat', 'dog')) # dog search a dog which can say dog
print(string_to_find) # cat search a cat which can say cat

# 純英文判斷
booleanString1 = 'this is a cat'
print(booleanString1.isalpha()) # False
booleanString2 = 'cat'
print(booleanString2.isalpha()) # True
```

### 字串索引

```python
numbers = '0123456789'

print(numbers[0]) # 0
print(numbers[4]) # 4
print(numbers[-1]) # 9

# 直接拋錯停止執行
print(numbers[100]) # IndexError: string index out of range

# 取得 index 4 ~ 8 的字串，不包含 8
print(numbers[4:8]) # 4567
# 取得 index 0 ~ 8 的字串，不包含 8
print(numbers[:8]) # 01234567
# 取得 index 8 之後的字串，包含 8
print(numbers[8:]) # 89

mail = 'example@yahoo.com.tw'
index = mail.index('@')
print(mail[(index+1):]) # yahoo.com.tw
```

### f-string 格式化字串

```python
# 字串顯示數值
p1 = 3.3321
p2 = -7.7
p3 = 15

print(f"p1 value {p1}\n"
      f"p2 value {p2}\n"
      f"p3 value {p3}")

# 指定位數
p4 = 3.123456
p5 = -77
print(f"p4 value {p4:.2f}\n" # p4 value 3.12
      f"p4 value {p4:.4f}\n" # p4 value 3.1235
      f"p4 value {p4:10.4f}\n" # p4 value     3.1235 ⬅️ 文字長度 10
      f"p5 value {p5:.2f}")  # p5 value -77.00

# 顯示 +、- 符號
p6 = 10
print(f'p6 value: {p6:+}') # p6 value: +10
p7 = -10
print(f'p6 value: {p7:+}') # p7 value: -10

# 文字預設長度
p8 = -77
print(f"p8 value {p8:.2f}\n"
      f"p8 value {p8:10.2f}\n"
      f"p8 value {p8:12.2f}\n"
      f"p8 value {p8:14.2f}\n"
      f"p8 value {p8:16.2f}\n"
      )

# p8 value -77.00
# p8 value     -77.00
# p8 value       -77.00
# p8 value         -77.00
# p8 value           -77.00


# 文字靠左右中
p8 = -77
print(
      f"p8 value {p8:<16.2f}\n"
      f"p8 value {p8:>16.2f}\n"
      f"p8 value {p8:^16.2f}\n"
      f"p8 value {p8:^+16.2f}\n"
      f"p8 value {p8:+^16.2f}\n"
      )

# p8 value -77.00
# p8 value           -77.00
# p8 value      -77.00
# p8 value      -77.00
# p8 value +++++-77.00+++++
```

## 迴圈

### while

```python
name = input('enter ur name ')

while name == "":
  print('name is required')
  name = input('enter ur name ')

print(f"hi {name}")

# 無窮迴圈
while True:
  isCancel = input('cancel loop?')
  if (isCancel.lower() == "yes")
    break
```

### for

for 變數 in 迭代物:

```python
# 輸出 0 ~ 10 (不包含 11)
for x in range(0, 11):
  print('x value', x)

# 輸出 10 ~ 0 (不包含 11)
for x in reversed(range(0, 11)):
  print('x value', x)

# break & continue
for x in reversed(range(0, 11)):
  if x == 1:
    continue
  elif x == 3:
    break
  else:
    print(x)

# 迭代字典 dictionary
dict = {'a':1, 'b': True, 'c': 'name'}

# 輸出 a b c
for x in dict:
  print(x)

# 無效寫法 直接拋錯
for x, y in dict:
  print(x,y)

for key, value in dict.items():
  print(key, value)

# 輸出
# a 1
# b True
# c name

rows = 5
cols = 6

for i in range(rows):
    for j in range(cols):
        print('@', end=' ')
    print()

# 輸出
# @ @ @ @ @ @
# @ @ @ @ @ @
# @ @ @ @ @ @
# @ @ @ @ @ @
# @ @ @ @ @ @


# range -1 代表反向迭代
my_time = int(input('輸入秒數: '))
for x in range(my_time, 0, -1):
    print(x)

# 範例 倒數秒數
import time

my_time = int(input('輸入秒數: '))

for x in range(my_time, 0, -1):
    seconds = x % 60
    minutes = (x // 60) % 60
    print(f"{minutes:02}:{seconds:02}", end="\r")
    time.sleep(1)
print("時間到！")
```

## list, sets, tuple

### list

```python
fruits = ['apple', 'banana', 'cherry']

# 001 迭代
# apple
# banana
# cherry
for f in fruits:
    print(f)

# 002 插入
# ['apple', 'banana', 'cherry', 'orange']
fruits.append('orange')
print(fruits)

# 003 取得某值重複數量
# 2
fruits.append('orange')
print(fruits.count('orange'))

# 004 移除
# ['apple', 'cherry', 'orange', 'orange']
fruits.remove('banana')
print(fruits)

# 005 取得索引
# 1
print(fruits.index('cherry'))

# 006 反轉排序
# ['orange', 'orange', 'cherry', 'apple']
fruits.reverse()
print(fruits)
```

### sets

```python
# Set 無順序、數值只能唯一

fruits_set = {"apple", "banana", "cherry"}

# 001 新增元素
fruits_set.add("orange")

# 002 迭代
for fruit in fruits_set:
    print(fruit, end=" ")

# 003 Set 是否有該元素
if "apple" in fruits_set:
    print("\napple is in the set")
```

### tuple

```python
# tuple 有順序、數值可以重複，但方法只有 index 跟 count

fruits = ('apple', 'banana', 'cherry', 'banana')

print(fruits.index('banana'))
print(fruits.count('banana'))
```

### 遍歷

```python
fruits = ('apple', 'banana', 'cherry', 'banana')

for index, item in enumerate(fruits):
    print(index, item)

# 0 apple
# 1 banana
# 2 cherry
# 3 banana
```

## dictionary

key & value 組成；有順序、可改變、不允許重複 key

```python
capital = {
  "Tokyo": "Japan",
  "Seoul": "South Korea",
  "Beijing": "China",
}

# 001 get 特定值
print(capital["Tokyo"])
# Japan
print(capital.get("Seoul"))
# South Korea
notFound = capital.get("Unknown")
if notFound is None:
  print(notFound)
# None

# 002 update 更新
capital.update({"Taiwan": "Taipei"})
print(capital)
# {'Tokyo': 'Japan', 'Seoul': 'South Korea', 'Beijing': 'China', 'Taiwan': 'Taipei'}

# 003 pop 刪除
result = capital.pop("Taiwan")
print(result)
# Taipei
print(capital)
# {'Tokyo': 'Japan', 'Seoul': 'South Korea', 'Beijing': 'China'}

# 004 values 所有值
print(capital.values())
# dict_values(['Japan', 'South Korea', 'China'])

# 005 所有鍵值
print(capital.items())
# dict_items([('Tokyo', 'Japan'), ('Seoul', 'South Korea'), ('Beijing', 'China')])

# 006 loop
for key, value in capital.items():
    print(f"{key} is the capital of {value}")
```

## function

```python
def say_hello(name):
    print(f"Hello, {name}")

say_hello() # 這樣會拋錯


# 預設引數 (不能是第一個參數)
def greet(name, msg="Hello"):
    print(f"{msg}, {name}")

greet("Alice") # Hello, Alice
greet("Tom", "hi") # hi, Tom
```

### 關鍵字參數

帶多參數函式、提升可讀性、更少限制傳遞參數(?)

```python
def get_phone(country_code, area_code, first, second):
    """
    This function takes a country code, area code, first number, and second number as input
    and returns a formatted phone number string.
    """
    return f"+{country_code} ({area_code}) {first}-{second}"

# +886 (02) 88-6
print(get_phone(country_code="886", area_code="02", first="88", second="6"))
```

### args、kwargs

- args:
  - arguments 任意數量的參數
  - 用 \* 符號將參數以 tuple 方式表示

```python
def add(*args):
    # 這裡的 args 型別是 tuple
    print(type(args)) # <class 'tuple'>
    total = 0
    for arg in args:
        print(f"arg: {arg}")
        total += arg
    return total

print(add(1, 2, 3, 4, 5))  # Output: 15
```

相當於 js

```javascript
function add(...args) {
  return args.reduce((total, arg) => (total += arg), 0)
}
```

- kwargs
  - 關鍵字 + args (keyword args)
  - 用 \*\* 符號將參數以 dictionary 方式表示

```python
def print_info(**kwargs):
    print(type(kwargs)) # <class 'dict'>
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="John", age=30, city="New York")

# name: John
# age: 30
# city: New York
```

## 模組

```python
# 001 標準內建模組
import math

# 說明該模組功能
help(math)

# 002 別名
import math as m

print(m.floor(20.6))

# 003 單獨引用
from math import pi, radians
print(math.pi) # 3.141592653589793
print(pi) # 3.141592653589793
```

### 引用自定義模組

```python title="./index.py"
pi = '3.141592653589793'

def square(x):
    return x ** 2
```

```python title="main.py"
# 這裡的 index 對應 index.py
import index as my_module

print(my_module.pi) # 3.141592653589793
print(my_module.square(4)) # 16
```

## 異常處理

```python
x = 0
y = 1
print(y/x)
```

```console
Traceback (most recent call last):
  File "F:\programs\python\playground\main.py", line 10, in <module>
    print(y/x)
          ~^~
ZeroDivisionError: division by zero
```

處理錯誤

```python
try:
  x = 0
  y = 1
  print(y/x)
except (ValueError, ZeroDivisionError):
  print("特定錯誤")
except Exception as e:
  print(f"所有錯誤都會在這裡執行，除了ValueError, ZeroDivisionError: {e}")
except:
  print("所有錯誤都會在這裡執行")
else:
  print("只有當 try 區塊內的程式碼完全成功、且沒有拋出任何例外時才會執行。")
finally:
  print("不論是否發生例外，一定會執行。")
```

```python
# 001 可以另外針對各類型錯誤處理
try:
  x = 0
  y = 1
  print(y/x)
except ZeroDivisionError:
  print("Error: Division by zero is not allowed.")
except ValueError:
  print("Value error.")

# 002 或者將所有錯誤整合一起
try:
  x = 0
  y = 1
  print(y/x)
except (ValueError, ZeroDivisionError):
  print("Error")
```

## 檔案操作

### 偵測

```python
import os

# 跳脫字元寫法
path_1 = "F:\\programs\\python\\playground\\index.py"
# 原始字串寫法
path_2 = r"F:\programs\python\playground\index.py"

def isExist(path):
    if os.path.exists(path):
        print(f"{path} exists")
        if os.path.isfile(path):
            print(f"{path} is a file")
        elif os.path.isdir(path):
            print(f"{path} is a directory")
        else:
            print(f"{path} is neither a file nor a directory")
    else:
        print(f"{path} does not exist")

isExist(path_1)
isExist(path_2)
```

### 讀取

```python
path_2 = r"F:\programs\python\playground\index.py"

with open(path_2) as file:
  print(file.read())

with open(path_2, 'r') as file:
  print(file.read())
```

### 寫入

```python
path = r"F:\programs\python\playground\note.txt"

# 新增/覆蓋
with open(path, 'w') as file:
  file.write("this\nis a message")

# 編輯
with open(path, 'a') as file:
  file.write("\nNew Message")
```

### 複製

```python
import shutil

"""
複製檔案
只有文件的內容會被複製
其他資訊例如該文件何時被創建、檔案大小等不會被複製
"""
shutil.copyfile('source_file_path.txt', 'copy_file_path.txt')

"""
複製檔案
只有文件的內容會被複製
其他資訊例如該文件何時被創建、檔案大小等不會被複製
可以用於目錄
"""
shutil.copy('source_file_path.txt', 'copy_file_path.txt')

"""
複製檔案
文件的內容、資訊 (例如權限) 會被複製
"""
shutil.copy2('source_file_path.txt', 'copy_file_path.txt')
```

### 刪除

```python
import os
import shutil

# 刪除檔案
os.remove(r"note.txt")

# 刪除空資料夾
os.rmdir(r"dir_path")

# 刪除資料家及其內容
shutil.rmtree(r"dir_path")

# 移動檔案到資源回收桶 (外掛套件)
# pip install send2trash
import send2trash
send2trash.send2trash(r"note.txt")
```

## 套件安裝

方法一：使用 requirements.txt

```txt title="requirements.txt"
send2trash==1.8.2
requests>=2.28.0
```

```bash
pip install -r requirements.txt
```

方法二：使用 pip freeze 自動列出當前環境的套件

```bash
pip freeze > requirements.txt
```

方法三：使用 pyproject.toml（現代推薦方式，搭配工具如 poetry 或 pdm）

範例（使用 poetry）

```toml
[tool.poetry.dependencies]
python = "^3.11"
send2trash = "^1.8"
```

## Class

```python
"""
物件 (Object) 是 類別 (Class) 的實例 (Instance)
"""

class Car:
  number_of_wheels = 4

  def __init__(self, make, model, year, color):
    # 初始化

    # 賦值
    self.make = make
    self.year = year
    self.model = model
    self.color = color

  def drive(self):
    print(f"{self.model} 行駛中")

car_1 = Car("Toyota", "Altis", 2021, "Blue")
car_2 = Car("Ford", "kuga", 1990, "White")

print(car_1.model) # Altis
print(car_2.model) # kuga

car_1.drive() # Altis 行駛中

car_1.number_of_wheels = 2
print(car_1.number_of_wheels) # 2
```

### 繼承

```python
class Animal:
  limb = 4

  def __init__(self, name):
    self.name = name

  def eat(self):
    print(f"{self.name} Eating")

class Rabbit(Animal):
  def jump(self):
    print(f"{self.name} jumping")

rabbit = Rabbit("Rabbit")
rabbit.eat()
```

### method overwriting

```python
class Animal:
  def eat(self):
    print("some animal eat")

class Fish(Animal):
  def eat(self):
    print("fish eat")

fish = Fish()
fish.eat() # fish eat
```

### method chaining

```python
class Car:
  def turn_on(self):
    print("car turn on")
    return self
  def drive(self):
    print("drive car")
    return self

car = Car()
car.drive().turn_on()

# drive car
# car turn on
```

### 繼承

```python
class Rectangle:
  def __init__(self, length, width):
    self.length = length
    self.width = width
    print(f"Rectangle init length: {length}; width:{width}")

class Square(Rectangle):
  id = 1

  def __init__(self, length, width):
    super().__init__(length, width)
    print("Square init")

class Triangle(Rectangle):
  id = 2

sq = Square(500, 500)

# Rectangle init length: 500; width:500
# Square init

# 直接使用父 class init 方法
tr = Triangle(20, 600)
# Rectangle init length: 20; width:600
```

### 傳遞物件為引數

```python
class Car:
  color = None

def change_color(car, color):
  car.color = color


car = Car()
change_color(car, 'red')
print(car.color) # red
```

### duck typing

物件所屬的類別沒有該物件擁有的方法、屬性重要，如果一個物件有足夠的方法、屬性，就算不是繼承特定類別， python 也會認定該物件為該類別的實例

> 如果走路像鴨子、叫聲像鴨子，那牠就是鴨子

```python
class Duck:
  def walk(self):
    print("Duck is walking")
  def talk(self):
    print("Duck is quacking")

class Chicken:
  def walk(self):
    print("Chicken is walking")
  def talk(self):
    print("Chicken is clucking")

# Duck 跟 Chick 沒有繼承的關係，但可以當作同樣的類別使用

def catch(duck: Duck):
  duck.walk(duck) # Chicken is walking
  duck.talk(duck) # Chicken is clucking

catch(Chicken)
```

## 獠牙運算符

- Python 3.8 才有的特性
- 賦值表達式 :=
- 賦值運算子

```python
happy = True
print(happy)

# 上面的寫法可以簡化為
print(happy := False)
```

```python
foods = []

while True:
  food = input('輸入食物 ')
  if food == 'quit':
    break
  foods.append(food)

print(foods)

# 上面的寫法可以簡化為
foods = []

while (food := input('輸入食物 ')):
    if food == 'quit':
      break
    foods.append(food)

print(foods)

# 上面的寫法可以簡化為
foods = []

while (food := input('輸入食物 ')) != 'quit':
    foods.append(food)

print(foods)
```

## lambda

- 將簡單函式簡化的語法
- 只有一個表達式
- 接受任意數量參數
- 一行寫完

```python
def double(x):
    return x * 2

print(double(5)) # 10

# lambda 寫法
double2 = lambda x: x * 2

print(double2(5)) # 10

# 多參數
multiple = lambda x, y: x * y
print(multiple(2, 3)) # 6

"""
if else
lambda 參數: <True 的回傳結果> if 條件 else <False 的回傳結果>
"""
isEven = lambda x: f"{x} 是偶數" if x % 2 == 0 else f"{x} 是奇數"
print(isEven(2)) # 2 是偶數
```

## map

```python
# map(函式, <可迭代的[列表]>): Object
fruits = [
  ('banana', 1),
  ('apple', 2),
  ('orange', 3),
  ('grape', 4),
  ('kiwi', 5),
]

add_dollar = lambda fruit: (fruit[0], fruit[1] * 1.5)

print(list(map(add_dollar, fruits)))
# [('banana', 1.5), ('apple', 3.0), ('orange', 4.5), ('grape', 6.0), ('kiwi', 7.5)]
```

## filter

過濾可以迭帶的資料結構，例如 List

```python
people = [
  ('banana', 16),
  ('apple', 21),
  ('orange', 3),
  ('grape', 44),
  ('kiwi', 50),
]

get_over = lambda humans: (humans[1] > 40)

print(list(filter(get_over, people)))
# [('grape', 44), ('kiwi', 50)]
```

## 推導式

類似 lambda 可以用比較少的語法建立列表

### 列表推導式

原本的寫法

```python
def square(x):
  return x * x

list = []
for i in range(1, 11):
  list.append(square(i))
print(list)
# [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

可以精簡為

```python
squares = [i * i for i in range(1, 11)]
print(squares)
# [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

加入條件判斷

```python
numbers = [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
num_filtered = [num for num in numbers if num > 25]
print(num_filtered)
# [36, 49, 64, 81, 100]
```

### 字典推導式

dictionary = { key: express for key, value in iterable }

```python
dic = {
  'apple': 5,
  'grape': 10
}

print({f"key:{key}": (value * 10) for key, value in dic.items()})
# {'key:apple': 50, 'key:grape': 100}
```

條件判斷

```python
weather = {
  'taipei': 'sunny',
  'tokyo': 'rainy',
  'la': 'cloudy'
}

def value_text(description):
  return f"current weather is {description}"

print({f"key:{key}": value_text(value) for key, value in weather.items() if value != 'sunny'})
# {'key:tokyo': 'current weather is rainy', 'key:la': 'current weather is cloudy'}
```

## Zip 函式

```python
user_names = ['Tom', 'jimmy', 'marry']
ages = [5,10,15]

users = zip(user_names, ages)

# print(users)
# # <zip object at 0x00000245ED8B12C0>

for i in users:
  print(i)
# ('Tom', 5)
# ('jimmy', 10)
# ('marry', 15)

"""
因為 zip() 建立的是一個迭代器。當你使用 for 迴圈遍歷 users 這個 zip 物件時，它會消耗掉所有的元素。一旦迭代器中的元素被遍歷完畢，它就變成空的。因此，當你嘗試使用 list() 再次將 users 轉換為列表時，由於迭代器已經沒有任何元素了，你會得到一個空的列表。
"""
print(list(users))
# []
```

```python
user_names = ['Tom', 'jimmy', 'marry']
ages = [5,10, 100]
date = ['1990', '1995', '1920']

users = zip(user_names, ages, date)

print(list(users))
# [('Tom', 5, '1990'), ('jimmy', 10, '1995'), ('marry', 100, '1920')]
```

```python
user_names = ['Tom', 'jimmy', 'marry']
ages = [5,10,15]

users = zip(user_names, ages)

print(dict(users))
# {'Tom': 5, 'jimmy': 10, 'marry': 15}
```

如果加入的參數數量不一致，會取最低數量

```python
user_names = ['Tom', 'jimmy', 'marry']
ages = [5,10]

users = zip(user_names, ages)

print(dict(users))
# {'Tom': 5, 'jimmy': 10}
```

## 判斷 entry 程式碼

```python title sub.py
print(f"sub.py __name__ value: {__name__}")
# sub.py __name__ value: sub
```

```python title main.py
import sub

# 當 __name__ 為字串 __main__ 代表該檔案是程式執行的入口

print(f"main.py __name__ value: {__name__}")
# main.py __name__ value: __main__
print(__name__ == '__main__')
# True
```

## time

```python
import time

# 001 系統初始時間 epoch 系統時間

print(time.ctime(0))

# Thu Jan 1 08:00:00 1970

# 002 系統初始時間 epoch 到現在的系統時間，用秒表示

print(time.time())

# 1747670942.4822173

# 003 格式化時間

local_time = time.localtime()
print(local_time)

# local_time 是 時間物件

print(time.strftime("%Y-%m-%d %H:%M:%S", local_time))

# 2025-05-20 00:09:02

# 004 字串轉時間物件

time_obj = time.strptime("2021-01-01 12:05:06", "%Y-%m-%d %H:%M:%S")
print(time_obj)
"""
time.struct_time(
tm_year=2021,
tm_mon=1,
tm_mday=1,
tm_hour=12,
tm_min=5,
tm_sec=6,
tm_wday=4,
tm_yday=1,
tm_isdst=-1
)
"""
```

## pip 套件管理

python 3.4 以上的版本才有提供

```terminal
<!-- 列印版本 -->
pip --version

<!-- 升級 pip -->
pip install --upgrade pip

<!-- 顯示安裝的套件 -->
pip list

<!-- 安裝套件 -->
pip install <套件名稱>
pip install pygame
```
