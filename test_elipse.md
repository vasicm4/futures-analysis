Posmatraju se parametri srednje vrednosti i kovarijansne matrice za e(t) i e(t+k) (posmatraju se po dva
odbirka kako bi smo ostali u dvodimenzionom prostoru zbog iscrtavanja elipse). Kovarijansna matrica
(c12 = c21) se može dekomponovati na proizvod matrice sačinjene od sopstvenih (kolona) vektora,
dijagonalne matrice sa sopstvenim vrednostima, i transponovane matrice od sopstvenih vektora :
𝐶(𝑒(𝑡), 𝑒(𝑡 + 𝑘)) = (𝑐1,1 𝑐1,2
𝑐2,1 𝑐2,2) = (𝑐𝑜𝑠(𝜃) −𝑠𝑖𝑛(𝜃)
𝑠𝑖𝑛(𝜃) 𝑐𝑜𝑠(𝜃) ) (𝜆1 0
0 𝜆2
) ( 𝑐𝑜𝑠(𝜃) 𝑠𝑖𝑛(𝜃)
−𝑠𝑖𝑛(𝜃) 𝑐𝑜𝑠(𝜃)) = 𝑄𝛬𝑄𝑇
Od ovako predstavljene matrica može se uzeti „koren“ po Ojlerovoj metodi, koristeći se osobinama:
𝑄𝑄𝑇 = 𝐼 ,
(𝜆1 0
0 𝜆2
)= (√𝜆1 0
0 √𝜆2
) (√𝜆1 0
0 √𝜆2
),
(𝜆1 0
0 𝜆2
)
−1
= (
1
𝜆1
0
0 1
𝜆2
)
−1
pa je
𝐶(𝑒(𝑡), 𝑒(𝑡 + 𝑘)) = 𝑄√𝛬𝑄𝑇𝑄√𝛬𝑄𝑇
Sada, posmatramo proizvod
[𝑋 − 𝐸(𝑒(𝑡))]𝐶−1[𝑋 − 𝐸(𝑒(𝑡))]𝑇
i posmatramo jedinični krug radijusa 1:
𝑦1
2 + 𝑦2
2 = 1
što bi smo u matričnom obliku zapisali kao:
𝑌𝑇𝑌 = 1
uvedemo smenu koordinata u matričnom obliku:
𝑍 = √𝛬𝑌 ⇔
√𝛬−1𝑍 = 𝑌 ⇔
1 = 𝑌𝑇𝑌 = 𝑍𝑇𝛬−1𝑍 ⇔
𝑧1
2
𝜆1
+ 𝑧2
2
𝜆2
= 1
Sada posmatramo novu smenu:
𝑈 = 𝑄𝑥𝑍 ⇔
𝑍 = 𝑄−1𝑥𝑈 = 𝑄𝑇𝑈 ⇔
𝑍𝑇𝛬−1𝑍 = 𝑈𝑇𝑄𝛬−1𝑄−1𝑈 = 𝑈𝑇𝑄𝛬−1𝑄𝑇𝑈
Nova smena:
𝑈 = 𝑋 − 𝐸(𝑒) ⇔
𝑈𝑇𝑄𝛬−1𝑄𝑇𝑈 = (𝑋 − 𝐸(𝑒))𝑇𝑄𝛬−1𝑄𝑇(𝑋 − 𝐸(𝑒))
Na taj način dolazimo do poslednje jednačine. Dakle, počev od kruga
𝑌𝑇𝑌 = 1
dobili smo elipsu:
(𝑋 − 𝐸(𝑒))𝑇𝑄𝛬−1𝑄𝑇(𝑋 − 𝐸(𝑒)) = 1
Elipsa ima centar u
(𝐸(𝑒(0)), 𝐸(𝑒(𝑘)))
ugao prema x osi je 𝜃 , dok su ose elipse 𝜆1, 𝜆2 .
Test elipse podrazumeva:
- da se nađu 𝜆1, 𝜆2 i 𝜃 na bazi sopstvenih vrednosti i sopstvenih vektora kovarijansne matrice
- da bi e(0) i e(k) mogli da se smatraki nekorelisanim (vrednosti 𝜆1, 𝜆2 treba da budu jednake), elipsa
treba da bude krug
- da bi srednja vrednost e(0) i e(k) bila nula, elipsa treba da bude u koordinatnom početku
