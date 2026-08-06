Analiza vremenskih serija finansijskih podataka jedan je od temelja finansijskog inženjeringa, gde je
potrebno da procenimo prediktivne stohastičke modele za pokretače tržišnog i kreditnog rizika kako
bismo mogli da predvidimo prinos buduć eg portfolija. Pored toga, kointegracija je ključni alat za
bavljenje uklapanjem i predviđanjem multivarijantnih vremenskih serija i nalazi se u osnovi dobro
poznatih trgovačkih signala za procenu optimalnih portfolio strategija.
Podaci za procenu:
Nedeljne cene od 1. januara 2013. do 1. januara 2023. za sledeć e (tikeri Yahoo Finance u zagradama):
- Fjučersi sirove nafte WTI („CL=F“) [W]
- Fjučersi sirove nafte BRENT („BZ=F“) [W]
- Cene akcija NVIDIA („NVDA“) [W]
Podaci za prognozu: Poslednjih 10 posmatranja nedeljnih cena fjučersa sirove nafte WTI od 5. marta
2025.
1. Preuzmite skupove podataka o nedeljnim cenama fjučersa WTI nafte, fjučersa BRENT sirove nafte i
akcija NVIDIA.
- Konvertujte vremenske serije u logaritamske cene.
- Nacrtajte vremenske serije logaritamskih cena i razlika logaritamskih cena.
- Da li se ponašaju kao slab beli šum (*)?
- Da li pokazuju vraćanje ka srednje vrednosti (**) ili stohastički trend(***)?
(*) Definicija slabo stacionarnog slučajnog procesa:
Slabo stacionaran slučajan proces je proces
- koji ima autokovarijacionu funkciju koja zavisi isključivo od rastojanja odbiraka a ne i od vremena
odabiranja
- čija je srednja vrednost konstantna
- čija je autokovarijaciona funkcija konstantna
(**) Definicija vraćanja ka srednjoj vrednosti:
Process koji se vraća ka srednjoj vrednosti ima osobine:
- ako je vrednost u nekom trenutku veća od srednje vrednosti, očekivana promena je manja od 0
- ako je vrednost u nekom trenutku manja od srednje vrednosti, očekivana promena je veća od 0
(***)
Definicija stohastičkog trenda:
Process koji poseduje stohastički trend ima osobinu da je razlika n-tog reda (n >= 1) neka konstanta +
šum koji nema srednu vrednost, nije korelisan sa sobom (razlika prvog reda je proces koji nastaje
kreiranjem razlike susednih odbiraka, drugog reda razlikom odbiraka koji su razlika prvog reda itd.).
Ovaj proces nema definisanu konstantnu srednju vrednost a varijansa sa vremenom raste.
2. Izvršite pojednostavljeni test jediničnog korena uklapanjem AR(1) modela (*) i proverom
rezultujuć eg parametra i vremena .
Ovi testovi ć e nam pomoć i da utvrdimo da li se serija može smatrati kovarijansno stacionarnom, dakle
ona koja se vrać a na srednju vrednost, ili ne i kovarijansno stacionarnom sa stohastičkim trendom.
Definicija AR(1) modela (*):
Ovaj process ima oblik
𝑋𝑡+1 = 𝑎 + 𝑏𝑋𝑡 + 𝑒𝑡
gde su a i b realne konstante a e(t) process nulte srednje vrednosti koji nije auto korelisan i konstantne
srednje varijanse.
Za vrednosti |b|<1, proces je stacionaran.
Za vrednost b=1, proces je tzv. random walk
Za vrednost |b|>1, proces divergira
3. Izvršite eliptični test (*) na logaritamskim cenama i logaritamskim razlikama, na susednim
odbircima reziduala e(t) i e(t+k), za k = 1, 2, ...n .
Definicija eliptičnog testa (*)
Data je u posebnom fajlu “test elipse.docx”.
4. Pošto AR(1) test za razlike logaritama odbiraka pokaže da su vrednosti za b koeficijent blizu 1,
proverićemo da li su slučajni procesi tipa
𝑋𝑡 = 𝑋𝑡−1 + 𝑒𝑡 (tzv. Random Walk model)
gde je 𝑒𝑡 nekorelisani Gausovski slučajni proces. Rezultat ć e biti da Gausova kriva ne odgovara e(t)
odbircima.
Nakon toga, pretpostavivši da je to model, izvršite simulaciju Monte Carlo metodom za predviđanje
procesa razlika logaritama cena i proces logaritma cene za 1, 2, … , 52 odbirka u budućnost. Rezultat ć
e biti da simulacije ne odgovaraju realnoj realizaciji slučajnog procesa.
5. Izvršiti simulaciju za iz tačke 4 za identifikovan AR(1) model.
Treba uočiti da kako vrednost koeficijenta b čini da se proces vraća ka srednjoj vrednosti (u našem
slučaju, kada je b blisko jedinici, to vraćanje srednjoj vrednosti ide sporo). Ipak, na većem horizontu
predikcije, vidi se razlika AR(1) i Random Walk modela.
6. Identifikovati parametre GARCH(1, 1) modela (*) na seriju nastalu oduzimanjem susednih odbiraka
logaritmovane cene.
Definicija GARCH(1, 1) modela:
𝑋𝑡+1 = 𝑎 + 𝑏𝑋𝑡 + 𝜖𝑡
𝜖𝑡 = 𝜎𝑡𝑧𝑡
𝜎𝑡
2 = 𝛼 + 𝛽𝜎𝑡−1
2 + 𝛾𝜖𝑡−1
2
𝑧𝑡 je slučajan process srednje vrednosti 0 i varijanse 1 sa međusobno nezavisnim odbircima
Parametri GARCH modela se identifikuju tako što se prvo identifikuju parametri AR modela za 𝑋𝑡 i
pod pretpostavkom da je 𝜖𝑡 nekorelisana slučajna sekvenca, nastavlja se sa identifikacijom modela
varijanse.
Identifikacija kvadrata varijanse vrši se tako što se na bazi identifikovanih parametara 𝑎, 𝑏 od AR(1)
modela nađu reziduali, pa se podignu na kvadrat. Zatim se nad njima sprovodi AR(1) identifikacija,
kako bi se pronašli parametri 𝛼, 𝛽 .
Nakon dobijanja parametara sigma modela, potrebno je vratiti se u prethodnu tačku i ponovo izračunati
parametre AR modela za X(t) (sada je to složenija procedura jer ne može da se radi jednostavnom
metodom najmanjih kvadrata već generalizovanom metodom najmanjih kvadrata).
Ovo se naziva dvo-koračnom GARCH korekcijom.
