---
title: 使用者介面製作
tags:
  - python
sidebar_position: 2
---

參考來源: [Python Full Course for free 🐍 (2024)](https://www.youtube.com/watch?v=ix9cRaBkVe0)

## Graphical User Interface

### 安裝

```console
pip install PyQt5
```

### 概念

#### 基本架構

```py
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow

class MainWindow(QMainWindow):
  def __init__(self):
    super().__init__()

def main():
  # 可以從 sys.argv 得到傳遞給 main.py 的參數
  # 例如 py main.py my_argv 66
  app = QApplication(sys.argv)
  window = MainWindow()

  # 視窗預設不顯示，需要執行 show
  window.show()

  # 執行到這行因為所有程式碼都結束了，需要另外設定程式碼結束時間點，否則 GUI 會瞬間關閉
  sys.exit(app.exec_())

if __name__ == "__main__":
  main()
```

#### 基本功能

```py
class MainWindow(QMainWindow):
  def __init__(self):
    super().__init__()
    # 建立視窗標題
    # highlight-next-line
    self.setWindowTitle("Python app title")

    # 指定視窗顯示位置(從左上開始)、大小
    # highlight-start
    x = 0
    y = 0
    width = 720
    height = 480
    self.setGeometry(x,y,width, height)
    # highlight-end
```

#### icon 設定

```py
from PyQt5.QtGui import QIcon

class MainWindow(QMainWindow):
  def __init__(self):
    super().__init__()
    self.setWindowIcon(QIcon('./icon/app_icon.jpg'))
```

#### label

相當於 html 中的 div，可以顯示文字、圖片

```py
from PyQt5.QtWidgets import QApplication, QMainWindow, QLabel

class MainWindow(QMainWindow):
  def __init__(self):
    super().__init__()

    # 這裡的 self 代表 window，也就是 parent widget
    label = QLabel("hello world", self)
    # 字型設定 QFont(字型名稱,字型大小(單位數字))
    label.setFont(QFont('Arial', 16))
    # 字型也能設定位置、大小
    label.setGeometry(5,5,600,200)
    # 設定 css，透過 """ """ 可以換行寫比較多 css 樣式
    label.setStyleSheet("""
      color: red;
      font-weight: bold;
    """)
```

#### 排列

```py
from PyQt5.QtCore import Qt

class MainWindow(QMainWindow):
  def __init__(self):
  label = QLabel("hello world", self)

  # 垂直置頂
  label.setAlignment(Qt.AlignTop)
  # 垂直置底
  label.setAlignment(Qt.AlignBottom)
  # 垂直置中
  label.setAlignment(Qt.AlignVCenter)

  # 水平置右
  label.setAlignment(Qt.AlignRight)
  # 水平置中
  label.setAlignment(Qt.AlignHCenter)
  # 水平置左
  label.setAlignment(Qt.AlignLeft)

  # 水平、垂直置中
  label.setAlignment(Qt.AlignCenter)

  # 用 or operator 合併位置
  label.setAlignment(Qt.AlignHCenter | Qt.AlignTop)

```

#### 顯示圖片

```py
from PyQt5.QtGui import QPixmap

class MainWindow(QMainWindow):
  def __init__(self):
    super().__init__()

    self.setGeometry(0,0, 720, 480)

    # QLabel 傳入 window，也就是 self
    label = QLabel(self)
    label.setGeometry(0,0, 360, 240)

    path_to_img = 'icon.webp'
    pixMap = QPixmap(path_to_img)

    # 設定圖片到這個 label 上
    label.setPixmap(pixMap)

    # 是否要讓圖片依照 label 大小縮放，True 代表是
    label.setScaledContents(True)

    # 將圖片垂直水平置中
    label.setGeometry((self.width() - label.width()) // 2,
                      (self.height() - label.height()) // 2,
                      label.width(),
                      label.height())
```

#### Layout

##### 垂直排列

```py
from PyQt5.QtWidgets import (
  QApplication, QMainWindow, QLabel, QWidget, QVBoxLayout, QHBoxLayout, QGridLayout
)

class MainWindow(QMainWindow):
  def __init__(self):
    super().__init__()

    # 在視窗中央建立一個主部件
    central_widget = QWidget()
    self.setCentralWidget(central_widget)

    label_colors = ['red', 'yellow', 'green', 'blue', 'purple']

    # 垂直排列容器
    vbox = QVBoxLayout()

    for index, color in enumerate(label_colors):
      label = QLabel(f"#{index}", self)
      label.setStyleSheet(f"background-color: {color};")
      vbox.addWidget(label)

    central_widget.setLayout(vbox)
    """
    會看到視窗顯示的畫面:

    --- #0 (紅色) ---
    --- #1 (黃色) ---
    --- #2 (綠色) ---
    --- #3 (藍色) ---
    --- #4 (紫色) ---
    """
```

##### 水平排列

```py
class MainWindow(QMainWindow):
  def __init__(self):
    super().__init__()

    # 在視窗中央建立一個主部件
    central_widget = QWidget()
    self.setCentralWidget(central_widget)

    label_colors = ['red', 'yellow', 'green', 'blue', 'purple']

    # 水平排列容器
    hbox = QHBoxLayout()

    for index, color in enumerate(label_colors):
      label = QLabel(f"#{index}", self)
      label.setStyleSheet(f"background-color: {color};")
      hbox.addWidget(label)

    central_widget.setLayout(hbox)
    """
    會看到視窗顯示的畫面:

    --- #0 (紅色) --- #1 (黃色) --- #2 (綠色) --- #3 (藍色) --- #4 (紫色) ---
    """
```

##### Grid 格線系統

```py
class MainWindow(QMainWindow):
  def __init__(self):
    super().__init__()

    # 在視窗中央建立一個主部件
    central_widget = QWidget()
    self.setCentralWidget(central_widget)

    label_colors = ['red', 'yellow', 'green', 'blue', 'purple']

    # 水平排列容器
    grid = QGridLayout()

    for index, color in enumerate(label_colors):
      label = QLabel(f"#{index}", self)
      label.setStyleSheet(f"background-color: {color};")

      row = index // 3
      column = index % 3
      print(f"r: {row}; c: {column}; index: {index}")
      grid.addWidget(label, row, column)

    central_widget.setLayout(grid)
    """
    r: 0; c: 0; index: 0
    r: 0; c: 1; index: 1
    r: 0; c: 2; index: 2
    r: 1; c: 0; index: 3
    r: 1; c: 1; index: 4

    會看到視窗顯示的畫面:

    --- #1 (紅色) --- #2 (黃色) --- #3 (綠色) ---
    --- #4 (藍色) --- #5 (紫色) ---
    """
```

#### 按鈕

```py
from PyQt5.QtWidgets import (
  QApplication, QMainWindow, QLabel, QPushButton
)

class MainWindow(QMainWindow):
  def __init__(self):
    super().__init__()

    self.button = QPushButton("click me", self)
    # 指定按下的動作
    self.button.clicked.connect(self.on_btn_click)

  def on_btn_click(self):
    # 設定按鈕文字
    self.button.setText('Clicked')
    # 設定禁止使用按鈕
    self.button.setDisabled(True)
```

#### checkBox

```py
from PyQt5.QtWidgets import (QApplication, QMainWindow, QCheckBox)
from PyQt5.QtCore import Qt

class MainWindow(QMainWindow):
  def __init__(self):
    super().__init__()

    checkBox = QCheckBox('text aside checkbox', self)
    checkBox.setStyleSheet('font-size: 30px;')
    # 設定初始值
    checkBox.setChecked(True)
    checkBox.stateChanged.connect(self.on_checkbox_click)

  def on_checkbox_click(self, newState):
    # newState 是當下 checkbox 最新值
    print(f'checkbox value: {newState}')

    # 0 unchecked、2 checked
    if newState == Qt.Checked:
      print("checkbox is checked")
    else:
      print("uncheck checkbox")
```

#### radio button

```py
from PyQt5.QtWidgets import (QApplication, QMainWindow, QRadioButton, QButtonGroup)

class MainWindow(QMainWindow):
  def __init__(self):
    super().__init__()

    self.setGeometry(700, 300, 500, 500)

    # 設定 radio button 的群組，一組只有一個 radio button 會被選擇
    button_group_1 = QButtonGroup(self)
    button_group_2 = QButtonGroup(self)

    radioBtnTexts = ['Visa', 'Mastercard', 'Gift Card', 'In-Store', 'Online']

    for (index, text) in enumerate(radioBtnTexts):
      radio = QRadioButton(text, self)
      radio.setGeometry(0, index * 50, 300, 50)
      radio.toggled.connect(self.radio_btn_changed)

      if index < 3:
        button_group_1.addButton(radio)
      else:
        button_group_2.addButton(radio)

    # 設定共用 css style
    self.setStyleSheet("""
        QRadioButton {
          font-size: 40px;
          padding: 10px;
        }
    """)

  def radio_btn_changed(self):
    # 從 sender 回傳值取得按鈕本身
    radio_btn_element = self.sender()
    if radio_btn_element.isChecked():
      print(f"{radio_btn_element.text()} is selected")
```

#### line edits

相當於 html 中的 input tag

```py
from PyQt5.QtWidgets import (QApplication, QMainWindow, QPushButton, QLineEdit)

class MainWindow(QMainWindow):
  def __init__(self):
    super().__init__()

    self.setGeometry(700, 300, 500, 500)

    line_edit = QLineEdit(self)
    line_edit.setGeometry(10,10,200,40)
    line_edit.setStyleSheet(
      "font-size: 25px;"
      "font-family:Arial;"
      "color: cyan;"
    )
    self.line_edit = line_edit

    button = QPushButton('Submit', self)
    button.setGeometry(210,10,100,40)
    button.setStyleSheet(
      "font-size: 25px;"
      "font-family:Arial;"
      "color: cyan;"
    )
    button.clicked.connect(self.submit)

  def submit(self):
    text = self.line_edit.text()
    print(f"input text: {text}")

```

#### css styles

```py
from PyQt5.QtWidgets import (QApplication, QMainWindow, QPushButton, QWidget, QHBoxLayout)

class MainWindow(QMainWindow):
  def __init__(self):
    super().__init__()

    central_widget = QWidget()
    self.setCentralWidget(central_widget)

    hbox = QHBoxLayout()

    for index in range(1, 4):
      button = QPushButton(f"#{index}")
      # 指定按鈕的 id，css 依靠這 id 做 style 的指定
      button.setObjectName(f"button{index}")
      hbox.addWidget(button)

    central_widget.setLayout(hbox)

    self.setStyleSheet("""
      QPushButton {
        font-size: 40px;
        margin: 25px;
        padding: 15px 60px;
        border: 3px solid;
        border-radius: 10px;
      }
      QPushButton#button1{
        background-color: hsl(0,100%,64%);
      }
      QPushButton#button2{
        background-color: hsl(122,100%,64%);
      }
      QPushButton#button3{
        background-color: hsl(204,100%,64%);
      }
      QPushButton#button1:hover{
        background-color: hsl(0,100%,84%);
      }
      QPushButton#button2:hover{
        background-color: hsl(122,100%,84%);
      }
      QPushButton#button3:hover{
        background-color: hsl(204,100%,84%);
      }
    """)
```
