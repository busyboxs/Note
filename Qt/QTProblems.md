# QT 使用中遇到的一些问题解决方案

## 如何获取 QPushButton 等控件的背景颜色

参考链接：[Get background color of QPushbutton](https://www.qtcentre.org/threads/57979-Get-background-color-of-QPushbutton)

对于 QPushButton 来说，首先需要设置以下属性

```cpp
setFlat(true);
setAutoFillBackground(true);
```

然后可以调用以下方法获取背景颜色

```cpp
QColor m_backgroundColor = this->palette().color(QPalette::Button);
QString colorStr = m_backgroundColor.name(QColor::HexArgb);

int r = m_backgroundColor.red();
int g = m_backgroundColor.green();
int b = m_backgroundColor.blue();
int a = m_backgroundColor.alpha();
```

## 自定义控件时如何使用 Q_PROPERTY

参考链接: [The Property System](https://doc.qt.io/qt-5/properties.html)

如果只是简单的定义一个属性，可以使用以下代码：

```cpp
class QMyButton : public QPushButton {
    Q_OBJECT
    Q_PROPERTY(QColor fillColor MEMBER m_FillColor)
    .....

private:
    QColor m_FillColor{ QColor(255, 255, 255) };
    .....
}
```

如果是像上面代码定义的属性有 MEMBER，则不需要显式定义 READ 和 WRITE 对应的函数。

----

另一种方式就是显式指定 READ 和 WRITE

```cpp
class QMyButton : public QPushButton {
    Q_OBJECT
    Q_PROPERTY(QColor fillColor READ getFillColor WRITE setFillColor)
    .....

public:
    QColor getFillColor() const { return m_FillColor; }
    void setFillColor(QColor color) { m_FillColor = color; }

private:
    QColor m_FillColor{ QColor(255, 255, 255) };
    .....
}
```

-----

待扩充

## 如何设置 QLabel 的字体渐变色

在 QSS 中设置渐变色通常是使用下面的语法

```css
background-color: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 1, stop:0 rgba(255, 255, 255, 1), stop:1 rgba(255, 255, 255, 0.5));
```

这样就会在 y 轴方向生成渐变色

但是使用以上的方式对 QLabel 的字体设置渐变色看起来并没有生效，需要使用以下的 qss 代码

```css
color: qlineargradient(x1: 0, y1: 0, x2: 0, y2: 60, stop:0 rgba(255, 255, 255, 1), stop:1 rgba(255, 255, 255, 0.5));
```

其中 y2 : 60 表示的是 QLabel 的高度，也可以设置超过高度的值。这只是控制颜色变化区间在 y1 ~ y2。

## 在自定义的控件中如何过滤或者传递一些事件

参考链接：[void QObject::installEventFilter(QObject *filterObj)](https://doc.qt.io/qt-5/qobject.html#installEventFilter)

在自定义的控件中，可能对于不同的事件，控件的表现形式或者状态会不一样，比如当按钮按下时是一种图标，没按下时是一种图标。对于这种需求可以使用 eventFilter 来实现。

```cpp
class KeyPressEater : public QObject
{
    Q_OBJECT
    ...

protected:
    bool eventFilter(QObject *obj, QEvent *event) override;
};

bool KeyPressEater::eventFilter(QObject *obj, QEvent *event)
{
    if (event->type() == QEvent::KeyPress) {
        QKeyEvent *keyEvent = static_cast<QKeyEvent *>(event);
        qDebug("Ate key press %d", keyEvent->key());
        return true;
    } else {
        // standard event processing
        return QObject::eventFilter(obj, event);
    }
}
```

如果需要过滤掉事件则对应的事件处理需要返回 true，要传递事件给其他部件则返回 false。

```cpp
KeyPressEater *keyPressEater = new KeyPressEater(this);
QPushButton *pushButton = new QPushButton(this);
QListView *listView = new QListView(this);

pushButton->installEventFilter(keyPressEater);
listView->installEventFilter(keyPressEater);
```