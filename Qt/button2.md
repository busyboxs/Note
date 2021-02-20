```cpp
#pragma once

#include <QPushButton>

class QRoundButton : public QPushButton
{
    Q_OBJECT
    //Q_PROPERTY(QColor borderColor MEMBER m_BorderColor)
    Q_PROPERTY(QColor borderColor READ getBorderColor WRITE setBorderColor)
    Q_PROPERTY(QColor fillColor MEMBER m_FillColor)
    Q_PROPERTY(QColor borderColorHover MEMBER m_BorderColorHover)
    Q_PROPERTY(qreal opacity READ getOpacity WRITE setOpacity)
    Q_PROPERTY(qreal opacityHover MEMBER m_OpacityHover)
    Q_PROPERTY(int cornerRadius MEMBER m_nRadius)
    Q_PROPERTY(int borderSize MEMBER m_nBorderSize)


public:
    QRoundButton(QWidget *parent = Q_NULLPTR, int nBorderSize = 2);
    ~QRoundButton();

    void setProgress(qreal progress) { m_fProgress = progress; }
    void setIsRun(bool isRun);

    void setBorderColor(QColor color) { m_BorderColor = color; m_VBorderColor = color; }
    QColor getBorderColor() const { return m_BorderColor; }

    void setOpacity(qreal opacity) { m_Opacity = opacity; m_VOpacity = opacity; }
    qreal getOpacity() const { return m_Opacity; }

    void getPathLeft(qreal x, QPainterPath& fillPath, QPainterPath& borderPath);
    void getPathCenter(qreal x, QPainterPath& fillPath, QPainterPath& borderPath);
    void getPathRight(qreal x, QPainterPath& fillPath, QPainterPath& borderPath);

protected:
    void paintEvent(QPaintEvent* e) override;
    void timerEvent(QTimerEvent* e) override;
    bool eventFilter(QObject* obj, QEvent* e) override;

private:
    QColor     m_VBorderColor{ QColor(255, 255, 255, 0.1) };   // 用于存储按钮非进度条模式下的背景色
    QColor     m_BorderColor{ QColor(255, 255, 255) };         // 按钮边框颜色
    QColor     m_BorderColorHover{ QColor(255, 255, 255) };    // hover 状态下按钮边框颜色
    QColor     m_FillColor{ QColor(0, 255, 0, 127) };          // 进度填充颜色
    int64_t    m_nBorderSize{};                                // 按钮边框尺寸
    qreal      m_fProgress{ 0 };                               // 进度 0. ~ 1.
    qreal      m_VOpacity;                                     // 存储当前使用的透明度
    qreal      m_Opacity{ 1 };                                 // 按钮的透明度
    qreal      m_OpacityHover{ 1 };                            // 鼠标 hover 状态下的透明度
    int        m_nTimerId{ 0 };                                // 
    int64_t    m_nRadius{ 0 };                                 // 按钮边框圆角半径
    bool       m_bIsRun{ false };                              // 是否为进度条模式
    QString    m_style{};
};

```

