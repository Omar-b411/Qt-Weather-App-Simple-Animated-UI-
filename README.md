#include <QApplication>
#include <QWidget>
#include <QPainter>
#include <QTimer>
#include <QLineEdit>
#include <QPushButton>
#include <QLabel>

class Weather : public QWidget {
    int x = 0;
public:
    Weather() {
        resize(800,500);

        auto city = new QLineEdit(this);
        city->setGeometry(250,150,300,45);
        city->setPlaceholderText("Enter city");

        auto btn = new QPushButton("Get Weather", this);
        btn->setGeometry(250,210,300,45);

        auto label = new QLabel("22°C  ☁ Cloudy", this);
        label->setGeometry(280,300,250,50);

        setStyleSheet(
            "QLineEdit,QPushButton,QLabel{"
            "background:rgba(255,255,255,0.15);"
            "border-radius:15px;"
            "padding:10px;"
            "color:white;"
            "font-size:20px;}"
        );

        QTimer *t = new QTimer(this);

        connect(t,&QTimer::timeout,[=](){
            x += 3;
            if(x > width()) x = 0;
            update();
        });

        t->start(30);
    }

    void paintEvent(QPaintEvent*) {
        QPainter p(this);

        QLinearGradient g(0,0,width(),height());
        g.setColorAt(0, QColor(0,150,255));
        g.setColorAt(1, QColor(180,0,255));

        p.fillRect(rect(), g);

        p.setBrush(QColor(255,255,255,40));
        p.setPen(Qt::NoPen);

        p.drawEllipse(x-150,80,250,250);
        p.drawEllipse(width()-x,250,200,200);
    }
};

int main(int argc,char *argv[]) {
    QApplication a(argc,argv);
    Weather w;
    w.show();
    return a.exec();
}
