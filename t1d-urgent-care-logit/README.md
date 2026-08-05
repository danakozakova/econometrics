# Determinanty urgentných návštev pri diabete 1. typu — LOGIT model

> 🇬🇧 *Econometric analysis of factors driving emergency healthcare visits (severe hypoglycemia / DKA) among type-1 diabetes patients, using T1D Exchange registry data (USA, 2016, n = 2 454). Binary LOGIT model with collinearity diagnostics (VIF, BKW) and a probit comparison, built in gretl. **The data is not redistributable** — see Data access. Write-up in Slovak.*

Projekt zo Základov ekonometrie (Masarykova univerzita). Skúmam, ktoré faktory ovplyvňujú pravdepodobnosť **urgentnej návštevy zdravotníckeho zariadenia** u pacientov s diabetom 1. typu — konkrétne kvôli ťažkej hypoglykémii alebo diabetickej ketoacidóze (DKA). Dáta pochádzajú z amerického registra **T1D Exchange** (2 454 pacientov, rok 2016).

## Metóda

Binárna vysvetľovaná premenná `VISIT` (návšteva kvôli hypo/DKA za posledné 3 mesiace) je modelovaná **logistickou regresiou**. Finálna špecifikácia je výsledkom postupného výberu prediktorov — otestovaných bolo vyše 70 alternatívnych modelov (rôzne kombinácie demografie, spôsobu liečby, nákladov, dostupnosti pomoci, zdravotných komplikácií, awareness škál a i.), z ktorých boli nevýznamné prediktory postupne vyradené.

Súčasťou je diagnostika multikolinearity (VIF, Belsley-Kuh-Welsch) a porovnanie s **probit** modelom rovnakej špecifikácie.

## Hlavné zistenia

Finálny model má 12 prediktorov, všetky štatisticky významné (väčšina na hladine 1 %). Pravdepodobnosť urgentnej návštevy **zvyšujú**: africko-americký pôvod, nezamestnanosť, počet pádov, návšteva zariadenia aj z iného dôvodu, postihnutie končatín a ťažká hypoglykémia. Prekvapivo rovnakým smerom pôsobia aj **určená pomocná osoba v škole** a **vždy dostupná pomoc** — pravdepodobne ako „signál" horšie kompenzovaného pacienta (reverzná kauzalita), nie ako príčina. Pravdepodobnosť **znižujú**: mužské pohlavie, používanie inzulínovej pumpy, vyššie vzdelanie a (mierne) vyššie náklady na starostlivosť.

| Ukazovateľ | Hodnota |
|:--|--:|
| Počet pozorovaní | 2 454 |
| Významné prediktory | 12 / 12 |
| McFadden R² | 0,164 |
| Multikolinearita (VIF) | bez problémov |
| Correctly predicted | 94,2 % |
| ROC-AUC | 0,789 |

Poznámka k interpretácii: vysoká „správnosť predikcie" (94,2 %) je do veľkej miery dôsledkom nevyváženosti — urgentné návštevy tvoria len ~6 % prípadov, takže model, ktorý väčšinou predpovedá „bez návštevy", má prirodzene vysokú presnosť. Zmysluplnejší je pohľad na jednotlivé koeficienty a ich smer než na celkovú úspešnosť. Model neslúži na presnú predikciu, ale na identifikáciu determinantov.

Podrobný komentovaný výklad je v **[reporte (PDF)](Projekt_ekonometria.pdf)**.

## Data access

**Dáta nie sú súčasťou repozitára.** Pochádzajú z registra T1D Exchange a podliehajú jeho podmienkam použitia — prístup je individuálny a redistribúcia nie je povolená.

O prístup k dátam možno požiadať cez **T1D Exchange** (https://t1dexchange.org). Po získaní datasetu ho uložte do priečinka `data/` ako `t1d_data.gdt` (gretl formát) alebo `t1d_data.xlsx` a upravte prvý riadok skriptu podľa formátu. 

## Spustenie

```
gretlcli -b t1d_urgent_care_logit.inp
```

alebo otvorením `t1d_urgent_care_logit.inp` priamo v gretli (Súbor → Otvoriť príkazový súbor). Analýza je napísaná v [gretli](https://gretl.sourceforge.net/) — voľne dostupnom ekonometrickom softvéri.

## Atribúcia

Táto analýza využíva dáta z registra T1D Exchange. Obsah a závery sú výhradne moje a neboli revidované ani schválené registrom T1D Exchange. Dáta pacientov sú v datasete anonymizované.

## Limity a možné rozšírenia

Hlavným obmedzením je **nevyváženosť datasetu** — urgentné návštevy tvoria len ~6 % prípadov. Koeficienty logit modelu sú na to relatívne odolné, ale predikčné vlastnosti (najmä záchyt vzácnej triedy) tým trpia. Možné smery zlepšenia:

- **Vážená regresia** (vyššia váha menšinovej triedy) alebo **undersampling** väčšinovej triedy — vyvážia vplyv tried a zlepšia senzitivitu na rizikových pacientov. Undersampling ale zmenšuje vzorku a posúva základnú mieru, takže odhadnuté pravdepodobnosti by bolo treba rekalibrovať (prior correction).
- **Firthova penalizovaná regresia** (King & Zeng — logistická regresia pre vzácne javy), navrhnutá presne pre tento typ dát: koriguje malú vzorku menšinovej triedy aj vychýlenie odhadov bez zahadzovania pozorovaní.

## Iné projekty

Toto je jeden z dvoch mojich projektov nad diabetologickými dátami — druhým je [analýza CGM glykémie metódami časových radov](https://github.com/danakozakova/time-series-econometrics/tree/main/03-cgm-glucose-analysis) (SARIMA, GARCH, Grangerova kauzalita). Viac na mojom [GitHub profile](https://github.com/danakozakova).
