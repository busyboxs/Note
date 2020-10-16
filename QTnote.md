# QT 学习笔记

## QT 特性

* 可移植性
* 易用性
* 运行速度快

## QT 基础

* QWidget, QDialog
* 连接信号与槽 or 信号与信号 (`connect()`)
* 断开信号与槽 (`disconnect`)

## QT 部件

* 滚动条(`QScrollView` & `QScrollBar`)


## QT 类

* QObject:
  * Q_OBJECT:
  * Function:
    * `parent()`
    * `findChild()`
    * `findChildren()`
    * `setParent()`
    * `blockSignals(bool block)`
    * `connect(...)` : 
      * 信号和槽参数通过 `SIGNAL()` 和 `SLOT()` 宏来指定，并且信号函数和槽函数的参数不能包括变量名，只需要提供类型
      * 信号也可以和信号进行连接
      * 一个信号可以和多个槽和信号连接，多个信号可以连接到一个槽
      * 当一个信号连接多个槽时，槽的激活顺序与连接顺序相同
    * `connectNotify()`
    * `customEvent()`
    * `deleteLater()` -- [slot]
    * `destroyed()` -- [signal]
    * `disconnect()`
      * 断开对象信号的所有连接 `disconnect(myObject, 0, 0, 0);`
      * 断开指定信号的所有连接 `disconnect(myObject, SIGNAL(mySignal()), 0, 0);`
      * 断开与指定对象的连接 `disconnect(myObject, 0, myReceiver, 0);`
    * `disconnectNotify()`
    * `dumpObjectInfo()`
    * `dumpObjectTree()`
    * `event(e)`
    * `eventFilter()`
    * `inherits()` -> bool
    * `isSignalConnected()`
    * `isWidgetType()`
    * `isWindowType()`
    * `Q_DISABLE_COPY`

* Signals & Slots
  * 定义信号需要在类中添加 `signals:`
  * 定义槽需要在类中添加 `public slots:`

* QFrame
  * `QFrame`(base) -> `QWidgets`(inherit)
  * `setFrameStyle()`, `frameStyle()`
  * frame **shapes**: `NoFrame`, `Box`(边框), `Panel`(平面), `StyledPanel`, `HLine`, `VLine`
  * **shadow** styles: `Plain`(平的), `Raised`(凸出的), `Sunken`(凹陷的)
  * thickness of the border: `lineWidth`, `midLineWidth`, `frameWidth`
  * `frameRect()`, `setFrameRect()`
  * `frameShadow()`, `setFrameShadow()`
  * `frameShape()`, `setFrameShape()`
  * `frameWidth()`, `lineWidth()`, `midLineWidth()`
  * `changeEvent()`, `event()`, `paintEvent()`
  * `sizeHint`

* QWidget
  * `QWidget` 类时所有用户交互对象的基类
  * Events:
    * `paintEvent()`
    * `resizeEvent()`
    * `mousePressEvent()`
    * `mouseReleaseEvent()`
    * `mouseDoubleClickEvent()`
    * `keyPressEvent()`
    * `focusInEvent()`
    * `focusOutEvent()`
    * `mouseMoveEvent()`
    * `keyReleaseEvent()`
    * `wheelEvent()`
    * `enterEvent()`
    * `leaveEvent()`
    * `moveEvent()`
    * `closeEvent()`
  
