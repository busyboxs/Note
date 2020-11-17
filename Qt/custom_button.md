# 自定义 PushButton

对于原生的 PushButton，当使用透明的 border 时，会出现边框出线白点的问题，因此这里需要通过继承 QPushButton 重新实现 paintEvent 来实现。以下代码不仅实现了透明边框，同时还实现了进度条的展示。

```cpp
// QRoundButton.h
#pragma once

#include <QPushButton>

/*
QRoundButton 中定义了四个属性，这些属性的值可以在 qss 文件中进行设置，例如：

QPushButton[whatsThis="btnButton"]
{
    qproperty-borderColor: rgba(255, 255, 255, 0.3);
    qproperty-fillColor: rgba(255, 0, 0, 0.3);
    qproperty-cornerRadius: 26;
    qproperty-borderSize: 2;
}
*/
class QRoundButton : public QPushButton
{
    Q_OBJECT
    Q_PROPERTY(QColor borderColor READ getBorderColor WRITE setBorderColor)
    Q_PROPERTY(QColor fillColor READ getFillColor WRITE setFillColor)
    Q_PROPERTY(int cornerRadius READ getCornerRadius WRITE setCornerRadius)
    Q_PROPERTY(int borderSize READ getBorderSize WRITE setBorderSize)

public:
    QRoundButton(QWidget* parent = Q_NULLPTR, int nBorderSize = 2);
    ~QRoundButton();

    void setBorderColor(const QColor& color) { m_BorderColor = color; }
    QColor getBorderColor() const { return m_BorderColor; };

    void setFillColor(const QColor& color) { m_FillColor = color; }
    QColor getFillColor() const { return m_FillColor; }

    void setBorderSize(const int size) { m_nBorderSize = size; }
    int getBorderSize() const { return m_nBorderSize; };

    void setProgress(qreal progress) { m_fProgress = progress; }
    qreal getProgress() { return m_fProgress; }

    void setCornerRadius(int radius) { m_nRadius = radius; }
    int getCornerRadius() const { return m_nRadius; }

    QPainterPath getPathLeft(qreal x);
    QPainterPath getPathCenter(qreal x);
    QPainterPath getPathRight(qreal x);

protected:
    void paintEvent(QPaintEvent* e) override;
    void timerEvent(QTimerEvent* e) override;

private:
    QColor m_BorderColor;
    QColor m_FillColor;
    int    m_nBorderSize;
    qreal  m_fProgress;
    int    m_nTimerId;
    int    m_nRadius;
};
```