```cpp
#include "stdafx.h"
#include "QRoundButton.h"
#include <QPainter>
#include <QStyleOptionButton>
#include <QTimerEvent>
#include <QtMath>
#include <windows.h>

QRoundButton::QRoundButton(QWidget *parent, int nBorderSize /*= 2*/)
    : QPushButton(parent), m_nBorderSize(nBorderSize)
{
    m_VOpacity = m_Opacity;
    setAttribute(Qt::WA_Hover);  // 当鼠标 Hover 时会触发 paintEvent
    //setAttribute(Qt::WA_TranslucentBackground);
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

void QRoundButton::setIsRun(bool isRun)
{
    m_bIsRun = isRun;
    if (isRun)
    {
        m_style = this->styleSheet();
        setStyleSheet("background-color: transparent;");
    }
    else
    {
        // 当按钮没有进度条时，需要设置进度为0，此时 background-color 会作为控件背景色
        setProgress(0);
        setStyleSheet(m_style);
    }
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

    painter.save();
    painter.setOpacity(m_VOpacity);
    // https://doc.qt.io/qt-5/qstyle.html#ControlElement-enum
    //style()->drawControl(QStyle::CE_PushButton, &option, &painter, this);
    style()->drawControl(QStyle::CE_PushButtonBevel, &option, &painter, this);
    style()->drawPrimitive(QStyle::PE_FrameFocusRect, &option, &painter, this);
    painter.restore();

    painter.save();
    style()->drawControl(QStyle::CE_PushButtonLabel, &option, &painter, this);
    painter.restore();

    painter.save();
    QRectF borderRect = rect();
    int changeSize = m_nBorderSize / 2;
    borderRect.adjust(changeSize, changeSize, -changeSize, -changeSize);
    //m_nRadius = min(m_nRadius, (borderRect.height() - m_nBorderSize) / 2);  // 限制圆角半径不超过内矩形高度的一半（通常认为宽大于高）

    QPen penBorder(m_VBorderColor, m_nBorderSize);
    painter.setOpacity(m_VOpacity);
    painter.setBrush(Qt::NoBrush);
    painter.setPen(penBorder);
    painter.drawRoundedRect(borderRect, m_nRadius, m_nRadius);
    painter.restore();

    painter.save();
    QRectF innerRect = rect();
    qreal x = m_fProgress * innerRect.width();
    innerRect.setRight(x);
    QPainterPath rectPath;
    rectPath.addRect(innerRect);

    QPainterPath roundRectPath;
    roundRectPath.addRoundedRect(rect(), m_nRadius, m_nRadius);
    QPainterPath inner = roundRectPath.intersected(rectPath);

    painter.fillPath(inner, m_FillColor);
    painter.restore();

    //painter.save();
    //QRectF innerRect_ = rect();
    //innerRect_.adjust(m_nBorderSize / 2, m_nBorderSize / 2, -m_nBorderSize / 2, -m_nBorderSize / 2);
    //qreal x_ = m_fProgress * innerRect_.width();
    //QPainterPath fPath;
    //QPainterPath bPath;
    //if (qFuzzyCompare(1 + x_, 1 + 0.0))  // == 0
    //{
    //    fPath = QPainterPath();
    //    bPath = QPainterPath();
    //}
    //else if (x_ < m_nRadius)
    //{
    //    getPathLeft(x_, fPath, bPath);
    //}
    //else if (x_ < innerRect_.width() - m_nRadius)
    //{
    //    getPathCenter(x_, fPath, bPath);
    //}
    //else if (x_ < innerRect_.width())
    //{
    //    getPathRight(x_, fPath, bPath);
    //}
    //else
    //{
    //    innerRect_ = rect();
    //    innerRect_.adjust(m_nBorderSize / 2, m_nBorderSize / 2, -m_nBorderSize / 2, -m_nBorderSize / 2);
    //    fPath.addRoundedRect(innerRect_, m_nRadius, m_nRadius);
    //    bPath = fPath;
    //}

    ////painter.fillPath(fPath, m_FillColor);
    //painter.setPen(QPen(m_FillColor, m_nBorderSize / 2));
    //painter.drawPath(fPath);
    ////painter.setPen(QPen(QColor(255, 255, 255, 120), m_nBorderSize));
    //painter.setPen(QPen(m_FillColor, m_nBorderSize));
    //painter.drawPath(bPath);
    //painter.restore();
}

void QRoundButton::getPathLeft(qreal x, QPainterPath& fillPath, QPainterPath& borderPath)
{
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

void QRoundButton::timerEvent(QTimerEvent* e)
{
    /*if (e->timerId() == m_nTimerId)
    {
        if (auto p = m_fProgress + 0.01; p <= 1.01)
        {
            setProgress(p);
        }
    }
    update();*/
}

bool QRoundButton::eventFilter(QObject* obj, QEvent* e)
{
    switch (e->type())
    {
    case QEvent::HoverEnter:
        m_VBorderColor = m_BorderColorHover;
        m_VOpacity = m_OpacityHover;
        return false;
        break;
    case QEvent::HoverLeave:
        m_VBorderColor = m_BorderColor;
        m_VOpacity = m_Opacity;
        return false;
        break;

    case QEvent::MouseButtonPress:
        m_VBorderColor = m_BorderColor;
        m_VOpacity = m_Opacity;
        return false;
        break;

    case QEvent::MouseButtonRelease:
        return QObject::eventFilter(obj, e);
        break;
    default:
        return QObject::eventFilter(obj, e);
        break;
    }
}

```

```css
QPushButton[whatsThis="progressButton"]
{
    color: rgba(255, 255, 255, 1);
    background-color: rgba(255, 255, 255, 0.7);
    border-radius: 27px;
    padding: 3px;
    font: 22px "Microsoft Yahei";
    qproperty-borderColor: rgba(255, 255, 255, 1);
    qproperty-borderColorHover: rgba(255, 255, 255, 0.6);
    qproperty-fillColor: rgba(255, 255, 255, 0.3);
    qproperty-cornerRadius: 27;
    qproperty-borderSize: 2;
    qproperty-opacity: 0.3;
    qproperty-opacityHover: 1;
    outline: none;
}

QPushButton:hover[whatsThis="progressButton"]
{
    color: rgba(255, 255, 255, 1);
    background-color: rgba(255, 255, 255, 0.3);
}

QPushButton:pressed[whatsThis="progressButton"]
{
    color: rgba(255, 255, 255, 1);
    background-color: rgba(255, 255, 255, 0.7);
}
```