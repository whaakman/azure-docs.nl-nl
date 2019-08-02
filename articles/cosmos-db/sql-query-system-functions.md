---
title: Systeemfuncties
description: Meer informatie over SQL-systeem functies in Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mjbrown
ms.openlocfilehash: b0e9c751d46f805af75196da464a39783c95ae6a
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68619990"
---
# <a name="system-functions"></a>Systeemfuncties

 Cosmos DB biedt veel ingebouwde SQL-functies. De categorieën ingebouwde functies worden hieronder vermeld.  
  
|Function|Description|  
|--------------|-----------------|  
|[Wiskundige functies](#mathematical-functions)|Wiskundige functies elke uitvoeren van een berekening, meestal op basis van de invoerwaarden die zijn opgegeven als argumenten en retourneert een numerieke waarde.|  
|[Controle van functies van het type](#type-checking-functions)|Met de typecontrolefuncties kunt u het type van een expressie in SQL-query's controleren.|  
|[Tekenreeksfuncties](#string-functions)|De tekenreeks-functies uitvoeren van een bewerking op een tekenreekswaarde voor invoer en retourneert een tekenreeks, een numerieke of Booleaanse waarde.|  
|[Matrixfuncties](#array-functions)|De matrixfuncties uitvoeren van een bewerking op een matrix invoerwaarde en retourneren numerieke, Booleaanse waarde of Matrixwaarde.|
|[Datum-en tijd functies](#date-time-functions)|Met de functies datum en tijd kunt u de huidige UTC-datum en-tijd in twee vormen ophalen. een numerieke tijds tempel waarvan de waarde de UNIX-epoche is in milliseconden of als een teken reeks die voldoet aan de ISO 8601-indeling.|
|[Ruimtelijke-functies](#spatial-functions)|De ruimtelijke functies uitvoeren van een bewerking op een invoerwaarde ruimtelijke index en een numerieke of Booleaanse waarde retourneren.|  

Hieronder vindt u een lijst met functies binnen elke categorie:

| Functiegroep | Bewerkingen |
|---------|----------|
| Wiskundige functies | ABS, PLAFOND, EXP, VLOER, LOG, LOG10, KRACHT, AFRONDING, TEKEN, WORTEL, KWADRAAT, TRUNC, BOOGCOS, ASIN, BOOGTAN, ATN2, COS, COT, GRADEN, PI, RADIALEN, ASELECT, SIN, TAN |
| Type functies controleren | IS_ARRAY, IS_BOOL, IS_NULL, IS_NUMBER, IS_OBJECT, IS_STRING, IS_DEFINED, IS_PRIMITIVE |
| Tekenreeksfuncties | CONCAT, CONTAINS, ENDSWITH, INDEX_OF, LEFT, LENGTH, LOWER, LTRIM, REPLACE, REPLICATE, REVERSE, RIGHT, RTRIM, STARTSWITH, SUBSTRING, UPPER |
| Matrixfuncties | ARRAY_CONCAT, ARRAY_CONTAINS, ARRAY_LENGTH en ARRAY_SLICE |
| Datum-en tijd functies | GETCURRENTDATETIME, GETCURRENTTIMESTAMP,  |
| Ruimtelijke functies | ST_DISTANCE, ST_WITHIN, ST_INTERSECTS, ST_ISVALID, ST_ISVALIDDETAILED |

Als u momenteel een door de gebruiker gedefinieerde functie (UDF) gebruikt waarvoor een ingebouwde functie nu beschikbaar is, wordt de bijbehorende ingebouwde functie sneller uitgevoerd en efficiënter.

Het belangrijkste verschil tussen Cosmos DB functies en ANSI SQL-functies is dat Cosmos DB-functies zijn ontworpen om goed te werken met schemaloze en gemengde schema gegevens. Als bijvoorbeeld een eigenschap ontbreekt of een niet-numerieke waarde heeft zoals `unknown`, wordt het item overgeslagen in plaats van een fout te retour neren.

##  <a name="mathematical-functions"></a> Wiskundige functies  

Met elke wiskundige functie wordt een berekening op basis van de opgegeven invoerwaarden uitgevoerd en een numerieke waarde geretourneerd.

U kunt query's uitvoeren zoals in het volgende voor beeld:

```sql
    SELECT VALUE ABS(-4)
```

Het resultaat is:

```json
    [4]
```

Dit is een tabel met ondersteunde ingebouwde wiskundige functies.
  
||||  
|-|-|-|  
|[ABS](#bk_abs)|[ACOS](#bk_acos)|[ASIN](#bk_asin)|  
|[ATAN](#bk_atan)|[ATN2](#bk_atn2)|[MAXIMUM](#bk_ceiling)|  
|[COS](#bk_cos)|[COT](#bk_cot)|[DEGREES](#bk_degrees)|  
|[EXP](#bk_exp)|[FLOOR](#bk_floor)|[LOGBOEK](#bk_log)|  
|[LOG10](#bk_log10)|[PI](#bk_pi)|[ENERGIEBEHEER](#bk_power)|  
|[RADIANS](#bk_radians)|[AFRONDEN](#bk_round)|[ASELECT](#bk_rand)|
|[SIN](#bk_sin)|[SQRT](#bk_sqrt)|[SQUARE](#bk_square)|
|[SIGN](#bk_sign)|[TAN](#bk_tan)|[TRUNC](#bk_trunc)||  
  
####  <a name="bk_abs"></a> ABS  
 Retourneert de absolute (positieve) waarde van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
ABS (<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld toont de resultaten van het gebruik van de functie ABS op drie verschillende aantallen.  
  
```  
SELECT ABS(-1) AS abs1, ABS(0) AS abs2, ABS(1) AS abs3 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{abs1: 1, abs2: 0, abs3: 1}]  
```  
  
####  <a name="bk_acos"></a> ACOS  
 Retourneert de hoek, in radialen, waarvan de cosinus de opgegeven numerieke expressie is. Dit wordt ook wel de arccosinus genoemd.  
  
 **Syntaxis**  
  
```  
ACOS(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de ACOS van-1.  
  
```  
SELECT ACOS(-1) AS acos 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"acos": 3.1415926535897931}]  
```  
  
####  <a name="bk_asin"></a> ASIN  
 Retourneert de hoek, in radialen, waarvan de sinus de opgegeven numerieke expressie is. Dit wordt ook boogsinus genoemd.  
  
 **Syntaxis**  
  
```  
ASIN(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de ASIN-1.  
  
```  
SELECT ASIN(-1) AS asin  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"asin": -1.5707963267948966}]  
```  
  
####  <a name="bk_atan"></a> ATAN  
 Retourneert de hoek, in radialen, waarvan de tangens de opgegeven numerieke expressie is. Dit wordt ook wel de arctangens genoemd.  
  
 **Syntaxis**  
  
```  
ATAN(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de ATAN van de opgegeven waarde.  
  
```  
SELECT ATAN(-45.01) AS atan  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"atan": -1.5485826962062663}]  
```  
  
####  <a name="bk_atn2"></a> ATN2  
 Retourneert de hoofdsom van de arctangens van y / x, uitgedrukt in radialen.  
  
 **Syntaxis**  
  
```  
ATN2(<numeric_expression>, <numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld berekent de ATN2 voor de opgegeven x- en y onderdelen.  
  
```  
SELECT ATN2(35.175643, 129.44) AS atn2  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"atn2": 1.3054517947300646}]  
```  
  
####  <a name="bk_ceiling"></a> MAXIMUM  
 Retourneert het kleinste gehele getal dat groter is dan of gelijk is aan de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
CEILING (<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld toont een positieve numerieke, negatief en nulwaarden met de functie CEILING.  
  
```  
SELECT CEILING(123.45) AS c1, CEILING(-123.45) AS c2, CEILING(0.0) AS c3  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{c1: 124, c2: -123, c3: 0}]  
```  
  
####  <a name="bk_cos"></a> COS  
 Retourneert de trigonometrische cosinus van de opgegeven hoek, in radialen, in de opgegeven expressie.  
  
 **Syntaxis**  
  
```  
COS(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld berekent de Cosinus van de opgegeven hoek.  
  
```  
SELECT COS(14.78) AS cos  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"cos": -0.59946542619465426}]  
```  
  
####  <a name="bk_cot"></a> COT  
 Retourneert de trigonometrische cotangens van de opgegeven hoek, in radialen, in de opgegeven expressie.  
  
 **Syntaxis**  
  
```  
COT(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld berekent de COT van de opgegeven hoek.  
  
```  
SELECT COT(124.1332) AS cot  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"cot": -0.040311998371148884}]  
```  
  
####  <a name="bk_degrees"></a> GRADEN  
 Retourneert de overeenkomende hoek in graden voor een hoek die is opgegeven in radialen.  
  
 **Syntaxis**  
  
```  
DEGREES (<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert het aantal graden in een hoek van PI/2 radialen.  
  
```  
SELECT DEGREES(PI()/2) AS degrees  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"degrees": 90}]  
```  
  
####  <a name="bk_floor"></a> FLOOR  
 Retourneert het grootste gehele getal dat kleiner is dan of gelijk is aan de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
FLOOR (<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld toont een positieve numerieke, negatief en nulwaarden met de functie FLOOR.  
  
```  
SELECT FLOOR(123.45) AS fl1, FLOOR(-123.45) AS fl2, FLOOR(0.0) AS fl3  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{fl1: 123, fl2: -124, fl3: 0}]  
```  
  
####  <a name="bk_exp"></a> EXP  
 Retourneert de exponentiële waarde van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
EXP (<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Opmerkingen**  
  
  De constante **e** (2.718281...) vormt de basis van natuurlijke logaritmen.  
  
  De exponent van een getal is de constante **e** verheven tot de macht van het getal. Bijvoorbeeld, EXP(1.0) = e ^ 1.0 = 2.71828182845905 en EXP(10) = e ^ 10 = 22026.4657948067.  
  
  De exponentiële waarde van de natuurlijke logaritme van een getal is het getal zelf: EXP (logboek (n)) = n. En de natuurlijke logaritme van de exponentiële waarde van een getal is het getal zelf: LOG (EXP (n)) = n.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt een variabele gedeclareerd en berekent de exponentiële waarde van de opgegeven variabele (10).  
  
```  
SELECT EXP(10) AS exp  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{exp: 22026.465794806718}]  
```  
  
 Het volgende voorbeeld retourneert de exponentiële waarde van de natural logarithm van 20 en de natuurlijke logaritme van de exponentiële van 20. Omdat deze functies inverse functies van elkaar zijn, is de geretourneerde waarde met de afronding voor zwevende punt math in beide gevallen 20.  
  
```  
SELECT EXP(LOG(20)) AS exp1, LOG(EXP(20)) AS exp2  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{exp1: 19.999999999999996, exp2: 20}]  
```  
  
####  <a name="bk_log"></a> LOGBOEK  
 Retourneert de natuurlijke logaritme van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
LOG (<numeric_expression> [, <base>])  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
- `base`  
  
   Optionele numerieke argument dat Hiermee stelt u de basis voor de logaritme.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Opmerkingen**  
  
  Standaard retourneert LOG() de natuurlijke logaritme. U kunt het grondtal van de logaritme met een andere waarde wijzigen met behulp van de optionele parameter basis.  
  
  De natuurlijke logaritme is de logaritme voor de base **e**, waarbij **e** is een constante irrational ongeveer gelijk zijn aan 2.718281828.  
  
  De natuurlijke logaritme van de exponentiële waarde van een getal is het getal zelf: LOG (EXP (n)) = n. En de exponentiële waarde van de natuurlijke logaritme van een getal is het getal zelf: EXP (logboek (n)) = n.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt een variabele gedeclareerd en retourneert de logaritme-waarde van de opgegeven variabele (10).  
  
```  
SELECT LOG(10) AS log  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{log: 2.3025850929940459}]  
```  
  
 Het volgende voorbeeld wordt het logboek voor de exponent van een getal berekend.  
  
```  
SELECT EXP(LOG(10)) AS expLog  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{expLog: 10.000000000000002}]  
```  
  
####  <a name="bk_log10"></a> LOG10  
 Retourneert de logaritme met grondtal 10 van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
LOG10 (<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Opmerkingen**  
  
  De functies LOG10 en POWER zijn omgekeerd met elkaar gerelateerd. Bijvoorbeeld, 10 ^ LOG10(n) = n.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt een variabele gedeclareerd en retourneert de waarde LOG10 van de opgegeven variabele (100).  
  
```  
SELECT LOG10(100) AS log10 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{log10: 2}]  
```  
  
####  <a name="bk_pi"></a> PI  
 Retourneert de constante waarde van PI.  
  
 **Syntaxis**  
  
```  
PI ()  
```  
   
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de waarde van PI.  
  
```  
SELECT PI() AS pi 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"pi": 3.1415926535897931}]  
```  
  
####  <a name="bk_power"></a> ENERGIEBEHEER  
 Retourneert de waarde van de opgegeven expressie voor de opgegeven macht.  
  
 **Syntaxis**  
  
```  
POWER (<numeric_expression>, <y>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
- `y`  
  
   Is de kracht waarnaar u wilt verhogen `numeric_expression`.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet u een getal tot de macht 3 (de kubus van het getal) verhogen.  
  
```  
SELECT POWER(2, 3) AS pow1, POWER(2.5, 3) AS pow2  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{pow1: 8, pow2: 15.625}]  
```  
  
####  <a name="bk_radians"></a> RADIALEN  
 Retourneert radialen als een numerieke expressie in graden wordt ingevoerd.  
  
 **Syntaxis**  
  
```  
RADIANS (<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld duurt een paar hoeken als invoer en retourneert de overeenkomstige waarden ervan Radiaal.  
  
```  
SELECT RADIANS(-45.01) AS r1, RADIANS(-181.01) AS r2, RADIANS(0) AS r3, RADIANS(0.1472738) AS r4, RADIANS(197.1099392) AS r5  
```  
  
  Hier volgt de resultatenset.  
  
```  
[{  
       "r1": -0.7855726963226477,  
       "r2": -3.1592204790349356,  
       "r3": 0,  
       "r4": 0.0025704127119236249,  
       "r5": 3.4402174274458375  
   }]  
```  
  
####  <a name="bk_round"></a> AFRONDEN  
 Retourneert een numerieke waarde, afgerond naar het dichtstbijzijnde gehele getal.  
  
 **Syntaxis**  
  
```  
ROUND(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Opmerkingen**
  
  De Afrondings bewerking wordt uitgevoerd op basis van het middel punt afronding van nul. Als de invoer een numerieke expressie is die precies tussen twee gehele getallen ligt, is het resultaat de dichtstbijzijnde gehele waarde van nul.  
  
  |<numeric_expression>|Impliceer|
  |-|-|
  |-6,5000|-7|
  |-0,5|-1|
  |0.5|1|
  |6,5000|7||
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt de volgende positieve en negatieve getallen naar het dichtstbijzijnde gehele getal afgerond.  
  
```  
SELECT ROUND(2.4) AS r1, ROUND(2.6) AS r2, ROUND(2.5) AS r3, ROUND(-2.4) AS r4, ROUND(-2.6) AS r5  
```  
  
  Hier volgt de resultatenset.  
  
```  
[{r1: 2, r2: 3, r3: 3, r4: -2, r5: -3}]  
```  

#### <a name="bk_rand"></a>ASELECT
 Retourneert een wille keurig gegenereerde numerieke waarde van [0, 1).
 
 **Syntaxis**  
  
```  
RAND ()  
```  

  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  In het volgende voor beeld wordt een wille keurig gegenereerde numerieke waarde geretourneerd.  
  
```  
SELECT RAND() AS rand 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"rand": 0.87860053195618093}]  
``` 

####  <a name="bk_sign"></a> MELD U  
 Retourneert de positief (+ 1), nul (0) of minteken (-1) van de opgegeven numerieke expressie.  
  
 **Syntaxis**  
  
```  
SIGN(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de waarden van de aanmelding van de getallen van -2 naar 2.  
  
```  
SELECT SIGN(-2) AS s1, SIGN(-1) AS s2, SIGN(0) AS s3, SIGN(1) AS s4, SIGN(2) AS s5  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{s1: -1, s2: -1, s3: 0, s4: 1, s5: 1}]  
```  
  
####  <a name="bk_sin"></a> SIN  
 Retourneert de trigonometrische sinus van de opgegeven hoek, in radialen, in de opgegeven expressie.  
  
 **Syntaxis**  
  
```  
SIN(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld berekent de SINUS van de opgegeven hoek.  
  
```  
SELECT SIN(45.175643) AS sin  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"sin": 0.929607286611012}]  
```  
  
####  <a name="bk_sqrt"></a> WORTEL  
 Retourneert de vierkantswortel van de opgegeven numerieke waarde.  
  
 **Syntaxis**  
  
```  
SQRT(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de vierkantswortel van getallen 1-3.  
  
```  
SELECT SQRT(1) AS s1, SQRT(2.0) AS s2, SQRT(3) AS s3  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{s1: 1, s2: 1.4142135623730952, s3: 1.7320508075688772}]  
```  
  
####  <a name="bk_square"></a> VIERKANT  
 Retourneert het kwadraat van de opgegeven numerieke waarde.  
  
 **Syntaxis**  
  
```  
SQUARE(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de kwadraten van getallen 1-3.  
  
```  
SELECT SQUARE(1) AS s1, SQUARE(2.0) AS s2, SQUARE(3) AS s3  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{s1: 1, s2: 4, s3: 9}]  
```  
  
####  <a name="bk_tan"></a> TAN  
 Retourneert de tangens van de opgegeven hoek in radialen in de opgegeven expressie.  
  
 **Syntaxis**  
  
```  
TAN (<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld berekent de tangens van PI () / 2.  
  
```  
SELECT TAN(PI()/2) AS tan 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"tan": 16331239353195370 }]  
```  
  
####  <a name="bk_trunc"></a> GEHEEL  
 Retourneert een numerieke waarde, afgekapt tot het dichtstbijzijnde gehele getal.  
  
 **Syntaxis**  
  
```  
TRUNC(<numeric_expression>)  
```  
  
 **Argumenten**  
  
- `numeric_expression`  
  
   Een numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt de volgende positieve en negatieve getallen naar het dichtstbijzijnde gehele getal afgekapt.  
  
```  
SELECT TRUNC(2.4) AS t1, TRUNC(2.6) AS t2, TRUNC(2.5) AS t3, TRUNC(-2.4) AS t4, TRUNC(-2.6) AS t5  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{t1: 2, t2: 2, t3: 2, t4: -2, t5: -2}]  
```

## <a id="type-checking-functions"></a>Type controleren van functies

Met de functies voor type controles kunt u het type van een expressie in een SQL-query controleren. U kunt type-controle functies gebruiken om de typen eigenschappen binnen items te bepalen, wanneer deze worden gevariable of onbekend. Hier volgt een tabel met ondersteunde ingebouwde functies voor type controle:

De volgende functies ondersteunen controleren op basis van de invoerwaarden van het type en elke een Booleaanse waarde retourneren.  
  
||||  
|-|-|-|  
|[IS_ARRAY](#bk_is_array)|[IS_BOOL](#bk_is_bool)|[IS_DEFINED](#bk_is_defined)|  
|[IS_NULL](#bk_is_null)|[IS_NUMBER](#bk_is_number)|[IS_OBJECT](#bk_is_object)|  
|[IS_PRIMITIVE](#bk_is_primitive)|[IS_STRING](#bk_is_string)||  
  
####  <a name="bk_is_array"></a> IS_ARRAY  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een matrix is.  
  
 **Syntaxis**  
  
```  
IS_ARRAY(<expression>)  
```  
  
 **Argumenten**  
  
- `expression`  
  
   Een geldige expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_ARRAY.  
  
```  
SELECT   
 IS_ARRAY(true) AS isArray1,   
 IS_ARRAY(1) AS isArray2,  
 IS_ARRAY("value") AS isArray3,  
 IS_ARRAY(null) AS isArray4,  
 IS_ARRAY({prop: "value"}) AS isArray5,   
 IS_ARRAY([1, 2, 3]) AS isArray6,  
 IS_ARRAY({prop: "value"}.prop2) AS isArray7  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"isArray1":false,"isArray2":false,"isArray3":false,"isArray4":false,"isArray5":false,"isArray6":true,"isArray7":false}]
