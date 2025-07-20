---
title: 基本知識
tags:
  - python
sidebar_position: 1
---

REF:

- [Python 6 小時初學者課程 (2023)](https://www.youtube.com/watch?v=lvH4-4iYjgs)
- [Python Full Course for free 🐍 (2024)](https://www.youtube.com/watch?v=ix9cRaBkVe0)

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

或者用 dir 列舉所有屬性跟方法

```python
arr = ['1']
print() # 取得全域變數，例如 '__name__'
print(dir(arr))
```

取得 module 說明

```py
print(help("modules"))
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

print(type("string") == str) # True
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

print(type(bool("string"))) #<class 'bool'>
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
elif not temp > 50
  print('溫度不超過 50')
else :
  print('不適宜溫度')
```

### membership operators

used to test whether a value or variable is found in a sequence (string, list, tuple, set or dictionary)

1. in
2. not in

```python
name = "John"

print("J" in name) # True
print("Joh" in name) # True
print("hD" not in name) # True
print("L" not in name and "hn" in name) # True
```

### Conditional (ternary) operator

```python
num = 1
print("Positive" if num > 0 else "Negative")
# Positive
```

相當於 js

```javascript
let num = 1
console.log(num > 0 ? 'Positive' : 'Negative')
```

### match

相當於 js 的 switch 語法

```python
def day_of_week(day):
  match day:
    case 0:
      return "0"
    case 1:
      return "sunday"
    case 2:
      return "monday"
    case 3:
      return "tuesday"
    case 4:
      return "wednesday"
    case 5:
      return "thursday"
    case 6:
      return "friday"
    case 7:
      return "saturday"
    case _:
      return "not a valid day"

print(day_of_week(1)) # sunday
print(day_of_week(True)) # sunday
print(day_of_week(False)) # 0
print(day_of_week([])) # not a valid day
```

如果要合併條件，需要透過 or 運算符號 |

```python
def is_weekend(day):
  match day:
    case 'saturday' | "sunday":
      return True
    case _:
      return False

print(is_weekend("sunday")) # True
print(is_weekend("saturday")) # True
print(is_weekend("birthday")) # False
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

### 字串索引 indexing

參數是 [start : end : step]，每個參數都可以省略

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

credit_number = "123456789"
print(credit_number[::3]) # 147
print(credit_number[-3:]) # 789
print(credit_number[::-1]) # 987654321
```

### f-string 格式化字串 (Format Specifier)

#### 固定小數點位數

- :.(number)f ➡️ round to that may decimal places (fixed point)

```python
value1 = 3.1415
print(f"{value1:.2f}") # 3.14

value2 = -9.641561
print(f"{value2:.2f}") # -9.64

value3 = 5
print(f"{value3:.2f}") # 5.00
```

#### 指定文字長度

- :(number) ➡️ allocate that many spaces

如果文字本身小於長度，會用空格填空

```python
value1 = 3.1415
print(f"{value1:10}") #     3.1415

value2 = -9.641561
print(f"{value2:10}") #  -9.641561

value3 = 5
print(f"{value3:10}") #          5
```

- :03 ➡️ allocate and zero pad that many spaces
  如果數字前面補上 0，會用 0 填空

```python
value1 = 3.1415
print(f"{value1:010}") # 00003.1415

value2 = -9.641561
print(f"{value2:010}") # -09.64156

value3 = 5
print(f"{value3:010}") # 0000000005
```

#### 排列

- :< ➡️ left justify
- :> ➡️ right justify
- :^ ➡️ center align

```python
value = 9
print(f"{value:<10}")
print(f"{value:>10}")
print(f"{value:^10}")
print(f"{value:<010}")
print(f"{value:>010}")
print(f"{value:^010}")

"""
9
         9
    9
9000000000
0000000009
0000900000
"""
```

#### 表示正、負數

- :+ ➡️ use a plus sign to indicate positive value

```python
value1 = 3.1415
print(f"{value1:+}") # +3.1415

value2 = -9.641561
print(f"{value2:+}") # -9.641561

value3 = 5
print(f"{value3:+}") # +5
```

#### 將符號擺放至最左

- := ➡️ place sign to leftmost position

```python
value1 = -3.1
print(f"{value1:10}") #       -3.1
print(f"{value1:=10}") # -      3.1
```

#### 文字前面補上空格

- : ➡️ insert a space before position numbers

```python
value1 = 3.1415
print(f"{value1: }") #  3.1415
print(f"{value1:+}") # +3.1415

value2 = -9.641561
print(f"{value2: }") # -9.641561
print(f"{value2:+}") # -9.641561

value3 = 5
print(f"{value3: }") #  5
print(f"{value3:+}") # +5
```

#### 格式化數字

- :, ➡️ comma separator

```python
value = 90000000
print(f"{value:,}") # 90,000,000
print(f"{value:,.2f}") # 90,000,000.00
```

#### 總結

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

### iterables

```python
iterables = { list: [1,2], tuple: (3,4), set: {5,6}, str: '789' }

for iterable in iterables.values():
  for item in iterable:
    print(item, end="/")
# 1/2/3/4/5/6/7/8/9/

dic = { 'a': 1, 'b': 2 }

for key, value in dic.items():
  pass
for value in dic.values():
  pass
for key in dic.keys():
  pass
```

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

prompt_text = '輸入食物名稱 (輸入 q 離開)'
food = input(prompt_text)

while not food == 'q':
  print(f"你輸入食物名稱{food}")
  food = input(prompt_text)
```

### for

for 變數 in 迭代物:

```python
# 逐一迭代文字
char = "this is a python"
for x in char:
  print('x value', x)

# 輸出 0 ~ 10 (不包含 11)
for x in range(0, 11):
  print('x value', x)

# 輸出 10 ~ 0 (不包含 0)
for x in range(10, 0, -1):
  print('x value', x)

# 輸出 0 ~ 10 的 2 的倍數
for x in range(0, 11, 2):
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

# 007 複製
# ['+', '+', '+', '+', '+']
arr = ["+"] * 5
print(arr)
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

# 004 建立空 set
container = set()
container.add('5')
print(container) # {'5'}
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

print("Tokyo" in capital)
# True
print("Tokyo" not in capital)
# False

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

### 關鍵字參數 (keyword arguments)

- an argument preceded by an identifier, helps with readability order; order of arguments doesn't matter
- 帶多參數函式、提升可讀性、更少限制傳遞參數(?)

例如 print 常用 end 指定列印的文字後面該帶什麼字串

```python
print("say my name", end=" ")
```

```python
def get_phone(country_code, area_code, first, second):
    """
    This function takes a country code, area code, first number, and second number as input
    and returns a formatted phone number string.
    """
    return f"+{country_code} ({area_code}) {first}-{second}"

# +886 (02) 88-6
print(get_phone(country_code="886", area_code="02", first="88", second="6"))

# 指定 keyword arguments 後，可以不依照順序
# +886 (02) 88-6
print(get_phone(first="88", second="6", country_code="886", area_code="02"))

# 沒有指定的 keyword 一定要在前，否則噴錯
# +886 (02) 88-6
print(get_phone("886", area_code="02", first="88", second="6"))
# SyntaxError: positional argument follows keyword argument
print(get_phone(area_code="02", first="88", second="6", "886"))
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

args、kwargs 可以同時使用

```python
def accept(*args, **kwargs):
  for a in args:
    print(f"args {a};", end=" ")

  print()

  for key, value in kwargs.items():
    print(f"key: {key}; value: {value}")


accept("my", "name", "is", name="john")
"""
args my; args name; args is;
key: name; value: john
"""

accept("my", "name", "is") # 這樣寫不會噴錯
accept(name="john") # 這樣寫不會噴錯

# args 一定要在前，這樣寫是錯誤的
def accept2(**kwargs, *args):
  for a in args:
    print(f"args {a};", end=" ")

  print()

  for key, value in kwargs.items():
    print(f"key: {key}; value: {value}")
```

### Decorator

一個能代理 function 的 function，能夠在沒有更動原本 function 情況下作額外處理

```python
def add_text(func):
   def wrapper():
      print(f"hack function")
      func()
   return wrapper

@add_text
def raw_function():
  print(f"this is raw fn")

raw_function()
# hack function
# this is raw fn
```

decorator 一定要返回 function，否則會立刻執行，不管有沒有執行該 function

```python
def add_text(func):
  print(f"hack function")
  func()

@add_text
def raw_function():
  print(f"this is raw fn")

# 😒 雖然沒有呼叫 raw_function 但是 add_text 執行了
# hack function
# this is raw fn
```

decorator 可以複數

```python
def add_text(func):
  def wrapper():
    print(f"add_text")
    func()
  return wrapper

def add_info(func):
  def wrapper():
    print(f"add_info")
    func()
  return wrapper

@add_info
@add_text
def raw_function():
  print(f"this is raw fn")

raw_function()
# add_info
# add_text
# this is raw fn
```

decorator 也能傳也能傳入帶入的參數

```python
def add_text(func):
  def wrapper(*args, **kwargs):
    print("args", args, "kwargs", kwargs)
    func(*args, **kwargs)
  return wrapper

@add_text
def raw_function(name, value):
  print(f"this is raw fn, name: {name}; value: {value}")

raw_function('cat', value="True")
# args ('cat',) kwargs {'value': 'True'}
# this is raw fn, name: cat; value: True
```

## variable scope

where a variable is visible and accessible.
scope resolution = Local ➡️ Enclosed ➡️ Global ➡️ Built-in

變數會先從執行的位置 (local) 尋找，沒有的話，依序向上尋找

```python
from math import e

e # built-in

x = 0 # Global

def func1():
  x = 1 # Enclosed
  def func2():
    x = 2 # Local
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

from math import pi
print(pi) # 3.141592653589793
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

### 主動拋錯

使用 raise 關鍵字來主動拋出錯誤

```python
def divide(a, b):
    if b == 0:
        raise ValueError("除數不能為零！")  # 拋出 ValueError 異常
    return a / b

try:
    result = divide(10, 0)
    print(result)
except ValueError as e:
    print(f"發生錯誤：{e}")

print("-" * 20)

def get_item(index, my_list):
    if not (0 <= index < len(my_list)):
        raise IndexError("索引超出範圍！")  # 拋出 IndexError 異常
    return my_list[index]

try:
    my_list = [1, 2, 3]
    item = get_item(5, my_list)
    print(item)
except IndexError as e:
    print(f"發生錯誤：{e}")
```

#### 拋出自訂異常

```python
class InsufficientFundsError(Exception):
    """
    自訂異常：餘額不足
    """
    def __init__(self, message="餘額不足，交易失敗！", required_amount=0, current_balance=0):
        super().__init__(message)
        self.required_amount = required_amount
        self.current_balance = current_balance

def withdraw(amount, balance):
    if amount > balance:
        raise InsufficientFundsError(
            message=f"提款失敗：需要 {amount}，但餘額只有 {balance}。",
            required_amount=amount,
            current_balance=balance
        )
    return balance - amount

try:
    current_balance = 500
    withdraw_amount = 700
    new_balance = withdraw(withdraw_amount, current_balance)
    print(f"新餘額：{new_balance}")
except InsufficientFundsError as e:
    print(f"發生自訂錯誤：{e}") # 發生自訂錯誤：提款失敗：需要 700，但餘額只有 500。
    print(f"需要金額：{e.required_amount}") # 需要金額：700
    print(f"目前餘額：{e.current_balance}") # 目前餘額：500
```

#### 再次拋出異常（Re-raising an Exception）

只使用 raise 關鍵字而**不帶任何參**

```python
def process_data(data):
    try:
        # 假設這裡會發生一些錯誤
        if not data:
            raise ValueError("數據不能為空！")
        print(f"正在處理數據：{data}")
        # ... 其他處理邏輯 ...
    except ValueError as e:
        print(f"在 process_data 中捕獲到錯誤：{e}")
        # 可以在這裡執行日誌記錄或清理工作
        raise  # 再次拋出相同的錯誤

try:
    process_data(None)
except ValueError as e:
    print(f"在主程式中捕獲到錯誤：{e}")
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

#### with

稱為上下文管理器 (Context Manager)，它提供了一種簡潔且安全的方式來處理資源，確保資源在不再需要時被正確地獲取（設置）和釋放（清理），即使在程式碼執行過程中發生錯誤也能正常運作

##### 功能

1. 確保資源正確釋放

with 語法最核心的功能。在處理檔案、網路連線、鎖定（lock）等資源時，我們需要確保它們在完成操作後被關閉或釋放，以避免資源洩漏、死鎖或其他潛在問題。with 語法自動處理了這個過程，無論程式碼是否正常執行完畢，或是否遇到例外，都能保證資源被清理

沒有 with，需要使用 try...finally 區塊來確保資源被釋放，這會讓程式碼變得冗長且容易出錯

```python
file = open("my_file.txt", "r")
try:
    content = file.read()
    # 處理內容
finally:
    file.close() # 必須手動關閉
```

open() 函式回傳的檔案物件就是一個上下文管理器。當程式碼進入 with 區塊時，檔案會被打開；當程式碼離開 with 區塊時（無論是正常結束還是因為錯誤），file.close() 會自動被呼叫

```python
with open("my_file.txt", "r") as file:
    content = file.read()
    # 處理內容
# 檔案在離開 with 區塊後會自動關閉
```

##### 上下文管理器協定

一個物件如果想支援 with 語法，它必須實作以下兩個特殊方法：

- **enter**(self)：
  - 當程式碼進入 with 區塊時，這個方法會被呼叫。
  - 它的回傳值會被賦值給 as 子句後面的變數（例如 with open(...) as file: 中的 file）。
  - 通常用於資源的獲取或初始化。
- **exit**(self, exc_type, exc_val, exc_tb)：
  - 當程式碼離開 with 區塊時，這個方法會被呼叫。無論是正常結束、break、continue、return 還是發生例外，它都會被呼叫。
  - 參數 exc_type、exc_val、exc_tb 分別代表例外類型、例外值和追溯資訊。如果沒有發生例外，它們都會是 None。
  - 主要用於資源的釋放或清理。如果 **exit** 方法回傳 True，表示它已經處理了例外，例外將不會被傳播；如果回傳 False 或沒有回傳值（預設），則例外會繼續傳播。

```python
class MyContext:
    def __init__(self, name):
        self.name = name
        print(f"初始化 MyContext({self.name})")

    def __enter__(self):
        print(f"進入 with 區塊：{self.name} - 資源獲取")
        return self.name.upper() # 回傳給 'as' 後的變數

    def __exit__(self, exc_type, exc_val, exc_tb):
        print(f"離開 with 區塊：{self.name} - 資源釋放")
        if exc_type:
            print(f"發生例外：類型={exc_type.__name__}, 值={exc_val}")
            # return True # 如果回傳 True，例外將被抑制

print("--- 開始示範 ---")
with MyContext("test_resource") as resource:
    print(f"在 with 區塊內，資源變數為：{resource}")
    # 這裡可以執行一些操作

print("--- 示範結束 ---")

print("\n--- 帶有例外的示範 ---")
try:
    with MyContext("error_resource") as resource:
        print(f"在 with 區塊內，資源變數為：{resource}")
        raise ValueError("這是一個模擬的錯誤")
except ValueError as e:
    print(f"外部捕獲到例外：{e}")

print("--- 帶有例外的示範結束 ---")
```

輸出

```console
--- 開始示範 ---
初始化 MyContext(test_resource)
進入 with 區塊：test_resource - 資源獲取
在 with 區塊內，資源變數為：TEST_RESOURCE
離開 with 區塊：test_resource - 資源釋放
--- 示範結束 ---

--- 帶有例外的示範 ---
初始化 MyContext(error_resource)
進入 with 區塊：error_resource - 資源獲取
在 with 區塊內，資源變數為：ERROR_RESOURCE
離開 with 區塊：error_resource - 資源釋放
發生例外：類型=ValueError, 值=這是一個模擬的錯誤
外部捕獲到例外：這是一個模擬的錯誤
--- 帶有例外的示範結束 ---
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
  number_of_wheels = 4 # class variables

  def __init__(self, make, model, year, color):
    # 初始化

    # 賦值給 instance variables
    self.make = make
    self.year = year
    self.model = model
    self.color = color

  def drive(self):
    print(f"{self.model} 行駛中")

car_1 = Car("Toyota", "Altis", 2021, "Blue")
car_2 = Car("Ford", "kuga", 1990, "White")

print(car_1) # <__main__.Car object at 0x000001D4CA3B6900> ⬅️ 記憶體位址
print(type(car_1) == Car) # True
print(car_1.model) # Altis
print(car_2.model) # kuga

car_1.drive() # Altis 行駛中

car_1.number_of_wheels = 2
print(car_1.number_of_wheels) # 2
```

- class variables 是所有 class 共有的數值
- class variables are shared among all instances of a class, defined outside the constructor, allow you to share data among all objects created from that class

```python
class Car:
  num_of_instance = 0

  def __init__(self):
    Car.num_of_instance += 1

car_1 = Car()
car_2 = Car()

print(f"num of inst: {Car.num_of_instance}") # 2
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

multiple inheritance = inherit from more than one parent class

- C(A, B)

multilevel inheritance

- inherit from a parent which inherits from another parent.
- C(B) ⬅️ B(A) ⬅️ A

```python
class Animal:
  def __init__(self, name):
    print(f"animal {name} init")
    self.name = name

class Prey(Animal):
  def flee(self):
    print(f"{self.name} fleeing")

class Predator(Animal):
  def hunt(self):
    print(f"{self.name} hunting")

class Rabbit(Prey):
  pass

class Tiger(Predator):
  pass

class Fish(Predator, Prey):
  pass

rabbit = Rabbit("Mr. rabbit")
# animal Mr. rabbit init
rabbit.flee()
# Mr. rabbit fleeing

tiger = Tiger("Ms. Tiger")
# animal Ms. Tiger init
tiger.hunt()
# Ms. Tiger hunting

fish = Fish("Lady Fish")
# animal Lady Fish init
fish.flee()
# Lady Fish fleeing
fish.hunt()
# Lady Fish hunting
```

#### SUPER

- Function used in a child class to call methods from a parent class (super class), allows to extend the functionality of the inherit methods
- 繼承父 class，複用功能

例子 001

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

例子 002

```python
class Circle():
  def __init__(self, color, radius):
    self.color = color
    self.radius = radius

class Square():
  def __init__(self, color, width):
    self.color = color
    self.width = width
```

可以簡化成

```python
class Shape():
  def __init__(self, color):
    self.color = color

  def describe(self):
    print(f"color: {self.color}")

class Circle(Shape):
  def __init__(self, color, radius):
    super().__init__(color)
    self.radius = radius

  def describe(self):
    print(f"circle color: {self.color}")

class Square(Shape):
  def __init__(self, color, width):
    super().__init__(color)
    self.width = width

c = Circle("red", 5)
c.describe() # circle color: red

s = Square("blue", 10)
s.describe() # color: blue
```

### classmethod

取得該 class 的屬性、參數，通常用於

1. 當作工廠方法

```python
class Shape:
    def __init__(self, color):
        print(f"init {color}")
        self.color = color

    def show_color(self):
        print(f"This shape's color is {self.color}.")

    @classmethod
    def create_blue_shape(cls):
        """
        這是一個工廠方法。
        cls 參數接收到的是 Shape 這個類別。
        回傳一個 color 為 'blue' 的 Shape 實例。
        """
        print(f"cls: {cls}") # cls: <class '__main__.Shape'>
        return cls('blue')

# 不需要先建立實例，直接用類別呼叫 classmethod
blue_shape = Shape.create_blue_shape()

blue_shape.show_color()
# 輸出: This shape's color is blue.
```

2. 操作類別屬性

```python
class Shape:
    # 這是一個類別屬性，所有 Shape 實例共享
    shape_type = 'General Shape'

    def __init__(self, color):
        # 這是實例屬性
        self.color = color

    @classmethod
    def describe(cls):
        """
        cls 參數接收到的是 Shape 這個類別。
        它可以存取類別屬性 shape_type。
        """
        print(f"All shapes created from this class are of type: {cls.shape_type}")

# 直接透過類別呼叫
Shape.describe()
# 輸出: All shapes created from this class are of type: General Shape
```

### static methods

- a method that belong to a class rather than any object from that class (instance). Usually used for general utility functions
- Instance methods: best for operations on instances of the class (objects)
- Static methods: best from utility functions that do not need access to class data

```python

class Employee:
  @staticmethod
  def is_valid_position(position):
    return position in ["Cook", "Janitor"]

print(Employee.is_valid_position("Cashier")) # False
print(Employee.is_valid_position("Cook")) # True
```

### class method

allow operations related to the class itself, take (cls) as the first parameter, which represents the class itself

```python
class Student:
  count = 0
  def __init__(self, name):
    self.name = name
    Student.count += 1

  # instance method
  def info(self):
    return f"name: {self.name}"

  @classmethod
  def total_student_num(cls):
    return f"total num of students: {cls.count}"

Student("Tom")
print(Student.total_student_num()) # total num of students: 1
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

### Dunder methods

- 相當於 JS 中的 toString、valueOf.
- magic methods, dunder methods (double underscore) **init**, **str**, **eq**, they are automatically called by many of Python's built-in operations. they allow developers to define or customize the behavior of objects

```python
class Book:
  def __init__(self, title, author, num_pages):
    self.title = title
    self.author = author
    self.num_pages = num_pages

  def __str__(self):
    return f"'{self.title}' by {self.author}"

  def __eq__(self, other):
    return self.title == other.title and self.author == other.author

  def __sub__(self, other):
    return self.num_pages - other.num_pages

  def __lt__(self, other):
    return self.num_pages < other.num_pages

  def __gt__(self, other):
    return self.num_pages > other.num_pages

  def __add__(self, other):
    return f'{self.num_pages + other.num_pages} pages'

  def __contains__(self, keyword):
    return keyword in self.title

  def __getitem__(self, key):
    match key:
      case 'special':
        return 'no thing special'
      case _:
        return self[key]

book1 = Book('相對論', '愛因斯坦', 869)
book2 = Book('相對論', '愛因斯坦', 123)
book3 = Book('建國論', '斯坦', 456)
print(str(book1)) # '相對論' by 愛因斯坦
print(book1 == book2) # True
print(book1 == book3) # False
print(book1 - book3) # 413
print(book1 < book3) # False
print(book1 > book3) # True
print(book1 + book3) # 1325 pages
print('相對' in book1) # True
print(book1['special']) # no thing special
```

參考

- [JS 中 toString()和 valueOf()的用法及两者的区别](https://blog.csdn.net/Web_J/article/details/84106129)
- [更 Python 的 Pythonic Coding – Dunder Method 篇](https://zhung.com.tw/article/more-pythonic-python-code-dunder-methods/)

### @property

- 類似於 JS class 的 private property 以及 getter、setter 用法
- decorator used to define a method as property (it can be accessed like an attribute)
- benefit: ass additional logic when read, write or delete attributes
- gives getter, setter and delete method

```python
class Rectangle:
  def __init__(self, width, height):
    self._width = width
    self._height = height

  # @property: 把一個方法變成屬性，這裡定義了 @width 屬性
  @property
  def width(self):
    return f"{self._width:.1f}cm"

  @property
  def height(self):
    return f"{self._height:.1f}cm"

  @width.setter
  def width(self, new_value):
    if new_value > 0:
      self._width = new_value
    else:
      raise ValueError("寬度不能小於 0")

  # @width 屬性指定 getter，這會覆蓋 @property 定義的 width
  @width.getter
  def width(self):
    return f"get value: {self._width}"

  @width.deleter
  def width(self):
    print(f"you delete width!!")
    del self._width

rec = Rectangle(5, 10)
print(rec.width)
# get value: 5
print(rec.height)
# 10.0cm
del rec.width
# you delete width!!
print(rec.width)
# AttributeError: 'Rectangle' object has no attribute '_width'. Did you mean: 'width'?
rec.width = -1
# ValueError: 寬度不能小於 0
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

### 列表推導式 List comprehension

寫法

```python
變數 = [表達式 for 值 in 迭代 if 條件]
list = [expression for value in iterable if condition]
```

相當於 js

```javascript
const list = Array.from(iterable).map(expression).filter(condition)
const list = Array.from({ length: 100 }, (value, index) => index + 1).filer((idx) => idx % 2 === 1)
```

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

- this script can be imported or run standalone
- Functions and classes in this module an be reused without the main block of code executing

```python
def main():
  # ....

if __name__ == '__main___':
  main()
```

- good practice

  - code is modular
  - helps readability
  - leaves no global variables
  - avoid unintended execution

- example
  - library = import library for functionality
  - when running library directly, display a help page

## multi threading

- used to perform multiple tasks concurrently (multitasking)
- Goof for I/O (input and output) bound tasks like reading files or fetching data from APIs threading.
- Thread(target=name_of_function)

```py
import time
import threading

def fun_01():
  time.sleep(2)
  print("func 1 done")

def fun_02():
  time.sleep(4)
  print("func 2 done")

def fun_03():
  time.sleep(6)
  print("func 3 done")

# 需要花費 2 + 4 + 6 = 12 秒跑完所有程式碼
fun_01()
fun_02()
fun_03()

# 需要花費 6 秒跑完所有程式碼
chore1 = threading.Thread(target=fun_01)
chore1.start()
chore2 = threading.Thread(target=fun_02)
chore2.start()
chore3 = threading.Thread(target=fun_03)
chore3.start()

# 如果要讓所有 thread 執行完，使用 join 方法
print("immediately")

chore1.join()
chore2.join()
chore3.join()

print("all done")

"""
整個程式碼會依序列印出

immediately
func 1 done
func 2 done
func 3 done
all done
"""
```

攜帶參數

```py
import time
import threading

def fun(msg, sec = 1):
  time.sleep(sec)
  print(f"msg: {msg};sec: {sec}")

"""
如果要攜帶參數，要以 tuple 型式，而且至少要兩個參數
"""

# 不合法，會拋錯
chore1 = threading.Thread(target=fun, args=("function 1 start"))
chore1.start()

# 合法
chore2 = threading.Thread(target=fun, args=("function 2 start",))
chore2.start()

# 合法
chore3 = threading.Thread(target=fun, args=("function 2 start", 3))
chore3.start()
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

## datetime

```py
import datetime

date = datetime.date(2025,1,2)
print(date) # 2025-01-02
print(type(date)) # <class 'datetime.date'>

time = datetime.time(12,30,0)
print(time) # 12:30:00
now = datetime.datetime.now()
print(now) # 2025-06-23 00:26:16.345169

print(now.strftime("%H:%M:%S %m-%d-%Y")) # 00:26:16 06-23-2025

"""
format https://docs.python.org/3/library/datetime.html#strftime-and-strptime-format-codes
"""

target_time = datetime.datetime(2099,1,2,12,30,1)

print(now < target_time) # True
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

## request api

需要先透過指令 `pip install requests` 安裝套件 `requests`

```py
import requests
# https://pokeapi.co/
base_url = 'https://pokeapi.co/api/v2/'

def get(name):
  url = f"{base_url}/pokemon/{name}"
  res = requests.get(url)

  if res.status_code == 200:
    return res.json()
  else:
    print(f"no data available for pokemon name: {name}")
    return None

res = get('pikachu')
print(res)
```

如果需要定義 response 資料結構

```py
from typing import Dict, List, Union
import requests

# 更精確的定義方式 (推薦，因為 TypedDict 提供更好的靜態分析)
try:
    from typing import TypedDict
except ImportError:
    # Fallback for Python versions older than 3.8
    TypedDict = Dict # 只是為了讓程式碼能跑，但失去 TypedDict 的優勢

class AbilitySlot(TypedDict):
    is_hidden: bool
    slot: int

class PokemonResponseData(TypedDict):
  abilities: List[AbilitySlot]
  base_experience: int
  height: int
  id: int
  name: str
  order: int


# https://pokeapi.co/
base_url = 'https://pokeapi.co/api/v2/'

def get(name: str) -> Union[PokemonResponseData, None]:
  url = f"{base_url}/pokemon/{name}"
  res = requests.get(url)

  if res.status_code == 200:
    return res.json()
  else:
    print(f"no data available for pokemon name: {name}")
    return None

res = get('pikachu')

if (res):
  print(res['base_experience'])
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
