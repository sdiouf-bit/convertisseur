# convertisseur mafenetre.cpp
#include "mafenetre.h"
#include "ui_mafenetre.h"
#include "QDialog"
#include "QPushButton"
#include "QHBoxLayout"
Mafenetre::Mafenetre( QWidget *parent ) : QDialog( parent )
{
// 1. Instancier les widgets
    valeur = new QLineEdit(this);
    resultat = new QLabel(this);
    unite = new QLabel(this);
    choixConversion = new QComboBox(this);
    bConvertir = new QPushButton(QString::fromUtf8("Convertir"), this);
    bQuitter = new QPushButton(QString::fromUtf8("Quitter"), this);
// 2. Personnaliser les widgets
    valeur->setStyleSheet("color: #0a214c; background-color: #C19A6B;");
    valeur->clear();
    QFont font("Liberation Sans", 12, QFont::Bold);
    choixConversion->setFont(font);
    choixConversion->addItem(QString::fromUtf8("Decimal -> Binaire"));
    choixConversion->addItem(QString::fromUtf8("Binaire -> Decimal"));
    choixConversion->addItem(QString::fromUtf8("Decimal -> Hexa"));
    resultat->setStyleSheet("color: #0a214c;");
    unite->setStyleSheet("color: #0a214c;");
// 3. Instancier les layouts
    QHBoxLayout *hLayout1 = new QHBoxLayout;
    QHBoxLayout *hLayout2 = new QHBoxLayout;
    QVBoxLayout *mainLayout = new QVBoxLayout;
// 4. Positionner les widgets dans les layouts
    hLayout1->addWidget(valeur);
    hLayout1->addWidget(choixConversion);
    hLayout1->addWidget(resultat);
    hLayout1->addWidget(unite);
    hLayout2->addWidget(bConvertir);
    hLayout2->addWidget(bQuitter);
    mainLayout->addLayout(hLayout1);
    mainLayout->addLayout(hLayout2);
    setLayout(mainLayout);
// 5. Connecter les signaux et slots
    connect(bConvertir, SIGNAL(clicked()), this, SLOT(convertir()));
    connect(this, SIGNAL(actualiser()), this, SLOT(convertir()));
    connect(choixConversion, SIGNAL(currentIndexChanged(int)), this,SLOT(permuter()));
    connect(bQuitter, SIGNAL(clicked()), qApp, SLOT(quit()));
    // bonus : conversion automatique
    connect(valeur, SIGNAL(textChanged(const QString &)), this, SLOT(convertir()));
// 6. Personnaliser la fenêtre
    setWindowTitle(QString::fromUtf8("Convertisseur de températures bineaire"));
    adjustSize();
    // on lance une conversion
    //convertir();
    // ou :
    emit actualiser();
}
// 7. Définir les slots
void Mafenetre::convertir()
{
    QString value = valeur->text();
    if (value.isEmpty())
    {
        resultat->setText(QString::fromUtf8("--.--"));
        void afficherUnit();
        return;
    }
    switch (choixConversion->currentIndex())
    {
        case DECIMAL_BINAIRE:
        {
        bool True;
        int decimal = valeur->text().toInt(&True, 10);


            QString binaire = QString::number(decimal, 2);
            resultat->setText(binaire);

        break;
        }
        case BINAIRE_DECIMAL:
    {
        bool True;
        int decimal = valeur->text().toInt(&True, 2);

            resultat->setText(QString::number(decimal));


        break;
    }
    case DECIMAL_HEXA:
{
    bool True;
    int decimal = valeur->text().toInt(&True, 10);


        QString hexvalue = QString("%1").arg(decimal, 8, 16, QLatin1Char( '0' ));
        resultat->setText(hexvalue);

    break;
}

    }
}


void Mafenetre::permuter()
{
    if(resultat->text() != "--.--")
    {
        valeur->setText(resultat->text());
        emit actualiser();
    }
    void afficherUnit();
}


// 8. Définir les méthodes
void Mafenetre::afficherUnite()
{
switch (choixConversion->currentIndex())
{
case DECIMAL_BINAIRE:
unite->setText(QString::fromUtf8(" "));
break;
case BINAIRE_DECIMAL:
unite->setText(QString::fromUtf8(" "));
break;

}
}
mafenetre.h
#if QT_VERSION >= 0x050000
#include <QtWidgets> /* tous les widgets de Qt5 */
#else
#include <QtGui> /* tous les widgets de Qt4 */
#include "QDialog"
#include "QLineEdit"
#include "QLabel"
#include "QComboBox"
#define DECIMAL_BINAIRE 0
#define BINAIRE_DECIMAL  1
#define DECIMAL_HEXA 2
#endif
class Mafenetre : public QDialog
{
Q_OBJECT
public:
Mafenetre( QWidget *parent=0 );
private:
void afficherUnite();
// les widgets pour la temperature
QLineEdit *valeur; //
QLabel *resultat; // afficher les resultats
QLabel *unite;
QComboBox *choixConversion;
QPushButton *bConvertir;
QPushButton *bQuitter;
QDoubleValidator *doubleValidator;
// les widgets pour le bineaire
signals:
void actualiser();
private slots:
void convertir();
void permuter();
public slots:
};