```  
  
####  <a name="bk_is_bool"></a> IS_BOOL  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een Booleaanse waarde is.  
  
 **Syntaxis**  
  
```  
IS_BOOL(<expression>)  
```  
  
 **Argumenten**  
  
- `expression`  
  
   Een geldige expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_BOOL.  
  
```  
SELECT   
    IS_BOOL(true) AS isBool1,   
    IS_BOOL(1) AS isBool2,  
    IS_BOOL("value") AS isBool3,   
    IS_BOOL(null) AS isBool4,  
    IS_BOOL({prop: "value"}) AS isBool5,   
    IS_BOOL([1, 2, 3]) AS isBool6,  
    IS_BOOL({prop: "value"}.prop2) AS isBool7  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"isBool1":true,"isBool2":false,"isBool3":false,"isBool4":false,"isBool5":false,"isBool6":false,"isBool7":false}]
```  
  
####  <a name="bk_is_defined"></a> IS_DEFINED  
 Retourneert een Booleaanse waarde die aangeeft of aan de eigenschap een waarde is toegewezen.  
  
 **Syntaxis**  
  
```  
IS_DEFINED(<expression>)  
```  
  
 **Argumenten**  
  
- `expression`  
  
   Een geldige expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt gecontroleerd op de aanwezigheid van een eigenschap binnen het opgegeven JSON-document. De eerste retourneert ' True ', omdat "a" aanwezig is, maar de tweede ' false ' retourneert omdat "b" afwezig is.  
  
```  
SELECT IS_DEFINED({ "a" : 5 }.a) AS isDefined1, IS_DEFINED({ "a" : 5 }.b) AS isDefined2 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"isDefined1":true,"isDefined2":false}]  
```  
  
####  <a name="bk_is_null"></a> IS_NULL  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie null is.  
  
 **Syntaxis**  
  
```  
IS_NULL(<expression>)  
```  
  
 **Argumenten**  
  
- `expression`  
  
   Een geldige expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_NULL.  
  
```  
SELECT   
    IS_NULL(true) AS isNull1,   
    IS_NULL(1) AS isNull2,  
    IS_NULL("value") AS isNull3,   
    IS_NULL(null) AS isNull4,  
    IS_NULL({prop: "value"}) AS isNull5,   
    IS_NULL([1, 2, 3]) AS isNull6,  
    IS_NULL({prop: "value"}.prop2) AS isNull7  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"isNull1":false,"isNull2":false,"isNull3":false,"isNull4":true,"isNull5":false,"isNull6":false,"isNull7":false}]