```cpp
//QRoundButton.cpp
#include "QRoundButton.h"
#include <QResource>
#include <QVBoxLayout>
#include <QDebug>
#include <QPushButton>
#include <QtSvg/QSvgWidget>
#include <QTextEdit>
#include <QFontMetrics>
#include <QApplication>
#include <QTextDocument>
#include <QTextFrame>
#include <QPainter>
#include <QStyleOption>
#include <QDateTime>
#include <QTimer>
#include <QtMath>
#include <QPainterPath>

QRoundButton::QRoundButton(QWidget* parent, int nBorderSize)
    : QPushButton(parent),
    m_BorderColor(QColor(255, 255, 255)),
    m_FillColor(QColor(0, 255, 0, 127)),   // 默认填充颜色为半透明绿色
    m_nBorderSize(nBorderSize),
    m_fProgress(0),
    m_nTimerId(0),
    m_nRadius(0)
{
    if (m_nTimerId == 0)
    {
        m_nTimerId = this->startTimer(50, Qt::PreciseTimer);
    }
}

QRoundButton::~QRoundButton()
{
    if (m_nTimerId)
    {
        this->killTimer(m_nTimerId);
        m_nTimerId = 0;
    }
}

QPainterPath QRoundButton::getPathLeft(qreal x)
{
    QPainterPath path;
    qreal r = m_nRadius;
    qreal y = qSqrt(r * r - (r - x) * (r - x));
    qreal theta = qFabs(qAcos((r - x) / r)) / M_PI * 180;
    QRectF innerRect = rect();
    innerRect.adjust(m_nBorderSize, m_nBorderSize, -m_nBorderSize, 0);
    path.moveTo(innerRect.x() + x, innerRect.y() + (r - y));
    path.lineTo(innerRect.x() + x, innerRect.height() - 2 * (r - y) + 2);
    path.arcTo(innerRect.x(), innerRect.height() - 2 * r, r * 2, r * 2, -(180 - theta), -theta);
    path.lineTo(innerRect.x(), innerRect.y() + r);
    path.arcTo(innerRect.x(), innerRect.y(), 2 * r, 2 * r, -180, -theta);
    return path;
}

QPainterPath QRoundButton::getPathCenter(qreal x)
{
    QPainterPath path;
    qreal r = m_nRadius;
    qreal y = r;
    QRectF innerRect = rect();
    innerRect.adjust(m_nBorderSize, m_nBorderSize, -m_nBorderSize, 0);
    path.moveTo(innerRect.x() + r, innerRect.y());
    path.lineTo(innerRect.x() + x, innerRect.y());
    path.lineTo(innerRect.x() + x, innerRect.height());
    path.lineTo(innerRect.x() + r, innerRect.height());
    path.arcTo(innerRect.x(), innerRect.height() - 2 * r, r * 2, r * 2, -90, -90);
    path.lineTo(innerRect.x(), innerRect.y() + r);
    path.arcTo(innerRect.x(), innerRect.y(), 2 * r, 2 * r, -180, -90);
    return path;
}

QPainterPath QRoundButton::getPathRight(qreal x)
{
    QRectF innerRect = rect();
    innerRect.adjust(m_nBorderSize, m_nBorderSize, -m_nBorderSize, 0);
    QPainterPath path;
    qreal r = m_nRadius;
    qreal w = innerRect.width();
    qreal xdot = r - w + x;
    qreal y = qSqrt(r * r - xdot * xdot);
    qreal theta = qFabs(qAcos(xdot / r)) / M_PI * 180;
    
    path.moveTo(innerRect.x() + r, innerRect.y());
    path.lineTo(innerRect.x() + w - r, innerRect.y());
    path.arcTo(innerRect.x() + w - 2 * r, innerRect.y(), 2 * r, 2 * r, 90, -(90 - theta));
    path.lineTo(innerRect.x() + x, innerRect.height() - 2 * (r - y));
    path.arcTo(innerRect.x() + w - 2 * r, innerRect.height() - 2 * r, 2 * r, 2 * r, -theta, -(90 - theta));
    path.lineTo(innerRect.x() + r, innerRect.height());

    path.arcTo(innerRect.x(), innerRect.height() - 2 * r, r * 2, r * 2, -90, -90);
    path.lineTo(innerRect.x(), innerRect.y() + r);
    path.arcTo(innerRect.x(), innerRect.y(), 2 * r, 2 * r, -180, -90);
    return path;
}

void QRoundButton::paintEvent(QPaintEvent* e)
{
    /*QStyleOption opt;
    opt.init(this);
    QPainter painter(this);
    painter.setRenderHint(QPainter::Antialiasing);
    style()->drawPrimitive(QStyle::PE_Widget, &opt, &painter, this);
    painter.drawText(rect(), Qt::AlignCenter, text());*/


    QStyleOptionButton option;
    option.initFrom(this);
    if (isDown())
    {
        option.state = QStyle::State_Sunken;  // 当按钮按下时，状态切换为 Sunken
    }
    if (isDefault())
        option.features |= QStyleOptionButton::DefaultButton;
    option.text = text();
    option.icon = icon();

    QPainter painter(this);
    painter.setRenderHint(QPainter::Antialiasing);   // 抗锯齿
    style()->drawControl(QStyle::CE_PushButton, &option, &painter, this);

    painter.save();
    QRectF borderRect = rect();
    int changeSize = m_nBorderSize / 2;
    borderRect.adjust(changeSize, changeSize, -changeSize, -changeSize);
    m_nRadius = min(m_nRadius, (borderRect.height() - m_nBorderSize) / 2);  // 限制圆角半径不超过内矩形高度的一半（通常认为宽大于高）
    QPen penBorder(m_BorderColor, m_nBorderSize);
    painter.setBrush(Qt::NoBrush);
    painter.setPen(penBorder);
    painter.drawRoundedRect(borderRect, m_nRadius, m_nRadius);
    painter.restore();

    // 画进度条
    /*painter.save();
    QRectF innerRect = rect();
    innerRect.adjust(m_nBorderSize, m_nBorderSize, -m_nBorderSize, -m_nBorderSize);
    qreal x = m_fProgress * innerRect.width();
    QPainterPath path;
    if (x < m_nRadius)
    {
        path = getPathLeft(x);
    }
    else if (x < innerRect.width() - m_nRadius)
    {
        path = getPathCenter(x);
    }
    else if (x < innerRect.width())
    {
        path = getPathRight(x);
    }
    else
    {
        path.addRoundedRect(innerRect, m_nRadius, m_nRadius);
    }
    painter.fillPath(path, QColor(255, 0, 0, 200));
    painter.restore();*/

    painter.save();
    QRectF innerRect = rect();
    innerRect.adjust(m_nBorderSize, m_nBorderSize, -m_nBorderSize, -m_nBorderSize);
    qreal x = m_fProgress * innerRect.width();

    innerRect.adjust(0, 0, -(innerRect.width() - x), 0);
    QPainterPath rectPath;
    rectPath.addRect(innerRect);

    innerRect = rect();
    innerRect.adjust(m_nBorderSize, m_nBorderSize, -m_nBorderSize, -m_nBorderSize);
    innerRect.adjust(0, 0, -max((innerRect.width() - x - m_nRadius), 0), 0);
    QPainterPath roundRectPath;
    roundRectPath.addRoundedRect(innerRect, 26, 26);
    QPainterPath inner = roundRectPath.intersected(rectPath);
    painter.fillPath(inner, m_FillColor);
    painter.restore();

}

void QRoundButton::timerEvent(QTimerEvent* e)
{
    if (e->timerId() == m_nTimerId)
    {
        if (auto p = m_fProgress + 0.01; p <= 1.01)
        {
            setProgress(p);
        }
    }
    update();
}

```

在 main 函数中使用

```cpp
QRoundButton* button = new QRoundButton(btnSubWidget);
button->setFixedSize(334, 56);
button->setWhatsThis("btnButton");
button->setText(qtTrId(UPDATEBTNCHECKUPDATE));
connect(button, SIGNAL(clicked()), this, SLOT(onCheck()));
```

qss 中的样式表

```css
QPushButton[whatsThis="btnButton"]
{
    color: white;
    background-color: rgba(255, 255, 255, 0.3);
    border-width: 2px;
    border-radius: 26px;
    font: 22px "Microsoft Yahei";
    qproperty-borderColor: rgba(255, 255, 255, 0.3);
    qproperty-fillColor: rgba(255, 0, 0, 0.3);
    qproperty-cornerRadius: 26;
    qproperty-borderSize: 2;
}

QPushButton:hover[whatsThis="btnButton"]
{
    border: 2px solid rgba(255, 255, 255, 1);
    background-color: rgba(255, 255, 255, 0.4);
}

QPushButton:pressed[whatsThis="btnButton"]
{
    border: 2px solid rgba(255, 255, 255, 1);
    background-color: rgba(255, 255, 255, 0.5);
}
```