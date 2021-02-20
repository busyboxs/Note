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
    Q_PROPERTY(QColor borderColor MEMBER m_BorderColor)
    Q_PROPERTY(QColor fillColor MEMBER m_FillColor)
    Q_PROPERTY(int cornerRadius MEMBER m_nRadius)
    Q_PROPERTY(int borderSize MEMBER m_nBorderSize)

public:
    QRoundButton(QWidget *parent = Q_NULLPTR, int nBorderSize = 2);
    ~QRoundButton();

    void setProgress(qreal progress) { m_fProgress = progress; }

    void getPathLeft(qreal x, QPainterPath& fillPath, QPainterPath& borderPath);
    void getPathCenter(qreal x, QPainterPath& fillPath, QPainterPath& borderPath);
    void getPathRight(qreal x, QPainterPath& fillPath, QPainterPath& borderPath);

protected:
    void paintEvent(QPaintEvent* e) override;
    void timerEvent(QTimerEvent* e) override;

private:
    QColor     m_BorderColor{ QColor(255, 255, 255) };
    QColor     m_FillColor{ QColor(0, 255, 0, 127) };
    int64_t    m_nBorderSize{};
    qreal      m_fProgress{ 0 };
    int        m_nTimerId{ 0 };
    int64_t    m_nRadius{ 0 };
};
```

```cpp
//QRoundButton.cpp
#include "QRoundButton.h"
#include <QPainter>
#include <QStyleOptionButton>
#include <QTimerEvent>
#include <QtMath>
#include <windows.h>

QRoundButton::QRoundButton(QWidget *parent, int nBorderSize /*= 2*/)
    : QPushButton(parent), m_nBorderSize(nBorderSize)
{
    if (m_nTimerId == 0)
    {
        m_nTimerId = this->startTimer(100, Qt::PreciseTimer);
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

void QRoundButton::getPathLeft(qreal x, QPainterPath& fillPath, QPainterPath& borderPath)
{
    if (qFuzzyCompare(x + 1.0, 1.0))
    {
        fillPath = QPainterPath();
        borderPath = QPainterPath();
    }

    QPainterPath path;
    qreal r = m_nRadius;
    qreal y = qSqrt(r * r - (r - x) * (r - x));
    qreal theta = qFabs(qAcos((r - x) / r)) / M_PI * 180;
    QRectF innerRect = rect();
    innerRect.adjust(m_nBorderSize / 2, m_nBorderSize / 2, -m_nBorderSize / 2, -m_nBorderSize / 2);

    path.moveTo(innerRect.x() + x, innerRect.y() + innerRect.height() - (r - y));
    path.arcTo(innerRect.x(), innerRect.y() + innerRect.height() - 2 * r, r * 2, r * 2, -(180 - theta), -theta);
    path.lineTo(innerRect.x(), innerRect.y() + r);
    path.arcTo(innerRect.x(), innerRect.y(), 2 * r, 2 * r, -180, -theta);

    borderPath = path;

    path.lineTo(innerRect.x() + x, innerRect.y() + innerRect.height() - (r - y));
    fillPath = path;
}

void QRoundButton::getPathCenter(qreal x, QPainterPath& fillPath, QPainterPath& borderPath)
{
    QPainterPath path;
    qreal r = m_nRadius;
    QRectF innerRect = rect();
    innerRect.adjust(m_nBorderSize / 2, m_nBorderSize / 2, -m_nBorderSize / 2, -m_nBorderSize / 2);
    path.moveTo(innerRect.x() + x, innerRect.y() + innerRect.height());
    path.lineTo(innerRect.x() + r, innerRect.y() + innerRect.height());
    path.arcTo(innerRect.x(), innerRect.y() + innerRect.height() - 2 * r, r * 2, r * 2, -90, -90);
    path.lineTo(innerRect.x(), innerRect.y() + r);
    path.arcTo(innerRect.x(), innerRect.y(), 2 * r, 2 * r, -180, -90);
    path.lineTo(innerRect.x() + x, innerRect.y());

    borderPath = path;
    path.lineTo(innerRect.x() + x, innerRect.y() + innerRect.height());
    fillPath = path;
}

void QRoundButton::getPathRight(qreal x, QPainterPath& fillPath, QPainterPath& borderPath)
{
    QRectF innerRect = rect();
    innerRect.adjust(m_nBorderSize / 2, m_nBorderSize / 2, -m_nBorderSize / 2, -m_nBorderSize / 2);
    QPainterPath path;
    qreal r = m_nRadius;
    qreal w = innerRect.width();
    qreal xdot = r - w + x;
    qreal y = qSqrt(r * r - xdot * xdot);
    qreal theta = qFabs(qAcos(xdot / r)) / M_PI * 180;

    path.moveTo(innerRect.x() + x, innerRect.y() + innerRect.height() - (r - y));
    path.arcTo(innerRect.x() + w - 2 * r, innerRect.y() + innerRect.height() - 2 * r, 2 * r, 2 * r, -theta, -(90 - theta));
    path.lineTo(innerRect.x() + r, innerRect.y() + innerRect.height());

    path.arcTo(innerRect.x(), innerRect.y() + innerRect.height() - 2 * r, r * 2, r * 2, -90, -90);
    path.lineTo(innerRect.x(), innerRect.y() + r);
    path.arcTo(innerRect.x(), innerRect.y(), 2 * r, 2 * r, -180, -90);
    path.lineTo(innerRect.x() + w - r, innerRect.y());
    path.arcTo(innerRect.x() + w - 2 * r, innerRect.y(), 2 * r, 2 * r, 90, -(90 - theta));
    borderPath = path;
    path.lineTo(innerRect.x() + x, innerRect.y() + innerRect.height() - (r - y));
    fillPath = path;
}


void QRoundButton::paintEvent(QPaintEvent* e)
{
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
    painter.save();
    QRectF innerRect = rect();
    innerRect.adjust(m_nBorderSize, m_nBorderSize, -m_nBorderSize, -m_nBorderSize);
    qreal x = m_fProgress * innerRect.width();
    QPainterPath fPath;
    QPainterPath bPath;
    if (x < m_nRadius)
    {
        getPathLeft(x, fPath, bPath);
    }
    else if (x < innerRect.width() - m_nRadius)
    {
        getPathCenter(x, fPath, bPath);
    }
    else if (x < innerRect.width())
    {
        getPathRight(x, fPath, bPath);
    }
    else
    {
        innerRect = rect();
        innerRect.adjust(m_nBorderSize / 2, m_nBorderSize / 2, -m_nBorderSize / 2, -m_nBorderSize / 2);
        fPath.addRoundedRect(innerRect, m_nRadius, m_nRadius);
        bPath = fPath;
    }
    painter.fillPath(fPath, QColor(255, 0, 0, 20));
    painter.setPen(QPen(QColor(0, 255, 0), 2));
    painter.drawPath(bPath);
    painter.restore();

    /*painter.save();
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
    roundRectPath.addRoundedRect(innerRect, m_nRadius - m_nBorderSize, m_nRadius - m_nBorderSize);
    QPainterPath inner = roundRectPath.intersected(rectPath);
    QPen p(QColor(255, 255, 255, 175), m_nBorderSize);
    painter.fillPath(inner, m_FillColor);

    painter.restore();*/
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