```  
  
####  <a name="bk_is_number"></a> IS_NUMBER  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een getal is.  
  
 **Syntaxis**  
  
```  
IS_NUMBER(<expression>)  
```  
  
 **Argumenten**  
  
- `expression`  
  
   Een geldige expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_NULL.  
  
```  
SELECT   
    IS_NUMBER(true) AS isNum1,   
    IS_NUMBER(1) AS isNum2,  
    IS_NUMBER("value") AS isNum3,   
    IS_NUMBER(null) AS isNum4,  
    IS_NUMBER({prop: "value"}) AS isNum5,   
    IS_NUMBER([1, 2, 3]) AS isNum6,  
    IS_NUMBER({prop: "value"}.prop2) AS isNum7  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"isNum1":false,"isNum2":true,"isNum3":false,"isNum4":false,"isNum5":false,"isNum6":false,"isNum7":false}]  
```  
  
####  <a name="bk_is_object"></a> IS_OBJECT  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een JSON-object is.  
  
 **Syntaxis**  
  
```  
IS_OBJECT(<expression>)  
```  
  
 **Argumenten**  
  
- `expression`  
  
   Een geldige expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_OBJECT.  
  
```  
SELECT   
    IS_OBJECT(true) AS isObj1,   
    IS_OBJECT(1) AS isObj2,  
    IS_OBJECT("value") AS isObj3,   
    IS_OBJECT(null) AS isObj4,  
    IS_OBJECT({prop: "value"}) AS isObj5,   
    IS_OBJECT([1, 2, 3]) AS isObj6,  
    IS_OBJECT({prop: "value"}.prop2) AS isObj7  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"isObj1":false,"isObj2":false,"isObj3":false,"isObj4":false,"isObj5":true,"isObj6":false,"isObj7":false}]
