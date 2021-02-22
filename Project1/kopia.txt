#include<iostream>
#include<vector>
#include<cstdlib>
#include<windows.h>
#include<string>
#include<fstream>
using namespace std;

void console_size()
{
	HWND console = GetConsoleWindow();
	RECT r;
	GetWindowRect(console, &r); //stores the console's current dimensions
	MoveWindow(console, r.left, r.top, 800, 800, TRUE); // 800 width, 100 height
}

//-------------------------------Klasy----------------------------------------

class Produkt {
public:
	virtual string opis() = 0;
	virtual float cena() = 0; //--------nowo dopisane
	virtual string opis_plik() = 0; //--------nowo dopisane
	Produkt() {}
	~Produkt() {}
};
class Dysk :public Produkt {
	int pojemnosc;
	float cenac = 0;
public:
	string opis() { return "Dysk, Pojemnosc: " + to_string(pojemnosc) + ", Cena za sztuke: " + to_string(cenac) + " zl"; };
	string opis_plik() { return "Dysk " + to_string(pojemnosc) + " " + to_string(cenac); };
	Dysk(int pojemnosc, float cenac) :pojemnosc(pojemnosc), cenac(cenac) {}
	float cena()
	{
		return cenac;
	};
};
class Monitor :public Produkt {
	double przekatna;
	float cenac = 0;
public:
	string opis() { return "Monitor, Przekatna: " + to_string(przekatna) + ", Cena za sztuke: " + to_string(cenac) + " zl"; }
	string opis_plik() { return "Monitor " + to_string(przekatna) + " " + to_string(cenac); };
	Monitor(double przekatna, float cenac) :przekatna(przekatna), cenac(cenac) {}
	float cena()
	{
		return cenac;
	};
};
class Klawiatura :public Produkt {
	string typ;
	float cenac = 0;
public:
	string opis() { return "Klawiatura, Typ: " + typ + ", Cena za sztuke: " + to_string(cenac) + " zl"; }
	string opis_plik() { return "Klawiatura " + typ + " " + to_string(cenac); };
	Klawiatura(string typ, float cenac) : typ(typ), cenac(cenac) {}
	float cena()
	{
		return cenac;
	};
};
class Myszka :public Produkt {
	double dpi;
	float cenac = 0;
public:
	string opis() { return "Myszka, DPI: " + to_string(dpi) + ", Cena za sztuke: " + to_string(cenac) + " zl"; }
	string opis_plik() { return "Myszka " + to_string(dpi) + " " + to_string(cenac); };
	Myszka(double dpi, float cenac) :dpi(dpi), cenac(cenac) {}
	float cena()
	{
		return cenac;
	};
};
class Magazyn {
	vector<pair<Produkt*, int>> produkty;
public:
	void dodaj(Produkt* produkt, int ilosc) {
		produkty.push_back(make_pair(produkt, ilosc));
	}
	void pokaz() {
		int id = 1;
		cout << endl << endl << "_____________ Stan magazynu ______________________________________________" << endl;
		for (auto& p : produkty) {
			cout << endl << id++ << ": " << p.first->opis() << ", sztuk: " << p.second;
		}
	}
	Produkt* pobierz(int id, int ile) {
		id--;
		if (ile <= produkty[id].second) {
			produkty[id].second -= ile;
			return produkty[id].first;
		}
		else return nullptr;
	}
	int zwroc_ilosc()
	{
		for (auto &p : produkty)
		{
			return p.second;
		}
	}
	vector<pair<Produkt*, int>> produkty_zwroc()
	{
		return produkty;
	}
	;
};
class PozycjaZamowienia {
	Produkt* produkt;
	int ilosc;
	float cena = 0;
public:
	float cenaf()
	{
		cena = produkt->cena() * ilosc;
		return cena;
	}
	float cena_zamf()
	{
		float cena_zam = 0;
		cena_zam += cenaf();
		return cena_zam;
	}
	string opis() {
		return produkt->opis() + ", ilosc: " + to_string(ilosc) + ", cena razem " + to_string(cenaf());
	}
	string opis_dopliku() {
		return produkt->opis_plik() + " " + to_string(ilosc) + " " + to_string(cenaf()) + " " + to_string(ilosc);
	}
	

	PozycjaZamowienia(Produkt* produkt, int ilosc) :produkt(produkt), ilosc(ilosc) {}
};
class Zamowienie {
	friend ostream& operator<<(ostream& ob, Zamowienie& o);
public:
	vector<PozycjaZamowienia*> pz;
	float lacznie = 0;
	float rozlacznie = 0;
	void dodaj(Produkt* produkt, int ilosc) {
		pz.push_back(new PozycjaZamowienia(produkt, ilosc));
	}