* 顺序容器类
  * `QList`: 
    * 对于数据类型大于指针存的或者是不可移动的类型，QList 实现时为 `QList<T*>`
    * 在前或后添加数据非常快
    * QList 的很多操作都要求列表为非空，所以一般在执行其他操作之前调用 `isEmpty()`
    * 操作：`insert()`, `replace()`, `removeAt()`, `move()`, `swap()`, `append()`, `prepend()`, `removeFirst()`, `removeLast()`, `takeFirst()`, `takeLast()`, `takeAt()`, `indexOf()`, `lastIndexOf()`, `contains()`, `count()`, `replace()`
    * 下标索引 `[]`和 `at()` 函数，`at()` 速度更快，因为不会造成深拷贝
    * `isEmpty()` 确认是否为空
    * `size()` 返回数据项个数
    * 可以使用初始化列表进行初始化，`QList<QString> list = {"one", "two", "three"};`
    * `<<` 操作符允许快速添加多个元素到列表中，`list << "four" << "five";`
    * type: `iterator`, `const_iterator`, `const_reference`, `const_reverse_iterator`, `difference_type`, `pointer`, `reference`, `reverse_iterator`, `size_type`, `value_type`
    * `append()` 函数既可以添加单个元素，也可以添加一个列表
    * `back()` 和 `last()` 函数可以获取最后一个元素，要求列表为非空
    * `clear` 删除所有的元素
    * `contains(value)`: 判读列表中是否含有 value 元素
    * `count(value)`: 返回列表中 value 发生的次数
    * `count()`: 返回列表中元素的个数
    * `empty()`: 判断列表是否为空，等效于 `isEmpty()`
    * `endsWith(value)`: 判断列表最后一个元素是否是为 value 元素
    * `erase(pos)`: 删除 pos 迭代器指向的元素，并返回一个指向下一个元素的迭代器
    * `fromSet(set)`: 从 QSet 创建 QList
    * `fromStdList(list)`: 从标准库 list 创建 QList
    * `fromVector(vector)`: 从 QVector 创建 QList
    * `front()`: 等效于 `first()`, 返回第一个元素的引用
    * `indexOf(value, from)`: 返回 value 第一次出现的索引。如果没找到，返回 `-1`
    * `insert(i, value)`: 在 i 位置处插入 value 元素
    * `isEmpty()`: 判断列表是否为空
    * `lastIndexOf(value, from = -1)`: 返回 value 反向第一次出现的所以，如果没有，返回 `-1`
    * `length()`: 返回列表长度，等效于 `size()`, `count()`
    * `mid(pos, length)`: 返回 pos 到 length 个长度的列表
    * `move(from, to)`: 移动 from 位置的元素到 to 位置
    * `pop_back()`: 删除最后一个元素，等同与 `removeLast()`
    * `prepend(value)`: 在列表头部添加元素，等同于 `push_front(value)`
    * `push_back(value)`: 在列表后添加元素，等同于 `append(value)`
    * `push_front(value)`: 在列表头部添加元素，等同于 `prepend(value)`
    * `removeAll(value)`: 删除列表中所有的 value 元素
    * `removeAt(i)`: 删除 i 位置的元素
    * `removeFirst()`: 删除列表中第一个元素
    * `removeLast()`: 删除列表中的最后一个元素
    * `removeOne(value)`: 删除第一个 value 元素
    * `replace(i, value)`: 替换第 i 个位置的元素为 value
    * `reserve(alloc)`: 保留 alloc 个空间
    * `size()`: 返回列表中元素的个数
    * `startWith(vlaue)`: 判断列表第一个元素是否为 value
    * `swap(other)`: 将列表和 other 列表进行交换
    * `swapItemsAt(i, j)`: 交换列表中 i 位置和 j 位置的元素
    * `takeAt(i)`: 删除列表 i 位置的元素，并返回该元素 
    * `takeFirst()`: 删除第一个元素，并返回该元素
    * `takeLast()`: 删除最后一个元素并返回该元素
    * `toSet()`: QList 转换为 QSet
    * `toStdList()`: QList 转换为 list
    * `toVector()`: QList 转换为 QVector
    * `value(i)`: 获取列表中 i 位置的值
    * 迭代器：`begin()`, `end()`, `cbegin()`, `cend()`, `constBegin()`, `constEnd()`, `crbegin()`, `crend()`, `rbegin()`, `rend()`
    * 引用：`constFirst()`, `constLast()`, `first()`, `last()`
    * 运算符： `!=`, `+`, `+=`, `<<`, `==`, `[]`

  * `QLinkedList`
    * 链式列表，数据项不是连续存储的
  * `QVector`
  * `QStack`
  * `QQueue`