```  
  
####  <a name="bk_is_primitive"></a> IS_PRIMITIVE  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een primitieve nemen is (string, Boolean, numerieke of null).  
  
 **Syntaxis**  
  
```  
IS_PRIMITIVE(<expression>)  
```  
  
 **Argumenten**  
  
- `expression`  
  
   Een geldige expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_PRIMITIVE.  
  
```  
SELECT   
           IS_PRIMITIVE(true) AS isPrim1,   
           IS_PRIMITIVE(1) AS isPrim2,  
           IS_PRIMITIVE("value") AS isPrim3,   
           IS_PRIMITIVE(null) AS isPrim4,  
           IS_PRIMITIVE({prop: "value"}) AS isPrim5,   
           IS_PRIMITIVE([1, 2, 3]) AS isPrim6,  
           IS_PRIMITIVE({prop: "value"}.prop2) AS isPrim7  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"isPrim1": true, "isPrim2": true, "isPrim3": true, "isPrim4": true, "isPrim5": false, "isPrim6": false, "isPrim7": false}]  
```  
  
####  <a name="bk_is_string"></a> IS_STRING  
 Retourneert een Booleaanse waarde die aangeeft of het type van de opgegeven expressie een tekenreeks is.  
  
 **Syntaxis**  
  
```  
IS_STRING(<expression>)  
```  
  
 **Argumenten**  
  
- `expression`  
  
   Een geldige expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt gecontroleerd voor objecten van JSON Boolean, getal, tekenreeks, null, object, matrix en niet-gedefinieerde typen met behulp van de functie IS_STRING.  
  
```  
SELECT   
       IS_STRING(true) AS isStr1,   
       IS_STRING(1) AS isStr2,  
       IS_STRING("value") AS isStr3,   
       IS_STRING(null) AS isStr4,  
       IS_STRING({prop: "value"}) AS isStr5,   
       IS_STRING([1, 2, 3]) AS isStr6,  
       IS_STRING({prop: "value"}.prop2) AS isStr7  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"isStr1":false,"isStr2":false,"isStr3":true,"isStr4":false,"isStr5":false,"isStr6":false,"isStr7":false}] 
```  

## <a id="string-functions"></a>Teken reeks functies

De volgende scalaire functies voeren een bewerking uit op een invoer waarde voor teken reeksen en retour neren een teken reeks, een numerieke of Booleaanse waarde:
  
||||  
|-|-|-|  
|[CONCAT](#bk_concat)|[BEVAT](#bk_contains)|[ENDSWITH](#bk_endswith)|  
|[INDEX_OF](#bk_index_of)|[LEFT](#bk_left)|[LENGTE](#bk_length)|  
|[LAGERE](#bk_lower)|[LTRIM](#bk_ltrim)|[REPLACE](#bk_replace)|  
|[REPLICEREN](#bk_replicate)|[REVERSE](#bk_reverse)|[RIGHT](#bk_right)|  
|[RTRIM](#bk_rtrim)|[STARTSWITH](#bk_startswith)|[StringToArray](#bk_stringtoarray)|
|[StringToBoolean](#bk_stringtoboolean)|[StringToNull](#bk_stringtonull)|[StringToNumber](#bk_stringtonumber)|
|[StringToObject](#bk_stringtoobject)|[DE SUBTEKENREEKS](#bk_substring)|[ToString](#bk_tostring)|
|[TRIM](#bk_trim)|[BOVENSTE](#bk_upper)||
  
####  <a name="bk_concat"></a> CONCAT  
 Retourneert een tekenreeks die het resultaat is van het samenvoegen van twee of meer tekenreekswaarden.  
  
 **Syntaxis**  
  
```  
CONCAT(<str_expr>, <str_expr> [, <str_expr>])  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de samengevoegde tekenreeks van de opgegeven waarden.  
  
```  
SELECT CONCAT("abc", "def") AS concat  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"concat": "abcdef"}  
```  
  
####  <a name="bk_contains"></a> BEVAT  
 Retourneert een Booleaanse waarde die aangeeft of de eerste tekenreeksexpressie de tweede bevat.  
  
 **Syntaxis**  
  
```  
CONTAINS(<str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld controleert als "abc" bevat "ab" en "d".  
  
```  
SELECT CONTAINS("abc", "ab") AS c1, CONTAINS("abc", "d") AS c2 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"c1": true, "c2": false}]  
```  
  
####  <a name="bk_endswith"></a> ENDSWITH  
 Retourneert een Booleaanse waarde die aangeeft of de eerste expressie tekenreeks eindigt met de tweede.  
  
 **Syntaxis**  
  
```  
ENDSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert dat de "abc" eindigt op "b" en 'bc'.  
  
```  
SELECT ENDSWITH("abc", "b") AS e1, ENDSWITH("abc", "bc") AS e2 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"e1": false, "e2": true}]  
```  
  
####  <a name="bk_index_of"></a> INDEX_OF  
 Retourneert de beginpositie van het eerste exemplaar van de tweede tekenreeksexpressie binnen de eerste opgegeven tekenreeksexpressie, of -1 als de tekenreeks niet is gevonden.  
  
 **Syntaxis**  
  