	void drukuj() {
		cout << endl << "_____________ Zamowienie: _______________________________________ " << endl;
		int nr = 1;
		for (auto& p : pz)
		{
			cout << endl << nr++ << ": " << p->opis();
			lacznie = lacznie + p->cena_zamf();
		}
		cout << endl << endl << "Cena zamowienia: " << lacznie << endl;
	}
	float zwroc_ceneZam()
	{
		for (auto& p : pz)
		{
			rozlacznie = rozlacznie + p->cena_zamf();
		}
		return rozlacznie;
	}
	vector<PozycjaZamowienia*>* zwrot()
	{
		return &pz;
	}
};
class KlienciIndywidualni
{
	string imie;
	string nazwisko;
	string adres;
	string email;
	string data;
public:
	KlienciIndywidualni() {};
	void dodaj()
	{
		cout << endl << "Podaj dane do faktury: " << endl << endl;
		cout << "Imie: ";
		cin >> imie;
		cout << endl << "Nazwisko: ";
		cin >> nazwisko;
		cout << endl << "Adres zamieszkania (Miejscowosc[spacja]Ulica[spacja]NumerBudynku/Domu): ";
		cin >> adres;
		cin.ignore(100, '\n');
		cout << endl << "Adres email: ";
		cin >> email;
	}
	string fun()
	{
		return "Faktura wystawiona na: " + imie + " "+ nazwisko + " zamieszkaly/a w: " + adres + " o emailu: " + email;
	}
	string nazwa()
	{
		return imie + nazwisko;
	}

};
class KlienciFirmowi
{
	string imie;
	string nazwisko;
	string adres;
	string nazwa_firmy;
	string email;
	string NIP;
public:
	KlienciFirmowi() {};
	void dodaj()
	{
		cout << endl << "Podaj dane do faktury: " << endl << endl;
		cout << "Imie: ";
		cin >> imie;
		cout << endl << "Nazwisko: ";
		cin >> nazwisko;
		cout << endl << "Adres zamieszkania (Miejscowosc[spacja]Ulica[spacja]NumerBudynku/Domu): ";
		cin >> adres;
		cout << endl << "Nazwa firmy: ";
		cin >> nazwa_firmy;
		cin.ignore(100, '\n');
		cout << endl << "Adres email: ";
		cin >> email;
		cout << endl << "NIP: ";
		cin >> NIP;
	}
	string fun()
	{
		return "Faktura wystawiona na firme: " + imie + " " + nazwisko + " zamieszkaly/a w: " + adres + " o emailu: " + email + " z NIP-em: " + NIP + " nazwa Firmy: " + nazwa_firmy;
	}
	string nazwa_firmy_fun()
	{
		return nazwa_firmy;
	}
};
class ZbiorKlienciIndywidualni
{
public:
	vector<Zamowienie*> clients;
	vector<KlienciIndywidualni*> kklienci;

	void dodaj_zamowienia(Zamowienie* zamow)
	{
		clients.push_back(zamow);
	}
	void dodaj_klienta(KlienciIndywidualni* klient)
	{
		kklienci.push_back(klient);
	}
	void pokazklientow()
	{
		for (auto& p : kklienci)
		{
			cout << endl << p->fun();
		}
	}
};
class ZbiorKlienciFirmowi
{
	vector<Zamowienie*> clients;
	vector<KlienciFirmowi*> kklienci;
public:
	void dodaj_zamowienia(Zamowienie* zamow)
	{
		clients.push_back(zamow);
	}
	void dodaj_klienta(KlienciFirmowi* klient)
	{
		kklienci.push_back(klient);
	}
	void zmien_dane(KlienciFirmowi* klient)
	{
		for (auto& p : kklienci)
		{
			p = klient;
		}
	}
	void pokazklientow()
	{
		for (auto& p : kklienci)
		{
			cout << endl << p->fun();
		}
	}
};
ostream& operator<<(ostream& ob, PozycjaZamowienia& o)
{
	ob << o.opis();
	return ob;
}

//----------------------------------------------------------------------------

