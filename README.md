# Academy_Website
Pravimo sajt fiktivne akademske ustanove
ACCADEMY WEBSITE
Ideja je da se pokrije vecina potrebnih funkcionalnosti za funkcionusanje jedne akademije.
Osnovna stvar su nam User I Role menadzment. Sta to znaci. Svaki od korisnika aplikacije ima odredjena prava koja su mu setovana na osnovu role u kojoj se nalazi
-	Role entitet (ID, Name, Description, Users (lista user-a), IsDeleted). Name Admin user moze da dodaje ili brise ostale role. 
o	Sto ce reci da je admin rola pre-definisana I ne moze se obrisati. Administrator, takodje, void racuna o kursevima, trenerima, potvrdjivanju registracije studenata, brisanju istih 
o	Trainer (Lead Trainer, Trainer, Assistent). Lead Trainer dodeljuje trenere  odredjenim kursevima I asestente trenerima. Trener moze da zahteva odredjenog asistenta za svoj predmet. Lead trener treba da odobri ili odbije ovakav zahtev.
o	Asistent ne moze da menja kurseve
o	Student moze biti registovan, ali je potrebno da njegovu registraciju odobri Admin. Posto je potvrdjena njegova rola, student moze da pregleda kurseve ili kupi neki od njih.
-	User (ID, FirstName, LastName, RegistrationDate, IsActive, Email, Avatar (image), Address). Vodite racuna da svaki od korisnika, bez obzira na rolu mora biti ulogovan da bi mogao da nastavi sa radom. (Moze biti Interfejs. Takodje, Address moze bitii interfejs)
-	Course entitet (ID, Title, Description, CourseType, Category, Difficulty (novi entiteti), Duration, StartDate, EndDate, IsActive, Location, Materials, Price). Imajte u vidu da kurs moze imati 3 tipa (online, u ucionici, download kursa), zbog toga bih ja uveo vezu Location (ID, Title, Address, City, CountryID). Kako bismo omogucili pretragu po zemlji u kojoj zelimo da pohadjamo kurs, napravicemo entitet Country (ID, Name, Code, IsAvailable). Takodje, svaki kurs moze imati listu materijala vezanih za njega (MaterialID, Title, Url)
-	CourseType (ID, Title (Online, In-house, Downloadable) ).
-	Category (ID, Name, IsActive) – (C#, JavaScript…..)
-	Difficulty (basic, intermediate, addvanced) - enum
-	Prilikom kupovine nekog kursa treba proveriti da li je ukljucen neki Discount I naravno, treba omoguciti studentu, da prilikom kupovine, doda postojeci DiscountCode. Discount (ID, UsePromoCode, DiscountPercetage (0-100), DiscountValue) 
-	Na kraju kupovine, student treba omoguciti izbor valute za placanje. Obrtite paznju da administratoru treba dati mogucnost promene CurrencyRate-a. Imajte u vidu da postoji mogucnost download-a kursa nakon kupovine. Currency (ID, Title, Currency, CurrencyRate). Admin moze da menja currency rate.
-	Napomenuli smo da trener treba imati dva moguca statusa, tj da li je Lead ili ne. Na osnovu toga moze ostavljati zabeleske o nekom treneru ili asistentu. Te zabeleske treba cuvati kao kolekciju entiteta Notes (ID, Note, TrainerFromID, TrainerToID)
-	Administrator I Finance rola mogu pregledati listu kupljenih kurseva. Dati mogucnost finansijskoj sluzbi da filtrira listu po kursu I promeni cenu kursa. Samo administrator moze da promeni trajanje kursa.
-	Za svaki kurs omoguciti pisanje evauacija. Naime, evaluacije mogu biti u tri smera: 
TrainerToTrainer, TrainerToStudent, StudentToTrainer, Evaluacija jos treba da sadrzi Text I ocenu (1-5).
-	Na kraju svakog kursa treba testirati studente. Napravite entitet Exam koji ce imati svoj ID, traajanje, listu pitanja, te propery Active. 
-	Svaki Exam cuvati u listi CompletedExams (ExamID, StudentID, Answers, SumPoints), kao I flag Passed/NotPassed.
-	Jasno, svaki Exam se sastoji od liste pitanja (entitet Question). Taj entitet treba da ima svoje pitanje, odgovor, broj poena I naravno flag IsActive.
-	Program treba da sadrzi I svoj finansijski deo. Osobe sa rolom Finance mogu da pogledaju listu svih kupljenih kurseva, filtriraju listu po student, imenu kursa… takodje, omoguciti finansijskoj sluzbi Refund opciju tj. povracaj novca studentu. Vodite racuna o listi kupljenih kurseva. Takodje, omogucite finansijskoj sluzbi da ima uvid u ukupnu sumu zaradjenu od prodaje kurseva. Pored toga, obavezno je definisanje plata trenera I asistenata.
-	Sa druge strane, treneri I asistenti mogu da vide svoju platu.

U nastavku dajem pregled osnovnih i manje bitnih klasa, kao i njihove propertije, metode, kolekcije, enume i ostale članove i polja.Sve je prevedeno na srpski i molim da se u nastavku rada pridrzavamo termina koji su dati u pregledu.


Uloga 
 
Propertiji:
-Šifra
-Naziv (Admin, Trener, GlavniTrener, Asistent, Student, Finansije)

Kolekcije:
-Korisnici koji pripadaju svakoj ulozi

!Flag(bool) jeObrisana

Metode:
*Daje razlicita ovlascenja korisnicima sa razlicitim ulogama

Korisnik

Propertiji:
-Sifra
-Ime
-Prezime
-DatumRegistracije
-Email
-Avatar(slika)
-Uloga UlogaKorisnika
-Adresa (interfejs)
-KorisnickoIme
-Lozinka

!Flag(bool) jeAktivan

Metodi:
*Loguje se preko KorisnickogImena i Lozinke

Trener(Korisnik)

Propertiji:
-Plata

Kolekcije:
 -Lista kurseva koje predaje
 -Lista Asistenata koji su mu dodeljeni
- Lista Zabeleški (Ostavljene, Upucene)

Zabeleške
-ŠifraZabeleške
-TekstZabeleške
-ŠifraTreneraKojiJeOstavioZabelešku
-ŠifraTreneraKomeJeUpucenaZabeleška

 ListaSvihZabeleški

Metodi
*Menja duzinu kurseva
*Trazi asistenta
*Pravi Zabeleške

GlavniTrener(Trener-Korisnik)

Metodi:
*Odobrava ili odbija asistenta treneru

Asistent (Trener-Korisnik)


Student(Korisnik)

Metodi:
*Vidi, filtrira i pretrazuje kurseve
*Kupuje kurseve (ovo azurira ListuKupljenihKurseva)
*Dobija Popust
*Menja Valutu u kojoj hoce da plati

Adminstrator(Korisnik)
Metodi:
*Dodaje i briše Uloge Korisnicima
*Vidi filtrira i pretrazuje ListuKupljenihKurseva
*Menja Kurs Valute
*Menja trajanje Kursa
*Odobrava regitraciju Korisnicima

Finansije(Korisnik)

Metodi:

*Menja cenu Kursa
*Vidi, filtrira i pretrazuje ListuKupljenihKurseva
*Refundira ovo azurira ListuKupljenihKurseva
*Vidi sumu prodatih kurseva
*Menja platu trenerima i assitentima

Kurs

Propertiji:
-Šifra
-NazivKursa
-Opis
-DatumPocetka
-DatumKraja
Enum Tezina kursa (pocetni, prelazni, napredni)

!Flag(bool) jeAktivan

Kolekcije
Lista?  Kurseva
ListaKupljenihKurseva

LokacijaKursa
-	sifraLokacije
-	nazivLokacije
-	Adresa
-	Grad
-	sifraDrzave
Drzava
-	sifraDrzave
-	NazivDrzave
-	PozivniBrojDrzave
-	!Flag(bool) jeDostupna
TipKursa 
 -sifraTipaKursa
- enum nazivTipaKursa(onlajn, NaLokaciji, Dounload)
KategorijaKursa
 -sifraKategorijeKursa
-nazivKategorijeKursa
- !bool jeAktivan

Materijali
-	sifraMaterijala
-	nazivMaterijala
-	urlMaterijala


Cena
-Popust 
-sifraPopusta
-IskoristitiPromoKod???
-ProcenatPopusta(0-100)
-IznosPopusta
ListaPopusta

-Valuta
- sifraValute
- nazivValute
- KursValute
Lista Valuta

Ispit:
 -sifraIspita
 -nazivIspita
 -Trajanje

Kolekcije
-Lista Pitanja
-ListaIspita

Pitanje

!Flag(bool) Aktivan

UrađenIspit (Ispit)
-ŠifraUrađenogispita
-ŠifraStudentaKojiRadioIspit
Lista Odgovora
!Flag(bool) Položen

Odgovor

Evaluacija
 Enum SmerEvaluacije (TrenerTreneru, StudentTreneru, TrenerStudentu)
-	TekstEvaluacije
Enum OcenaEvaluacije (1-5)


Moj predlog podele posla:

Tim Aleksandra/Nikola radi Uloge, Korisnika, Korisnika Studenta, Korisnika Finasije i Admina

Tim Vlada/Aleksandar radi Trenera i njegove poduloge (Glavni trener i Asistent)

Tim Milutin/Djole radi Kurs i sve vezano za njega.