```  
INDEX_OF(<str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt de index van verschillende subtekenreeksen in "abc".  
  
```  
SELECT INDEX_OF("abc", "ab") AS i1, INDEX_OF("abc", "b") AS i2, INDEX_OF("abc", "c") AS i3 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"i1": 0, "i2": 1, "i3": -1}]  
```  
  
####  <a name="bk_left"></a> LINKS  
 Retourneert het linkerdeel van een tekenreeks met het opgegeven aantal tekens.  
  
 **Syntaxis**  
  
```  
LEFT(<str_expr>, <num_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
- `num_expr`  
  
   Een geldige numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt het linkerdeel van "abc" voor verschillende waarden voor lengte.  
  
```  
SELECT LEFT("abc", 1) AS l1, LEFT("abc", 2) AS l2 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"l1": "a", "l2": "ab"}]  
```  
  
####  <a name="bk_length"></a> LENGTE  
 Retourneert het aantal tekens van de opgegeven tekenreeksexpressie.  
  
 **Syntaxis**  
  
```  
LENGTH(<str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de lengte van een tekenreeks.  
  
```  
SELECT LENGTH("abc") AS len 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"len": 3}]  
```  
  
####  <a name="bk_lower"></a> LAGERE  
 Retourneert een tekenreeksexpressie na het converteren van tekens in hoofdletters naar kleine letters.  
  
 **Syntaxis**  
  
```  
LOWER(<str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe u in een query lager gebruiken.  
  
```  
SELECT LOWER("Abc") AS lower
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"lower": "abc"}]  
  
```  
  
####  <a name="bk_ltrim"></a> LTRIM  
 Retourneert een tekenreeksexpressie na het verwijderen van lege voorlooptekens.  
  
 **Syntaxis**  
  
```  
LTRIM(<str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe u LTRIM binnen een query gebruiken.  
  
```  
SELECT LTRIM("  abc") AS l1, LTRIM("abc") AS l2, LTRIM("abc   ") AS l3 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"l1": "abc", "l2": "abc", "l3": "abc   "}]  
```  
  
####  <a name="bk_replace"></a> VERVANGEN  
 Vervangt alle exemplaren van een opgegeven tekenreekswaarde door een andere tekenreekswaarde.  
  
 **Syntaxis**  
  
```  
REPLACE(<str_expr>, <str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe u vervangen in een query.  
  
```  
SELECT REPLACE("This is a Test", "Test", "desk") AS replace 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"replace": "This is a desk"}]  
```  
  
####  <a name="bk_replicate"></a> REPLICEREN  
 Herhaalt een tekenreekswaarde een opgegeven aantal keren.  
  
 **Syntaxis**  
  
```  
REPLICATE(<str_expr>, <num_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
- `num_expr`  
  
   Een geldige numerieke expressie is. Als num_expr negatief of niet-eindig is, wordt het resultaat niet gedefinieerd.

  > [!NOTE]
  > De maximale lengte van het resultaat is 10.000 tekens, dat wil zeggen (length (str_expr) * num_expr) < = 10.000.
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe u kunt REPLICEREN in een query gebruiken.  
  
```  
SELECT REPLICATE("a", 3) AS replicate  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"replicate": "aaa"}]  
```  
  
####  <a name="bk_reverse"></a> OMKEREN  
 Retourneert een tekenreekswaarde in de omgekeerde volgorde.  
  
 **Syntaxis**  
  
```  
REVERSE(<str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld laat zien hoe REVERSE gebruiken in een query.  
  
```  
SELECT REVERSE("Abc") AS reverse  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"reverse": "cbA"}]  
```  
  
####  <a name="bk_right"></a> RECHTS  
 Retourneert het rechterdeel van een tekenreeks met het opgegeven aantal tekens.  
  
 **Syntaxis**  
  
```  
RIGHT(<str_expr>, <num_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
- `num_expr`  
  
   Een geldige numerieke expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt het rechterdeel van "abc" voor verschillende waarden voor lengte.  
  
```  
SELECT RIGHT("abc", 1) AS r1, RIGHT("abc", 2) AS r2 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"r1": "c", "r2": "bc"}]  
```  
  
####  <a name="bk_rtrim"></a> RTRIM  
 Retourneert een tekenreeksexpressie na het afsluitende spaties verwijderen.  
  
 **Syntaxis**  
  
```  
RTRIM(<str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe u RTRIM binnen een query gebruiken.  
  
```  
SELECT RTRIM("  abc") AS r1, RTRIM("abc") AS r2, RTRIM("abc   ") AS r3  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"r1": "   abc", "r2": "abc", "r3": "abc"}]  
```  
  
####  <a name="bk_startswith"></a> STARTSWITH  
 Retourneert een Booleaanse waarde die aangeeft of de eerste expressie tekenreeks begint met de tweede.  
  
 **Syntaxis**  
  
```  
STARTSWITH(<str_expr>, <str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld wordt als de tekenreeks "abc" met "b begint" en "a".  
  
```  
SELECT STARTSWITH("abc", "b") AS s1, STARTSWITH("abc", "a") AS s2  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"s1": false, "s2": true}]  
```  

  ####  <a name="bk_stringtoarray"></a>StringToArray  
 Retourneert een expressie die is vertaald naar een matrix. Als expressie niet kan worden vertaald, retourneert ongedefinieerd.  
  
 **Syntaxis**  
  
```  
StringToArray(<expr>)  
```  
  
 **Argumenten**  
  
- `expr`  
  
   Is een geldige scalaire expressie die moet worden geëvalueerd als een JSON-matrix expressie. Houd er rekening mee dat geneste teken reeks waarden moeten worden geschreven met dubbele aanhalings tekens die geldig zijn. Zie [JSON.org](https://json.org/) voor meer informatie over de JSON-indeling.
  
  **Typen retourneren**  
  
  Retourneert een matrix expressie of is niet gedefinieerd.  
  
  **Voorbeelden**  
  
  In het volgende voor beeld ziet u hoe StringToArray zich gedraagt in verschillende typen. 
  
 Hier volgen enkele voor beelden met geldige invoer.

```
SELECT 
    StringToArray('[]') AS a1, 
    StringToArray("[1,2,3]") AS a2,
    StringToArray("[\"str\",2,3]") AS a3,
    StringToArray('[["5","6","7"],["8"],["9"]]') AS a4,
    StringToArray('[1,2,3, "[4,5,6]",[7,8]]') AS a5
```

Hier volgt de resultatenset.

```
[{"a1": [], "a2": [1,2,3], "a3": ["str",2,3], "a4": [["5","6","7"],["8"],["9"]], "a5": [1,2,3,"[4,5,6]",[7,8]]}]
```

Hier volgt een voor beeld van ongeldige invoer. 
   
 Enkele aanhalings tekens in de matrix zijn geen geldige JSON.
Hoewel ze wel geldig zijn binnen een query, worden ze niet geparseerd naar geldige matrices. Teken reeksen binnen de matrix teken reeks moeten worden voorafgegaan door '\\[\\'] ' of de omliggende aanhaling moet één ' ['] ' bevatten.

```
SELECT
    StringToArray("['5','6','7']")
```

Hier volgt de resultatenset.

```
[{}]
```

Hier volgen enkele voor beelden van ongeldige invoer.
   
 De door gegeven expressie wordt geparseerd als een JSON-matrix. de volgende evaluaties zijn niet van het type matrix en daarom wordt ongedefinieerd geretourneerd.
   
```
SELECT
    StringToArray("["),
    StringToArray("1"),
    StringToArray(NaN),
    StringToArray(false),
    StringToArray(undefined)
```

Hier volgt de resultatenset.

```
[{}]
```

####  <a name="bk_stringtoboolean"></a>StringToBoolean  
 Retourneert een expressie die is vertaald naar een Booleaanse waarde. Als expressie niet kan worden vertaald, retourneert ongedefinieerd.  
  
 **Syntaxis**  
  
```  
StringToBoolean(<expr>)  
```  
  
 **Argumenten**  
  
- `expr`  
  
   Is een geldige scalaire expressie die als een Boole-expressie moet worden geëvalueerd.  
  
  **Typen retourneren**  
  
  Retourneert een booleaanse expressie of een niet-gedefinieerde waarde.  
  
  **Voorbeelden**  
  
  In het volgende voor beeld ziet u hoe StringToBoolean zich gedraagt in verschillende typen. 
 
 Hier volgen enkele voor beelden met geldige invoer.

Spatie is alleen toegestaan vóór of na "True"/"false".

```  
SELECT 
    StringToBoolean("true") AS b1, 
    StringToBoolean("    false") AS b2,
    StringToBoolean("false    ") AS b3
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"b1": true, "b2": false, "b3": false}]
```  

Hier volgen enkele voor beelden met ongeldige invoer.

 Boole-waarden zijn hoofdletter gevoelig en moeten met alle kleine letters worden geschreven, dat wil zeggen ' True ' en ' false '.

```  
SELECT 
    StringToBoolean("TRUE"),
    StringToBoolean("False")
```  

Hier volgt de resultatenset.  
  
```  
[{}]
``` 

De door gegeven expressie wordt geparseerd als een Boole-expressie. deze invoer kan niet worden geëvalueerd om Booleaanse waarden te typen en retour neren dus niet-gedefinieerd.

```  
SELECT 
    StringToBoolean("null"),
    StringToBoolean(undefined),
    StringToBoolean(NaN), 
    StringToBoolean(false), 
    StringToBoolean(true)
```  

Hier volgt de resultatenset.  
  
```  
[{}]
```  

####  <a name="bk_stringtonull"></a> StringToNull  
 Retourneert een expressie die is vertaald naar null. Als expressie niet kan worden vertaald, retourneert ongedefinieerd.  
  
 **Syntaxis**  
  
```  
StringToNull(<expr>)  
```  
  
 **Argumenten**  
  
- `expr`  
  
   Is een geldige scalaire expressie die als een null-expressie moet worden geëvalueerd.
  
  **Typen retourneren**  
  
  Retourneert een null-expressie of een niet-gedefinieerde waarde.  
  
  **Voorbeelden**  
  
  In het volgende voor beeld ziet u hoe StringToNull zich gedraagt in verschillende typen. 

Hier volgen enkele voor beelden met geldige invoer.

 Witruimte is alleen toegestaan vóór of na null.

```  
SELECT 
    StringToNull("null") AS n1, 
    StringToNull("  null ") AS n2,
    IS_NULL(StringToNull("null   ")) AS n3
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"n1": null, "n2": null, "n3": true}]
```  

Hier volgen enkele voor beelden met ongeldige invoer.

Null is hoofdletter gevoelig en moet worden geschreven met alleen kleine letters, dat wil zeggen ' null '.

```  
SELECT    
    StringToNull("NULL"),
    StringToNull("Null")
```  
  
 Hier volgt de resultatenset.  
  
```  
[{}]
```  

De door gegeven expressie wordt geparseerd als een null-expressie. deze invoer kan niet worden geëvalueerd om null te typen en daarom ongedefinieerd te retour neren.

```  
SELECT    
    StringToNull("true"), 
    StringToNull(false), 
    StringToNull(undefined),
    StringToNull(NaN) 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{}]
