Analiza vremenskih serija finansijskih podataka jedan je od temelja finansijskog inženjeringa, gde je
potrebno da procenimo prediktivne stohastičke modele za pokretače tržišnog i kreditnog rizika kako
bismo mogli da predvidimo kretanje cena u budućnosti (odnosno stanje portfolija sa modelovanim
instrumentima)
Podaci za procenu:
Nedeljne cene od 1. januara 2013. do 1. januara 2023. za sledeće (tikeri Yahoo Finance u zagradama):
- Fjučersi sirove nafte WTI („CL=F“) [W]
- Fjučersi sirove nafte BRENT („BZ=F“) [W]
- Cene akcija NVIDIA („NVDA“) [W]
Podaci za prognozu: Poslednjih 10 posmatranja nedeljnih cena fjučersa sirove nafte WTI od 5. marta
2025.
1. Preuzmite skupove podataka o dnevnim cenama fjučersa WTI nafte, fjučersa BRENT sirove nafte i
akcija NVIDIA i prebacite učitane odbirke (koji su na dnevnom periodom odabiranja) na nedeljni
period odabiranja
- Konvertujte vremenske serije u logaritamske cene.
- Nacrtajte vremenske serije logaritamskih cena i razlika logaritamskih cena.
2. Izvršite identifikaciju parametara AR(1) modela (*) na logaritamske serije i serije razlika
logaritamskih cena.
Definicija AR(1) modela (*):
Ovaj process ima oblik
𝑋𝑡+1=𝑎+𝑏𝑋𝑡+𝑒𝑡
gde su a i b realne konstante a e(t) process nulte srednje vrednosti koji nije auto korelisan i konstantne
srednje varijanse.
Za vrednosti |b|<1, proces je stacionaran.
Za vrednost b=1, proces je tzv. random walk
Za vrednost |b|>1, proces divergira
Za ovu tačku je bitno razumeti da ukoliko je u dobijenom AR modelu vrednost koeficijenta b blizu
broja 1, moguće je da je u pitanju zapravo suštinski drugačiji proces:
𝑋𝑡+1=𝑎+𝑋𝑡+𝑒𝑡
odnosno da važi
𝑋𝑡+1-𝑋𝑡=𝑎+𝑒𝑡
Niz vrednosti 𝑒𝑡 se naziva rezidual.
U slučaju modelovanja logaritma cene, ispostaviće se da je AR(1) model takav da koeficijent b blizu
broja 1.
U slučaju modelovanja razlika logaritama cena, dobićemo model kod koga koeficijent b nije blizu broja
1.
Značenje parametra b vidi se iz jednačine:
𝐸(𝑋𝑡+1 − 𝑎)=𝑏𝐸(𝑋𝑡) ⇔ 𝐸(𝑋𝑡+𝑘 − 𝑎) = 𝑏𝑘𝐸(𝑋𝑡) + 𝑎(𝑏+. . . +𝑏𝑘−1)
Od interesa je vreme (tj. broj k) za koje vrednost 𝑏𝑘 postane 0.5 . Za svaku od vremenskih serija treba
da naći posmatrano k za koje 𝑏𝑘 postaje 0.5 .
3. Sada se fokusiramo na AR model nad razlikama logaritama cena.
Izvršite eliptični test (*) logaritamskim razlikama, na susednim odbircima reziduala e(t) i e(t+1) tako
što ćete odvojiti odbirke reziduala u dve grupe – one sa parnim i one sa neparnim indexima. Za naš test
su to sada realizacije slučajnog procesa sa dve dimenzije. Posmatramo ocene očekivanja ta dva slučajna
procesa i njihove kovarijansne matrice.
Test elipse se sastoji u traženju sopstvenih vrednosti matrice kovarijanse te „dve“ slučajne promenljive.
Nacrta se elipsa čiji je centar definisan ocenom očekivanja, ugao definisan sopstvenim vektorima, a ose
sopstvenim vrednostima. Preko tako dobijene elipse nacrtajte realizacije ove naše
Definicija eliptičnog testa (*)
Data je u posebnom fajlu “test elipse.docx”.