int main() 
{

	console_size();

	int ilosc_wybranych_produktow = 0;
	int numer_porzadkowy = 1;
	int wybor_schematu = 1;
	char wybor_zmiany = '0';
	string ilosc_w_magazynie;
	int ilosc_w_magazynie_int = 0;

	//-------------------------------Dodawanie i wyswietlanie zawartosci magazynu

	Magazyn magazyn;

	ifstream zp;
	zp.open("magazyn.txt", ios_base::in);
	if (zp.good())
	{
		getline(zp, ilosc_w_magazynie);
		ilosc_w_magazynie_int = stoi(ilosc_w_magazynie);
		magazyn.dodaj(new Dysk(256, 120), ilosc_w_magazynie_int);
		getline(zp, ilosc_w_magazynie);
		ilosc_w_magazynie_int = stoi(ilosc_w_magazynie);
		magazyn.dodaj(new Monitor(24, 850), ilosc_w_magazynie_int);
		getline(zp, ilosc_w_magazynie);
		ilosc_w_magazynie_int = stoi(ilosc_w_magazynie);
		magazyn.dodaj(new Monitor(27, 1200), ilosc_w_magazynie_int);
		getline(zp, ilosc_w_magazynie);
		ilosc_w_magazynie_int = stoi(ilosc_w_magazynie);
		magazyn.dodaj(new Klawiatura("mechaniczna", 228), ilosc_w_magazynie_int);
		getline(zp, ilosc_w_magazynie);
		ilosc_w_magazynie_int = stoi(ilosc_w_magazynie);
		magazyn.dodaj(new Myszka(3400, 78), ilosc_w_magazynie_int);
	}
	zp.close();
	magazyn.pokaz();
	
	//-------------------------------Tworzenie zamowienia------------------------

	cout << endl << endl << "_____________ Stworz zamowienie: ________________________________" << endl << endl;
	Zamowienie zamowienie;
	cout << "1. Zamowienie dla osoby prywatnej" << endl;
	cout << "2. Zamowienie dla firmy" << endl;
	cin >> wybor_schematu;
	if (wybor_schematu == 1)
	{
		ZbiorKlienciIndywidualni zbk1;
		KlienciIndywidualni ki1;
		ki1.dodaj();
		cout << endl << "Wskaz produkt i ilosc (lub wpisz 'z' zeby zakonczyc): ";
		while (cin >> numer_porzadkowy >> ilosc_wybranych_produktow)
		{
			Produkt* produkt = magazyn.pobierz(numer_porzadkowy, ilosc_wybranych_produktow);
			if (produkt)
			{
				//int bleble = zamowienie;
				zamowienie.dodaj(produkt, ilosc_wybranych_produktow);
				zbk1.dodaj_zamowienia(&zamowienie);
				//bleble = 
				cout << "Do zamowienia dodano produkt " << produkt->opis() << endl;
			}
			else
			{
				cout << endl << "Nie mozna dodac produktu do zamowienia";
				cout << endl << "Wskaz produkt i ilosc (lub wpisz 'z' zeby zakonczyc): ";
			}
		}
		vector<pair<Produkt*, int>> produkty_main = magazyn.produkty_zwroc();
		ofstream zapp;
		zapp.open("magazyn.txt", ios_base::out);
		if (zapp.good())
		{
			for (auto &p: produkty_main)
			{
				zapp << p.second << endl;
			}
			
		}
		zapp.close();
		ofstream dopliku;
		string nazwa_pliku = ki1.nazwa();
		dopliku.open(nazwa_pliku + ".txt", ios_base::app);
		vector<PozycjaZamowienia*> container1 = *zamowienie.zwrot();
		for (auto& p : container1)
		{
			dopliku << p->opis_dopliku() + "\n";
		}
		
		dopliku << zamowienie.zwroc_ceneZam();
		zbk1.dodaj_klienta(&ki1);
		system("cls");
		zamowienie.drukuj();
		zbk1.pokazklientow();
		magazyn.pokaz();
		dopliku.close();
		container1.clear();
	}
	else
	{
		ZbiorKlienciFirmowi zkf1;
		KlienciFirmowi kf1;
		kf1.dodaj();
		ofstream dopliku;
		string nazwa_pliku = kf1.nazwa_firmy_fun();
		dopliku.open(nazwa_pliku + ".txt", ios_base::app);
		cout << endl << "Wskaz produkt i ilosc (lub wpisz 'z' zeby zakonczyc): ";
		while (cin >> numer_porzadkowy >> ilosc_wybranych_produktow)
		{
			Produkt* produkt = magazyn.pobierz(numer_porzadkowy, ilosc_wybranych_produktow);
			if (produkt)
			{
				zamowienie.dodaj(produkt, ilosc_wybranych_produktow);
				zkf1.dodaj_zamowienia(&zamowienie);
				cout << "Do zamowienia dodano produkt " << produkt->opis() << endl;
			}
			else
			{
				cout << endl << "Nie mozna dodac produktu do zamowienia";
				cout << endl << "Wskaz produkt i ilosc (lub wpisz 'z' zeby zakonczyc): ";
			}
		}
		vector<PozycjaZamowienia*> container1 = *zamowienie.zwrot();
		for (auto& p : container1)
		{
			dopliku << p->opis_dopliku() + "\n";
		}
		dopliku << zamowienie.zwroc_ceneZam();
		zkf1.dodaj_klienta(&kf1);
		system("cls");
		zamowienie.drukuj();
		zkf1.pokazklientow();
		magazyn.pokaz();
		container1.clear();
	}
	return 0;
	cin.get();
}