```  

####  <a name="bk_stringtonumber"></a>StringToNumber  
 Retourneert een expressie die is vertaald naar een getal. Als expressie niet kan worden vertaald, retourneert ongedefinieerd.  
  
 **Syntaxis**  
  
```  
StringToNumber(<expr>)  
```  
  
 **Argumenten**  
  
- `expr`  
  
   Is een geldige scalaire expressie die als een JSON-nummer expressie moet worden geëvalueerd. Getallen in JSON moeten een geheel getal of een drijvende komma zijn. Zie [JSON.org](https://json.org/) voor meer informatie over de JSON-indeling.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie of een niet-gedefinieerde waarde.  
  
  **Voorbeelden**  
  
  In het volgende voor beeld ziet u hoe StringToNumber zich gedraagt in verschillende typen. 

Witruimte is alleen toegestaan vóór of na het getal.

```  
SELECT 
    StringToNumber("1.000000") AS num1, 
    StringToNumber("3.14") AS num2,
    StringToNumber("   60   ") AS num3, 
    StringToNumber("-1.79769e+308") AS num4
```  
  
 Hier volgt de resultatenset.  
  
```  
{{"num1": 1, "num2": 3.14, "num3": 60, "num4": -1.79769e+308}}
```  

In JSON moet een geldig getal ofwel een geheel getal zijn of een getal met een drijvende komma.

```  
SELECT   
    StringToNumber("0xF")
```  
  
 Hier volgt de resultatenset.  
  
```  
{{}}
```  

De door gegeven expressie wordt geparseerd als een numerieke expressie. deze invoer kan niet worden geëvalueerd om een getal te typen en daarom ongedefinieerd te retour neren. 

```  
SELECT 
    StringToNumber("99     54"),   
    StringToNumber(undefined),
    StringToNumber("false"),
    StringToNumber(false),
    StringToNumber(" "),
    StringToNumber(NaN)
```  
  
 Hier volgt de resultatenset.  
  
```  
{{}}
```  

####  <a name="bk_stringtoobject"></a> StringToObject  
 Retourneert een expressie die is vertaald naar een object. Als expressie niet kan worden vertaald, retourneert ongedefinieerd.  
  
 **Syntaxis**  
  
```  
StringToObject(<expr>)  
```  
  
 **Argumenten**  
  
- `expr`  
  
   Is een geldige scalaire expressie die moet worden geëvalueerd als een JSON-object expressie. Houd er rekening mee dat geneste teken reeks waarden moeten worden geschreven met dubbele aanhalings tekens die geldig zijn. Zie [JSON.org](https://json.org/) voor meer informatie over de JSON-indeling.  
  
  **Typen retourneren**  
  
  Retourneert een object expressie of een niet-gedefinieerde waarde.  
  
  **Voorbeelden**  
  
  In het volgende voor beeld ziet u hoe StringToObject zich gedraagt in verschillende typen. 
  
 Hier volgen enkele voor beelden met geldige invoer.

``` 
SELECT 
    StringToObject("{}") AS obj1, 
    StringToObject('{"A":[1,2,3]}') AS obj2,
    StringToObject('{"B":[{"b1":[5,6,7]},{"b2":8},{"b3":9}]}') AS obj3, 
    StringToObject("{\"C\":[{\"c1\":[5,6,7]},{\"c2\":8},{\"c3\":9}]}") AS obj4
``` 

Hier volgt de resultatenset.

```
[{"obj1": {}, 
  "obj2": {"A": [1,2,3]}, 
  "obj3": {"B":[{"b1":[5,6,7]},{"b2":8},{"b3":9}]},
  "obj4": {"C":[{"c1":[5,6,7]},{"c2":8},{"c3":9}]}}]
```

 Hier volgen enkele voor beelden met ongeldige invoer.
Hoewel ze geldig zijn binnen een query, worden ze niet geparseerd naar geldige objecten. Teken reeksen in de teken reeks van een object moeten worden voorafgegaan door\\' {\\' a\\':\\' str '} ' of de omliggende aanhaling moet één ' {' a ': ' str '} ' zijn.

Enkele aanhalings tekens rond eigenschapnamen zijn geen geldige JSON-namen.

``` 
SELECT 
    StringToObject("{'a':[1,2,3]}")
```

Hier volgt de resultatenset.

```  
[{}]
```  

Eigenschaps namen zonder omringende aanhalings tekens zijn geen geldige JSON.

``` 
SELECT 
    StringToObject("{a:[1,2,3]}")
```

Hier volgt de resultatenset.

```  
[{}]
``` 

Hier volgen enkele voor beelden met ongeldige invoer.

 De door gegeven expressie wordt geparseerd als een JSON-object. deze invoer levert geen object op en retourneert dus niet-gedefinieerd.

``` 
SELECT 
    StringToObject("}"),
    StringToObject("{"),
    StringToObject("1"),
    StringToObject(NaN), 
    StringToObject(false), 
    StringToObject(undefined)
``` 
 
 Hier volgt de resultatenset.

```
[{}]
```

####  <a name="bk_substring"></a> DE SUBTEKENREEKS  
 Onderdeel van een tekenreeksexpressie vanaf de op nul gebaseerde positie van het opgegeven teken geretourneerd en blijft aan de opgegeven lengte of aan het einde van de tekenreeks.  
  
 **Syntaxis**  
  
```  
SUBSTRING(<str_expr>, <num_expr>, <num_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
- `num_expr`  
  
   Is een geldige numerieke expressie om het begin-en eind teken aan te duiden.    
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld retourneert de subtekenreeks van "abc" beginnen op 1 en een lengte van 1 teken bevatten.  
  
```  
SELECT SUBSTRING("abc", 1, 1) AS substring  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"substring": "b"}]  
```  
####  <a name="bk_tostring"></a> ToString  
 Retourneert een tekenreeksrepresentatie van de scalaire expressie. 
  
 **Syntaxis**  
  
```  
ToString(<expr>)
```  
  
 **Argumenten**  
  
- `expr`  
  
   Is geldige scalaire expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld laat zien hoe ToString gedraagt zich op verschillende typen.   
  
```  
SELECT 
    ToString(1.0000) AS str1, 
    ToString("Hello World") AS str2, 
    ToString(NaN) AS str3, 
    ToString(Infinity) AS str4,
    ToString(IS_STRING(ToString(undefined))) AS str5, 
    ToString(0.1234) AS str6, 
    ToString(false) AS str7, 
    ToString(undefined) AS str8
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"str1": "1", "str2": "Hello World", "str3": "NaN", "str4": "Infinity", "str5": "false", "str6": "0.1234", "str7": "false"}]  
```  
 De volgende invoer gegeven:
```  
{"Products":[{"ProductID":1,"Weight":4,"WeightUnits":"lb"},{"ProductID":2,"Weight":32,"WeightUnits":"kg"},{"ProductID":3,"Weight":400,"WeightUnits":"g"},{"ProductID":4,"Weight":8999,"WeightUnits":"mg"}]}
```    
 Het volgende voorbeeld laat zien hoe ToString met andere tekenreeksfuncties zoals CONCAT kan worden gebruikt.   
 
```  
SELECT 
CONCAT(ToString(p.Weight), p.WeightUnits) 
FROM p in c.Products 
```  

Hier volgt de resultatenset.  
  
```  
[{"$1":"4lb" },
{"$1":"32kg"},
{"$1":"400g" },
{"$1":"8999mg" }]

```  
De volgende invoer wordt gegeven.
```
{"id":"08259","description":"Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX","nutrients":[{"id":"305","description":"Caffeine","units":"mg"},{"id":"306","description":"Cholesterol, HDL","nutritionValue":30,"units":"mg"},{"id":"307","description":"Sodium, NA","nutritionValue":612,"units":"mg"},{"id":"308","description":"Protein, ABP","nutritionValue":60,"units":"mg"},{"id":"309","description":"Zinc, ZN","nutritionValue":null,"units":"mg"}]}
```
Het volgende voorbeeld laat zien hoe ToString kan worden gebruikt met een andere tekenreeks-functies, zoals vervangen.   
```
SELECT 
    n.id AS nutrientID,
    REPLACE(ToString(n.nutritionValue), "6", "9") AS nutritionVal
FROM food 
JOIN n IN food.nutrients
```
Hier volgt de resultatenset.  
 ```
[{"nutrientID":"305"},
{"nutrientID":"306","nutritionVal":"30"},
{"nutrientID":"307","nutritionVal":"912"},
{"nutrientID":"308","nutritionVal":"90"},
{"nutrientID":"309","nutritionVal":"null"}]
``` 
 
####  <a name="bk_trim"></a> TRIM  
 Retourneert een tekenreeksexpressie na het verwijderen van voorloopspaties en volgspaties.  
  
 **Syntaxis**  
  
```  
TRIM(<str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe u TRIM binnen een query.  
  
```  
SELECT TRIM("   abc") AS t1, TRIM("   abc   ") AS t2, TRIM("abc   ") AS t3, TRIM("abc") AS t4
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"t1": "abc", "t2": "abc", "t3": "abc", "t4": "abc"}]  
``` 
####  <a name="bk_upper"></a> BOVENSTE  
 Retourneert een tekenreeksexpressie na het converteren van tekens in kleine letters naar hoofdletters.  
  
 **Syntaxis**  
  
```  
UPPER(<str_expr>)  
```  
  
 **Argumenten**  
  
- `str_expr`  
  
   Is een geldige tekenreeks-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een tekenreeksexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld laat zien hoe het gebruik van hoofdletters in een query  
  
```  
SELECT UPPER("Abc") AS upper  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"upper": "ABC"}]  
```

## <a id="array-functions"></a>Matrix functies

De volgende scalaire functies voeren een bewerking uit op een invoer waarde van een matrix en retour neren numerieke, Booleaanse of matrix waarde:
  
||||  
|-|-|-|  
|[ARRAY_CONCAT](#bk_array_concat)|[ARRAY_CONTAINS](#bk_array_contains)|[ARRAY_LENGTH](#bk_array_length)|  
|[ARRAY_SLICE](#bk_array_slice)|||  
  
####  <a name="bk_array_concat"></a> ARRAY_CONCAT  
 Retourneert een matrix die het resultaat is van het samenvoegen van twee of meer matrixwaarden.  
  
 **Syntaxis**  
  
```  
ARRAY_CONCAT (<arr_expr>, <arr_expr> [, <arr_expr>])  
```  
  
 **Argumenten**  
  
- `arr_expr`  
  
   Is een geldige matrix-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een matrixexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld over het samenvoegen van twee matrices.  
  
```  
SELECT ARRAY_CONCAT(["apples", "strawberries"], ["bananas"]) AS arrayConcat 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"arrayConcat": ["apples", "strawberries", "bananas"]}]  
```  
  
####  <a name="bk_array_contains"></a> ARRAY_CONTAINS  
Retourneert een Booleaanse waarde die aangeeft of de matrix de opgegeven waarde bevat. U kunt controleren op een gedeeltelijke of volledige overeenkomst van een object met behulp van een Boole-expressie in de opdracht. 

**Syntaxis**  
  
```  
ARRAY_CONTAINS (<arr_expr>, <expr> [, bool_expr])  
```  
  
 **Argumenten**  
  
- `arr_expr`  
  
   Is een geldige matrix-expressie.  
  
- `expr`  
  
   Een geldige expressie is.  

- `bool_expr`  
  
   Een Booleaanse expressie is. Als de eigenschap is ingesteld op ' true'and ' als de opgegeven zoek waarde een object is, wordt met de opdracht gecontroleerd op een gedeeltelijke overeenkomst (het zoek object is een subset van een van de objecten). Als de eigenschap is ingesteld op ' false ', wordt met de opdracht gecontroleerd of alle objecten in de matrix volledig overeenkomen. Als niet wordt opgegeven, is de standaard waarde false. 
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse waarde.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld hoe om te controleren voor lidmaatschap in een matrix met behulp van ARRAY_CONTAINS.  
  
```  
SELECT   
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "apples") AS b1,  
           ARRAY_CONTAINS(["apples", "strawberries", "bananas"], "mangoes") AS b2  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"b1": true, "b2": false}]  
```  

Het volgende voorbeeld hoe om te controleren op een gedeeltelijke overeenkomst van een JSON-code in een matrix met behulp van ARRAY_CONTAINS.  
  
```  
SELECT  
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}, true) AS b1, 
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "apples"}) AS b2,
    ARRAY_CONTAINS([{"name": "apples", "fresh": true}, {"name": "strawberries", "fresh": true}], {"name": "mangoes"}, true) AS b3 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{
  "b1": true,
  "b2": false,
  "b3": false
}] 
```  
  
####  <a name="bk_array_length"></a> ARRAY_LENGTH  
 Retourneert het aantal elementen van de opgegeven matrixexpressie.  
  
 **Syntaxis**  
  
```  
ARRAY_LENGTH(<arr_expr>)  
```  
  
 **Argumenten**  
  
- `arr_expr`  
  
   Is een geldige matrix-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld over het verkrijgen van de lengte van een matrix met behulp van ARRAY_LENGTH.  
  
```  
SELECT ARRAY_LENGTH(["apples", "strawberries", "bananas"]) AS len  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{"len": 3}]  
```  
  
####  <a name="bk_array_slice"></a> ARRAY_SLICE  
 Retourneert een deel van een matrixexpressie.
  
 **Syntaxis**  
  
```  
ARRAY_SLICE (<arr_expr>, <num_expr> [, <num_expr>])  
```  
  
 **Argumenten**  
  
- `arr_expr`  
  
   Is een geldige matrix-expressie.  
  
- `num_expr`  
  
   Numerieke op nul gebaseerde index op waarop u wilt beginnen met de matrix. Negatieve waarden kunnen worden gebruikt om op te geven van de startIndex ten opzichte van het laatste element van de matrix-dat wil zeggen 1 verwijst naar het laatste element in de matrix.  

- `num_expr`  

   Maximum aantal elementen in de resulterende matrix.    

  **Typen retourneren**  
  
  Retourneert een matrixexpressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe u verschillende segmenten van een matrix met behulp van ARRAY_SLICE.  
  
```  
SELECT
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1) AS s1,  
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1) AS s2,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 1) AS s3,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], -2, 2) AS s4,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 0) AS s5,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, 1000) AS s6,
           ARRAY_SLICE(["apples", "strawberries", "bananas"], 1, -100) AS s7      
  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{  
           "s1": ["strawberries", "bananas"],   
           "s2": ["strawberries"],
           "s3": ["strawberries"],  
           "s4": ["strawberries", "bananas"], 
           "s5": [],
           "s6": ["strawberries", "bananas"],
           "s7": [] 
}]  
```  
## <a id="date-time-functions"></a>De functie datum en tijd

Met de volgende scalaire functies kunt u de huidige UTC-datum en-tijd in twee vormen ophalen. een numerieke tijds tempel waarvan de waarde de UNIX-epoche is in milliseconden of als een teken reeks die voldoet aan de ISO 8601-indeling. 

|||
|-|-|
|[GETCURRENTDATETIME](#bk_get_current_date_time)|[GETCURRENTTIMESTAMP](#bk_get_current_timestamp)||

####  <a name="bk_get_current_date_time"></a>GETCURRENTDATETIME
 Retourneert de huidige UTC-datum en-tijd als een ISO 8601-teken reeks.
  
 **Syntaxis**
  
```
GETCURRENTDATETIME ()
```
  
  **Typen retourneren**
  
  Retourneert de huidige UTC-datum en-tijd in ISO 8601-teken reeks waarde. 

  Dit wordt weer gegeven in de notatie JJJJ-MM-DDTuu: mm: SS. sssZ waarbij:
  
  |||
  |-|-|
  |DD|jaar met vier cijfers|
  |MM|maand van twee cijfers (01 = januari, etc.)|
  |DD|2-cijferige dag van de maand (01 tot en met 31)|
  |D|de aanzienlijke voor het begin van de tijd elementen|
  |UU|twee cijfers per uur (00 tot en met 23)|
  |mm|twee cijfer minuten (00 tot en met 59)|
  |SS|twee cijfer seconden (00 tot en met 59)|
  |.sss|drie cijfers van decimale delen van een seconde|
  |Z|UTC (Coordinated Universal Time)||
  
  Zie [ISO_8601](https://en.wikipedia.org/wiki/ISO_8601) voor meer informatie over de ISO 8601-indeling.

  **Opmerkingen**

  GETCURRENTDATETIME is een niet-deterministische functie. 
  
  Het geretourneerde resultaat is UTC (Coordinated Universal Time).

  **Voorbeelden**  
  
  In het volgende voor beeld ziet u hoe u de huidige UTC-datum tijd kunt ophalen met behulp van de ingebouwde functie GetCurrentDateTime.
  
```  
SELECT GETCURRENTDATETIME() AS currentUtcDateTime
```  
  
 Hier volgt een voor beeld van een resultatenset.
  
```  
[{
  "currentUtcDateTime": "2019-05-03T20:36:17.784Z"
}]  
```  

####  <a name="bk_get_current_timestamp"></a>GETCURRENTTIMESTAMP
 Retourneert het aantal milliseconden dat is verstreken sinds 00:00:00 donderdag, 1 januari 1970. 
  
 **Syntaxis**  
  
```  
GETCURRENTTIMESTAMP ()  
```  
  
  **Typen retourneren**  
  
  Retourneert een numerieke waarde, het huidige aantal milliseconden dat is verstreken sinds de UNIX-epoche, dat wil zeggen het aantal milliseconden dat is verstreken sinds 00:00:00 donderdag, 1 januari 1970.

  **Opmerkingen**

  GETCURRENTTIMESTAMP is een niet-deterministische functie.
  
  Het geretourneerde resultaat is UTC (Coordinated Universal Time).

  **Voorbeelden**  
  
  In het volgende voor beeld ziet u hoe u de huidige tijds tempel kunt ophalen met behulp van de ingebouwde functie GetCurrentTimestamp.
  
```  
SELECT GETCURRENTTIMESTAMP() AS currentUtcTimestamp
```  
  
 Hier volgt een voor beeld van een resultatenset.
  
```  
[{
  "currentUtcTimestamp": 1556916469065
}]  
```

## <a id="spatial-functions"></a>Ruimtelijke functies

Cosmos DB biedt ondersteuning voor de volgende ingebouwde OGC-functies (Open Geospatial Consortium) voor georuimtelijke query's. De volgende scalaire functies uitvoeren van een bewerking op een invoerwaarde ruimtelijke index en een numerieke of Booleaanse waarde retourneren.  
  
|||||
|-|-|-|-|
|[ST_DISTANCE](#bk_st_distance)|[ST_WITHIN](#bk_st_within)|[ST_INTERSECTS](#bk_st_intersects)|[ST_ISVALID](#bk_st_isvalid)|
|[ST_ISVALIDDETAILED](#bk_st_isvaliddetailed)||||
  
####  <a name="bk_st_distance"></a> ST_DISTANCE  
 Retourneert de afstand tussen de twee GeoJSON Point-, Polygon- of LineString-expressies.  
  
 **Syntaxis**  
  
```  
ST_DISTANCE (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumenten**  
  
- `spatial_expr`  
  
   Is geldige expressie voor GeoJSON-punt, Polygon of LineString-object.  
  
  **Typen retourneren**  
  
  Retourneert een numerieke expressie met de afstand. Dit wordt uitgedrukt in meters voor de verwijzing van het systeem.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe alle familie documenten die binnen 30 kilometer van de opgegeven locatie met behulp van de ingebouwde functie ST_DISTANCE worden geretourneerd. .  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{  
  "id": "WakefieldFamily"  
}]  
```  
  
####  <a name="bk_st_within"></a> ST_WITHIN  
 Retourneert een Booleaanse expressie die aangeeft of de GeoJSON-object dat in het eerste argument opgegeven (punt, Polygon of LineString) binnen de GeoJSON (punt, Polygon of LineString) in het tweede argument.  
  
 **Syntaxis**  
  
```  
ST_WITHIN (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumenten**  
  
- `spatial_expr`  
  
   Is geldige expressie voor GeoJSON-punt, Polygon of LineString-object.  
 
- `spatial_expr`  
  
   Is geldige expressie voor GeoJSON-punt, Polygon of LineString-object.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse waarde.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe u kunt zoeken naar alle familie documenten binnen een veelhoek met behulp van ST_WITHIN.  
  
```  
SELECT f.id   
FROM Families f   
WHERE ST_WITHIN(f.location, {  
    'type':'Polygon',   
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{ "id": "WakefieldFamily" }]  
```  

####  <a name="bk_st_intersects"></a> ST_INTERSECTS  
 Retourneert een Booleaanse expressie waarmee wordt aangegeven of de GeoJSON-object dat in het eerste argument opgegeven (punt, Polygon of LineString) samen met de GeoJSON (punt, Polygon of LineString) in het tweede argument.  
  
 **Syntaxis**  
  
```  
ST_INTERSECTS (<spatial_expr>, <spatial_expr>)  
```  
  
 **Argumenten**  
  
- `spatial_expr`  
  
   Is geldige expressie voor GeoJSON-punt, Polygon of LineString-object.  
 
- `spatial_expr`  
  
   Is geldige expressie voor GeoJSON-punt, Polygon of LineString-object.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse waarde.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld ziet hoe u kunt alle gebieden die elkaar met de opgegeven veelhoek overlappen vinden.  
  
```  
SELECT a.id
FROM Areas a
WHERE ST_INTERSECTS(a.location, {  
    'type':'Polygon',
    'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]  
})  
```  
  
 Hier volgt de resultatenset.  
  
```  
[{ "id": "IntersectingPolygon" }]  
```  
  
####  <a name="bk_st_isvalid"></a> ST_ISVALID  
 Retourneert een Booleaanse waarde die aangeeft of de opgegeven GeoJSON Point-, Polygon- of LineString-expressie geldig is.  
  
 **Syntaxis**  
  
```  
ST_ISVALID(<spatial_expr>)  
```  
  
 **Argumenten**
  
- `spatial_expr`  
  
   Is geldige GeoJSON-punt, Polygon of LineString-expressie.  
  
  **Typen retourneren**  
  
  Retourneert een Booleaanse expressie.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld laat zien hoe om te controleren of een punt geldig met behulp van ST_VALID is.  
  
  Bijvoorbeeld, heeft dit punt een Breedtegraadwaarde die zich niet in het geldige waardenbereik [tussen-90 en, 90], zodat de query retourneert ' false '.  
  
  Voor polygonen, de GeoJSON-specificatie is vereist dat de opgegeven combinatie van laatste coördinaat moet hetzelfde zijn als de eerste pagina, het maken van een gesloten vorm. Punten in een polygoon moeten worden opgegeven in volgorde van linksom draaien. Een veelhoek in rechtsom volgorde opgegeven vertegenwoordigt de omgekeerde waarde van de regio binnen het.  
  
```  
SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] }) AS b 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{ "b": false }]  
```  
  
####  <a name="bk_st_isvaliddetailed"></a> ST_ISVALIDDETAILED  
 Retourneert een JSON-waarde met een Booleaanse waarde die aangeeft of de opgegeven GeoJSON Point-, Polygon- of LineString-expressie geldig is. Als de expressie ongeldig is, worden ook de reden daarvoor en een tekenreekswaarde geretourneerd.  
  
 **Syntaxis**  
  
```  
ST_ISVALIDDETAILED(<spatial_expr>)  
```  
  
 **Argumenten**  
  
- `spatial_expr`  
  
   Een geldige GeoJSON-veelhoek-punt of de expressie is.  
  
  **Typen retourneren**  
  
  Retourneert een JSON-waarde met een Booleaanse waarde als de opgegeven GeoJSON-veelhoek-punt of de expressie is geldig, en als ongeldig, ook de reden als een string-waarde.  
  
  **Voorbeelden**  
  
  Het volgende voorbeeld hoe u controleert de geldigheid (met gegevens) met behulp van ST_ISVALIDDETAILED.  
  
```  
SELECT ST_ISVALIDDETAILED({   
  "type": "Polygon",   
  "coordinates": [[ [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] ]]  
}) AS b 
```  
  
 Hier volgt de resultatenset.  
  
```  
[{  
  "b": {
    "valid": false,
    "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a polygon must have the same start and end points."   
  }  
}]  
```  

## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Cosmos DB](introduction.md)
- [UDFs](sql-query-udfs.md)
- [Aggregates](sql-query-aggregates.md)
