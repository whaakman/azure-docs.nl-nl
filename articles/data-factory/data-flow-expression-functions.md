---
title: Functies van de expressie in de gegevensstroom Mapping-functie van Azure Data Factory
description: Meer informatie over functies in de gegevensstroom toewijzen.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/15/2019
ms.openlocfilehash: 6b4df70114dd481566ae1a666c91c1c9b5bad86f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64717015"
---
# <a name="data-transformation-expressions-in-mapping-data-flow"></a>Data transformation expressies in de gegevensstroom toewijzen 

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

## <a name="expression-functions"></a>Expressie functies

In Data Factory, gebruikt u de expressietaal van de functie voor toewijzing gegevensstroom gegevenstransformaties configureren.

*********************************
<code>abs</code>
==============================
<code><b>abs(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Deze expressie markeert de positieve absolute waarde van een combinatie van cijfers.
* ``abs(-20) -> 20``
* ``abs(10) -> 10``
*********************************
<code>acos</code>
==============================
<code><b>acos(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie een inverse cosinus-waarde wordt berekend.
* ``acos(1) -> 0.0``
*********************************
<code>add</code>
==============================
<code><b>add(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Deze expressie voegt twee tekenreeksen of getallen. Een datum wordt toegevoegd aan een aantal dagen. Wordt een matrix met soortgelijke toegevoegd aan een andere. 
* ``add(10, 20) -> 30``
* ``10 + 20 -> 30``
* ``add('ice', 'cream') -> 'icecream'``
* ``'ice' + 'cream' + ' cone' -> 'icecream cone'``
* ``add(toDate('2012-12-12'), 3) -> 2012-12-15 (date value)``
* ``toDate('2012-12-12') + 3 -> 2012-12-15 (date value)``
* ``[10, 20] + [30, 40] => [10, 20, 30, 40]``

De **toevoegen** operator is hetzelfde als de **+** operator.
*********************************
<code>addDays</code>
==============================
<code><b>addDays(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;days to add&gt;</i> : integral) => datetime</b></code><br/><br/>
Deze expressie voegt dagen toe aan een datum of tijdstempel. 
* ``addDays(toDate('2016-08-08'), 1) -> 2016-08-09``

De **addDays** operator is hetzelfde als de **+** operator voor datums.
*********************************
<code>addMonths</code>
==============================
<code><b>addMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to add&gt;</i> : integral) => datetime</b></code><br/><br/>
Deze expressie wordt maanden toegevoegd aan een datum of tijdstempel.
* ``addMonths(toDate('2016-08-31'), 1) -> 2016-09-30``
* ``addMonths(toTimestamp('2016-09-30 10:10:10'), -1) -> 2016-08-31 10:10:10``
*********************************
<code>and</code>
==============================
<code><b>and(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Dit is een logische **en** operator. Dit is hetzelfde als de **&&** operator.
* ``and(true, false) -> false``
* ``true && false -> false``
*********************************
<code>asin</code>
==============================
<code><b>asin(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie berekent een inverse sinus-waarde.
* ``asin(0) -> 0.0``
*********************************
<code>atan</code>
==============================
<code><b>atan(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie berekend een inverse tangens waarde.
* ``atan(0) -> 0.0``
*********************************
<code>atan2</code>
==============================
<code><b>atan2(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie retourneert de hoek in radialen tussen de positieve x-as van een beheerlaag en het moment dat de coördinaten geven.
* ``atan2(0, 0) -> 0.0``
*********************************
<code>avg</code>
==============================
<code><b>avg(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Deze expressie wordt het gemiddelde van waarden van een kolom.
* ``avg(sales) -> 7523420.234``
*********************************
<code>avgIf</code>
==============================
<code><b>avgIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Deze expressie op basis van een criterium, wordt het gemiddelde van waarden van een kolom opgehaald.
* ``avgIf(region == 'West', sales) -> 7523420.234``
*********************************
<code>byName</code>
==============================
<code><b>byName(<i>&lt;column name&gt;</i> : string) => any</b></code><br/><br/>
Deze expressie wordt een waarde in de kolom geselecteerd door de naam in de stroom. Als er meerdere overeenkomsten, wordt de eerste overeenkomst die wordt geretourneerd. Als er geen overeenkomst is, wordt in de expressie een NULL-waarde geretourneerd. De geretourneerde waarde moet type omgezet door een van de functies voor typeconversie (TO_DATE, TO_STRING...). De namen van kolommen waarvan bekend is dat tijdens de ontwerpfase moeten worden opgelost door hun naam. Berekende invoer worden niet ondersteund, maar kunt u vervangingen parameter.

* ``toString(byName('parent')) -> appa``
* ``toLong(byName('income')) -> 9000000000009``
* ``toBoolean(byName('foster')) -> false``
* ``toLong(byName($debtCol)) -> 123456890``
* ``birthDate -> 12/31/2050``
* ``toString(byName('Bogus Column')) -> NULL``
*********************************
<code>byPosition</code>
==============================
<code><b>byPosition(<i>&lt;position&gt;</i> : integer) => any</b></code><br/><br/>
Deze expressie wordt een waarde in de kolom met de relatieve positie (1-indeling) in de stroom geselecteerd. Als de positie buiten het bereik is, wordt er een NULL-waarde. De geretourneerde waarde moet type omgezet door een van de functies voor typeconversie (TO_DATE, TO_STRING, enzovoort). Berekende invoer worden niet ondersteund, maar kunt u vervangingen parameter.

* ``toString(byPosition(1)) -> amma``
* ``toDecimal(byPosition(2), 10, 2) -> 199990.99``
* ``toBoolean(byName(4)) -> false``
* ``toString(byName($colName)) -> family``
* ``toString(byPosition(1234)) -> NULL``
*********************************
<code>case</code>
==============================
<code><b>case(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, <i>&lt;false_expression&gt;</i> : any, ...) => any</b></code><br/><br/>
Op basis van voorwaarden afwisselende, geldt deze expressie één waarde of de andere. 
* ``case(custType == 'Premium', 10, 4.5)``
* ``case(custType == 'Premium', price*0.95, custType == 'Elite',   price*0.9, price*2)``
* ``case(dayOfWeek(saleDate) == 1, 'Sunday', dayOfWeek(saleDate) == 6, 'Saturday')``

Als het aantal invoerbewerkingen zelfs is, is de andere waarde NULL voor de laatste voorwaarde.
*********************************
<code>cbrt</code>
==============================
<code><b>cbrt(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie berekent de hoofdmap van de kubus van een getal.
* ``cbrt(8) -> 2.0``
*********************************
<code>ceil</code>
==============================
<code><b>ceil(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Deze expressie retourneert het kleinste gehele getal dat is niet kleiner zijn dan het getal.
* ``ceil(-0.1) -> 0``
*********************************
<code>concat</code>
==============================
<code><b>concat(<i>&lt;this&gt;</i> : string, <i>&lt;that&gt;</i> : string, ...) => string</b></code><br/><br/>
Deze expressie worden samengevoegd in een variabele aantal tekenreeksen. De **concat** operator is hetzelfde als de **+** operator met tekenreeksen.
* ``concat('Awesome', 'Cool', 'Product') -> 'AwesomeCoolProduct'``
* ``'Awesome' + 'Cool' + 'Product' -> 'AwesomeCoolProduct'``
* ``concat(addrLine1, ' ', addrLine2, ' ', city, ' ', state, ' ', zip)``
* ``addrLine1 + ' ' + addrLine2 + ' ' + city + ' ' + state + ' ' + zip``
*********************************
<code>concatWS</code>
==============================
<code><b>concatWS(<i>&lt;separator&gt;</i> : string, <i>&lt;this&gt;</i> : string, <i>&lt;that&gt;</i> : string, ...) => string</b></code><br/><br/>
Deze expressie worden samengevoegd in een variabele aantal tekenreeksen samen met een scheidingsteken. De eerste parameter is het scheidingsteken.
* ``concatWS(' ', 'Awesome', 'Cool', 'Product') -> 'Awesome Cool Product'``
* ``concatWS(' ' , addrLine1, addrLine2, city, state, zip) ->``
* ``concatWS(',' , toString(order_total), toString(order_discount))``
*********************************
<code>cos</code>
==============================
<code><b>cos(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie een cosinuswaarde wordt berekend.
* ``cos(10) -> -0.83907152907``
*********************************
<code>cosh</code>
==============================
<code><b>cosh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie wordt een hyperbolische cosinus van een waarde berekend.
* ``cosh(0) -> 1.0``
*********************************
<code>count</code>
==============================
<code><b>count([<i>&lt;value1&gt;</i> : any]) => long</b></code><br/><br/>
Deze expressie wordt het totale aantal waarden opgehaald. Als de optionele kolom of kolommen opgegeven, NULL zijn worden-waarden in de telling genegeerd.
* ``count(custId) -> 100``
* ``count(custId, custName) -> 50``
* ``count() -> 125``
* ``count(iif(isNull(custId), 1, NULL)) -> 5``
*********************************
<code>countDistinct</code>
==============================
<code><b>countDistinct(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : any], ...) => long</b></code><br/><br/>
Deze expressie Hiermee haalt u het totale aantal afzonderlijke waarden van een set kolommen.
* ``countDistinct(custId, custName) -> 60``
*********************************
<code>countIf</code>
==============================
<code><b>countIf(<i>&lt;value1&gt;</i> : boolean, [<i>&lt;value2&gt;</i> : any]) => long</b></code><br/><br/>
Deze expressie op basis van een criterium, wordt het totale aantal waarden opgehaald. Als de optionele kolom opgegeven, NULL is worden-waarden in de telling genegeerd.
* ``countIf(state == 'CA' && commission < 10000, name) -> 100``
*********************************
<code>covariancePopulation</code>
==============================
<code><b>covariancePopulation(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie haalt de populatiecovariantie tussen de twee kolommen.
* ``covariancePopulation(sales, profit) -> 122.12``
*********************************
<code>covariancePopulationIf</code>
==============================
<code><b>covariancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie op basis van een criterium, wordt de populatiecovariantie van twee kolommen opgehaald.
* ``covariancePopulationIf(region == 'West', sales) -> 122.12``
*********************************
<code>covarianceSample</code>
==============================
<code><b>covarianceSample(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie wordt de voorbeeld-covariantie van twee kolommen opgehaald.
* ``covarianceSample(sales, profit) -> 122.12``
*********************************
<code>covarianceSampleIf</code>
==============================
<code><b>covarianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number, <i>&lt;value3&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie op basis van een criterium, wordt de voorbeeld-covariantie van twee kolommen opgehaald.
* ``covarianceSampleIf(region == 'West', sales, profit) -> 122.12``
*********************************
<code>crc32</code>
==============================
<code><b>crc32(<i>&lt;value1&gt;</i> : any, ...) => long</b></code><br/><br/>
Deze expressie berekent de hash CRC32 van een set kolommen van verschillende primitieve gegevenstypen, krijgt een bitlengte waarvan de waarden alleen 0(256) mogen 224, 256, 384, 512. U kunt de **crc32** operator voor het berekenen van een vingerafdruk voor een rij.
* ``crc32(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 3630253689``
*********************************
<code>cumeDist</code>
==============================
<code><b>cumeDist() => integer</b></code><br/><br/>
Deze functie berekent de positie van een waarde ten opzichte van alle waarden in de partitie. Het resultaat is het aantal rijen is voorgaande of gelijk zijn aan de huidige rij in de volgorde van de partitie, gedeeld door het totale aantal rijen in de partitie venster. Eventuele tie-waarden in de volgorde retourneren dezelfde positie.
* ``cumeDist() -> 1``
*********************************
<code>currentDate</code>
==============================
<code><b>currentDate([<i>&lt;value1&gt;</i> : string]) => date</b></code><br/><br/>
Deze expressie haalt de huidige datum wanneer de taak wordt gestart om uit te voeren. 
* ``currentDate() -> 12-12-2030``
* ``currentDate('PST') -> 12-31-2050``

U kunt een optionele tijdzone doorgeven in de vorm van `'GMT'`, `'PST'`, `'UTC'`, `'America/Cayman'`. De lokale tijdzone is de standaardinstelling.
*********************************
<code>currentTimestamp</code>
==============================
<code><b>currentTimestamp() => timestamp</b></code><br/><br/>
Deze expressie haalt de huidige tijdstempel wanneer de taak wordt gestart om uit te voeren met de lokale tijdzone.
* ``currentTimestamp() -> 12-12-2030T12:12:12``
*********************************
<code>currentUTC</code>
==============================
<code><b>currentUTC([<i>&lt;value1&gt;</i> : string]) => timestamp</b></code><br/><br/>
Deze expressie haalt de huidige de tijdstempel als UTC. 
* ``currentUTC() -> 12-12-2030T19:18:12``
* ``currentUTC('Asia/Seoul') -> 12-13-2030T11:18:12``

U kunt een optionele tijdzone doorgeven in de vorm van `'GMT'`, `'PST'`, `'UTC'`, `'America/Cayman'`. De lokale tijdzone is de standaardinstelling.
*********************************
<code>dayOfMonth</code>
==============================
<code><b>dayOfMonth(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Deze expressie opgegeven een datum, wordt de dag van de maand opgehaald.
* ``dayOfMonth(toDate('2018-06-08')) -> 08``
*********************************
<code>dayOfWeek</code>
==============================
<code><b>dayOfWeek(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Deze expressie opgegeven een datum, wordt de dag van de week opgehaald. 
* ``dayOfWeek(toDate('2018-06-08')) -> 7``

Dagwaarden zijn als volgt:
- 1 - Sunday
- 2 - maandag 
- ...
- 7: zaterdag
*********************************
<code>dayOfYear</code>
==============================
<code><b>dayOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Deze expressie opgegeven een datum, wordt de dag van het jaar opgehaald.
* ``dayOfYear(toDate('2016-04-09')) -> 100``
*********************************
<code>degrees</code>
==============================
<code><b>degrees(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie converteert radialen naar graden.
* ``degrees(3.141592653589793) -> 180``
*********************************
<code>denseRank</code>
==============================
<code><b>denseRank(<i>&lt;value1&gt;</i> : any, ...) => integer</b></code><br/><br/>
Deze expressie berekent de positie van een waarde in een groep met waarden. Het resultaat is een plus het aantal rijen is voorgaande of gelijk zijn aan de huidige rij in de volgorde van de partitie. De waarden wordt niet produceren hiaten in de reeks. 
* ``denseRank(salesQtr, salesAmt) -> 1``

De **denseRank** operator werkt, zelfs wanneer gegevens niet worden gesorteerd. Een wijziging in waarden zoekt.
*********************************
<code>divide</code>
==============================
<code><b>divide(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Deze expressie deelt twee getallen. De **verdelen** operator is hetzelfde als de **/** operator.
* ``divide(20, 10) -> 2``
* ``20 / 10 -> 2``
*********************************
<code>endsWith</code>
==============================
<code><b>endsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></code><br/><br/>
Deze expressie wordt gecontroleerd of de tekenreeks met de opgegeven tekenreeks eindigt.
* ``endsWith('great', 'eat') -> true``
*********************************
<code>equals</code>
==============================
<code><b>equals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Dit is een vergelijkingsoperator is gelijk aan. Dit is hetzelfde als de **==** operator.
* ``equals(12, 24) -> false``
* ``12==24 -> false``
* ``'bad'=='bad' -> true``
* ``'good'== NULL -> false``
* ``NULL===NULL -> false``
*********************************
<code>equalsIgnoreCase</code>
==============================
<code><b>equalsIgnoreCase(<i>&lt;value1&gt;</i> : string, <i>&lt;value2&gt;</i> : string) => boolean</b></code><br/><br/>
De **equalsIgnoreCase** operator is een vergelijkingsoperator is gelijk aan die aanvraag wordt genegeerd. Dit is hetzelfde als de **<=>** operator.
* ``'abc'=='abc' -> true``
* ``equalsIgnoreCase('abc', 'Abc') -> true``
*********************************
<code>factorial</code>
==============================
<code><b>factorial(<i>&lt;value1&gt;</i> : number) => long</b></code><br/><br/>
Deze expressie berekent de faculteit van een getal.
* ``factorial(5) -> 120``
*********************************
<code>false</code>
==============================
<code><b>false() => boolean</b></code><br/><br/>
Deze expressie wordt altijd retourneert een waarde ONWAAR. 
* ``isDiscounted == false()``
* ``isDiscounted() == false``

Gebruik de `syntax(false())` functioneren als een kolom met de naam *false*.
*********************************
<code>first</code>
==============================
<code><b>first(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></code><br/><br/>
Deze expressie Hiermee haalt u de eerste waarde van de kolomgroep van een. Als u de tweede parameter weglaat `ignoreNulls`, wordt ervan uitgegaan dat op onwaar zijn ingesteld.
* ``first(sales) -> 12233.23``
* ``first(sales, false) -> NULL``
*********************************
<code>floor</code>
==============================
<code><b>floor(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Deze expressie retourneert het grootste gehele getal dat is niet groter zijn dan het aantal.
* ``floor(-0.1) -> -1``
*********************************
<code>fromUTC</code>
==============================
<code><b>fromUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></code><br/><br/>
Deze expressie converteert naar de tijdstempel van UTC. U kunt eventueel de tijdzone doorgeven in de vorm van `'GMT'`, `'PST'`, `'UTC'`, `'America/Cayman'`. De standaardwaarde is de huidige tijdzone.

* ``fromUTC(currentTimeStamp()) -> 12-12-2030T19:18:12``
* ``fromUTC(currentTimeStamp(), 'Asia/Seoul') -> 12-13-2030T11:18:12``
*********************************
<code>greater</code>
==============================
<code><b>greater(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Dit is een vergelijking **groter** operator. Dit is hetzelfde als de **>** operator.
* ``greater(12, 24) -> false``
* ``'abcd' > 'abc' -> true``
*********************************
<code>greaterOrEqual</code>
==============================
<code><b>greaterOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Dit is een vergelijkingsoperator groter-dan-of-gelijk. Deze operator is hetzelfde als de **>=** operator.
* ``greaterOrEqual(12, 12) -> false``
* ``'abcd' >= 'abc' -> true``
*********************************
<code>greatest</code>
==============================
<code><b>greatest(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Deze expressie retourneert de grootste waarde in de lijst met waarden als invoer. Het resultaat NULL als alle invoer NULL is.
* ``greatest(10, 30, 15, 20) -> 30``
* ``greatest(toDate('12/12/2010'), toDate('12/12/2011'), toDate('12/12/2000')) -> '12/12/2011'``
*********************************
<code>hour</code>
==============================
<code><b>hour(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Deze expressie Hiermee haalt u de uurwaarde van een tijdstempel. 
* ``hour(toTimestamp('2009-07-30T12:58:59')) -> 12``
* ``hour(toTimestamp('2009-07-30T12:58:59'), 'PST') -> 12``

U kunt een optionele tijdzone doorgeven in de vorm van `'GMT'`, `'PST'`, `'UTC'`, `'America/Cayman'`. De lokale tijdzone is de standaardinstelling.
*********************************
<code>iif</code>
==============================
<code><b>iif(<i>&lt;condition&gt;</i> : boolean, <i>&lt;true_expression&gt;</i> : any, [<i>&lt;false_expression&gt;</i> : any]) => any</b></code><br/><br/>
Op basis van een voorwaarde, geldt deze expressie één waarde of de andere. Als de andere waarde opgegeven is, wordt deze beschouwd als NULL. Beide waarden moeten compatibel zijn (numeriek of tekenreeks, bijvoorbeeld).
* ``iif(custType == 'Premium', 10, 4.5)``
* ``iif(amount > 100, 'High')``
* ``iif(dayOfWeek(saleDate) == 6, 'Weekend', 'Weekday')``
*********************************
<code>in</code>
==============================
<code><b>in(<i>&lt;array of items&gt;</i> : array, <i>&lt;item to find&gt;</i> : any) => boolean</b></code><br/><br/>
Deze expressie controleert op een item in de matrix.
* ``in([10, 20, 30], 10) -> true``
* ``in(['good', 'kid'], 'bad') -> false``
*********************************
<code>initCap</code>
==============================
<code><b>initCap(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Deze expressie converteert de eerste letter van elk woord naar hoofdletters. Woorden worden aangeduid met spatietekens ontslag nemen.
* ``initCap('cool iceCREAM') -> 'Cool IceCREAM'``
*********************************
<code>instr</code>
==============================
<code><b>instr(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string) => integer</b></code><br/><br/>
Deze expressie wordt gezocht naar de positie (gebaseerd op 1) van de subtekenreeks in een tekenreeks. Als de positie wordt gevonden, wordt 0 geretourneerd. 
* ``instr('great', 'eat') -> 3``
* ``instr('microsoft', 'o') -> 7``
* ``instr('good', 'bad') -> 0``
*********************************
<code>isDelete</code>
==============================
<code><b>isDelete([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Deze expressie controleert op rijen die zijn gemarkeerd voor verwijdering. 
* ``isDelete() -> true``
* ``isDelete(1) -> false``

Als uw transformatie stream met meer dan één invoer heeft, kunt u de index (1-indeling) van de stroom doorgeven. De standaardwaarde voor de stream-index is 1.
*********************************
<code>isError</code>
==============================
<code><b>isError([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Deze expressie controleert op rijen die zijn gemarkeerd als fout.
* ``isError() -> true``
* ``isError(1) -> false``

Als uw transformatie stream met meer dan één invoer heeft, kunt u de index (1-indeling) van de stroom doorgeven. De standaardwaarde voor de stream-index is 1.
*********************************
<code>isIgnore</code>
==============================
<code><b>isIgnore([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Deze expressie controleert of er rijen gemarkeerd om te worden genegeerd.
* ``isIgnore() -> true``
* ``isIgnore(1) -> false``

Als uw transformatie stream met meer dan één invoer heeft, kunt u de index (1-indeling) van de stroom doorgeven. De standaardwaarde voor de stream-index is 1.
*********************************
<code>isInsert</code>
==============================
<code><b>isInsert([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Deze expressie controleert op rijen die zijn gemarkeerd voor invoeging. 
* ``isInsert() -> true``
* ``isInsert(1) -> false``

Als uw transformatie stream met meer dan één invoer heeft, kunt u de index (1-indeling) van de stroom doorgeven. De standaardwaarde voor de stream-index is 1.
*********************************
<code>isMatch</code>
==============================
<code><b>isMatch([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Deze expressie controleert of er rijen overeenkomen op opzoeken. 
* ``isMatch() -> true``
* ``isMatch(1) -> false``

Als uw transformatie stream met meer dan één invoer heeft, kunt u de index (1-indeling) van de stroom doorgeven. De standaardwaarde voor de stream-index is 1.
*********************************
<code>isNull</code>
==============================
<code><b>isNull(<i>&lt;value1&gt;</i> : any) => boolean</b></code><br/><br/>
Deze expressie controleert of er een NULL-waarde.
* ``isNull(NULL()) -> true``
* ``isNull('') -> false'``
*********************************
<code>isUpdate</code>
==============================
<code><b>isUpdate([<i>&lt;value1&gt;</i> : integer]) => boolean</b></code><br/><br/>
Deze expressie controleert op rijen die zijn gemarkeerd voor update. 
* ``isUpdate() -> true``
* ``isUpdate(1) -> false``

Als uw transformatie stream met meer dan één invoer heeft, kunt u de index (1-indeling) van de stroom doorgeven. De standaardwaarde voor de stream-index is 1.
*********************************
<code>kurtosis</code>
==============================
<code><b>kurtosis(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie haalt de kurtosis van een kolom.
* ``kurtosis(sales) -> 122.12``
*********************************
<code>kurtosisIf</code>
==============================
<code><b>kurtosisIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criterium, haalt deze expressie de kurtosis van een kolom.
* ``kurtosisIf(region == 'West', sales) -> 122.12``
*********************************
<code>lag</code>
==============================
<code><b>lag(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look before&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></code><br/><br/>
Deze expressie wordt de waarde van de eerste parameter geëvalueerd *n* rijen voor de huidige rij. De tweede parameter is het aantal rijen om terug te kijken. De standaardwaarde is 1. Als er niet zo veel rijen, wordt een waarde NULL geretourneerd als er een standaardwaarde is opgegeven.
* ``lag(amount, 2) -> 60``
* ``lag(amount, 2000, 100) -> 100``
*********************************
<code>last</code>
==============================
<code><b>last(<i>&lt;value1&gt;</i> : any, [<i>&lt;value2&gt;</i> : boolean]) => any</b></code><br/><br/>
Deze expressie haalt de laatste waarde van de kolomgroep van een. Als u de tweede parameter weglaat `ignoreNulls`, de expressie retourneert `false`.
* ``last(sales) -> 523.12``
* ``last(sales, false) -> NULL``
*********************************
<code>lastDayOfMonth</code>
==============================
<code><b>lastDayOfMonth(<i>&lt;value1&gt;</i> : datetime) => date</b></code><br/><br/>
Deze expressie opgegeven een datum, worden de laatste datum van de maand opgehaald.
* ``lastDayOfMonth(toDate('2009-01-12')) -> 2009-01-31``
*********************************
<code>lead</code>
==============================
<code><b>lead(<i>&lt;value&gt;</i> : any, [<i>&lt;number of rows to look after&gt;</i> : number], [<i>&lt;default value&gt;</i> : any]) => any</b></code><br/><br/>
Deze expressie wordt de waarde van de eerste parameter geëvalueerd *n* rijen na de huidige rij. De tweede parameter is het aantal rijen dat u wilt kijken ernaar uit. De standaardwaarde is 1. Als er niet zo veel rijen, wordt een waarde NULL geretourneerd als er een standaardwaarde is opgegeven.
* ``lead(amount, 2) -> 60``
* ``lead(amount, 2000, 100) -> 100``
*********************************
<code>least</code>
==============================
<code><b>least(<i>&lt;value1&gt;</i> : any, ...) => any</b></code><br/><br/>
Dit is een vergelijkingsoperator kleiner-dan-of-gelijk. Dit is hetzelfde als de **<=** operator.
* ``least(10, 30, 15, 20) -> 10``
* ``least(toDate('12/12/2010'), toDate('12/12/2011'), toDate('12/12/2000')) -> '12/12/2000'``
*********************************
<code>left</code>
==============================
<code><b>left(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></code><br/><br/>
Deze expressie een subtekenreeks beginnen bij index 1 worden uitgepakt en maakt gebruik van het aantal tekens. De **links** operator is hetzelfde als **SUBTEKENREEKS (str, 1, n)**.
* ``left('bojjus', 2) -> 'bo'``
* ``left('bojjus', 20) -> 'bojjus'``
*********************************
<code>length</code>
==============================
<code><b>length(<i>&lt;value1&gt;</i> : string) => integer</b></code><br/><br/>
Deze expressie retourneert de lengte van de tekenreeks.
* ``length('kiddo') -> 5``
*********************************
<code>lesser</code>
==============================
<code><b>lesser(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Dit is een vergelijking kleiner-dan-operator. Dit is hetzelfde als de **<** operator.
* ``lesser(12 < 24) -> true``
* ``'abcd' < 'abc' -> false``
*********************************
<code>lesserOrEqual</code>
==============================
<code><b>lesserOrEqual(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
Dit is een vergelijkingsoperator kleiner-dan-of-gelijk. Dit is hetzelfde als de **<=** operator.
* ``lesserOrEqual(12, 12) -> true``
* ``'abcd' <= 'abc' -> false``
*********************************
<code>levenshtein</code>
==============================
<code><b>levenshtein(<i>&lt;from string&gt;</i> : string, <i>&lt;to string&gt;</i> : string) => integer</b></code><br/><br/>
Deze expressie haalt de Levenshtein afstand tussen de twee tekenreeksen.
* ``levenshtein('boys', 'girls') -> 4``
*********************************
<code>like</code>
==============================
<code><b>like(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></code><br/><br/>
In deze expressie is het patroon een tekenreeks is die wordt letterlijk. 
* ``like('icecream', 'ice%') -> true``

De uitzonderingen zijn de volgende speciale tekens:  
* `_` Dit komt overeen met Eén willekeurig teken in de invoer. Het is vergelijkbaar met `.` in POSIX reguliere expressies.
* `%` Dit komt overeen met nul of meer tekens in de invoer. Dit symbool is vergelijkbaar met `.*` in POSIX reguliere expressies.
* `''` Dit is het escape-teken. Als een escape-teken vóór een speciale symbool of een ander escapeteken, het volgende teken letterlijk. U kan niet als u een andere teken.
*********************************
<code>locate</code>
==============================
<code><b>locate(<i>&lt;substring to find&gt;</i> : string, <i>&lt;string&gt;</i> : string, [<i>&lt;from index - 1-based&gt;</i> : integral]) => integer</b></code><br/><br/>
Deze expressie wordt gezocht naar de positie (gebaseerd op 1) van de subtekenreeks in een tekenreeks, beginnend bij een bepaalde positie. Als u de positie weglaat, wordt de evaluatie aan het begin van de tekenreeks begint. Als de positie wordt gevonden, wordt 0 geretourneerd. 
* ``locate('eat', 'great') -> 3``
* ``locate('o', 'microsoft', 6) -> 7``
* ``locate('bad', 'good') -> 0``
*********************************
<code>log</code>
==============================
<code><b>log(<i>&lt;value1&gt;</i> : number, [<i>&lt;value2&gt;</i> : number]) => double</b></code><br/><br/>
Deze expressie berekend logaritmische waarde. U kunt een optionele base of een cijfer Euler opgeven als deze wordt gebruikt.
* ``log(100, 10) -> 2``
*********************************
<code>log10</code>
==============================
<code><b>log10(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie wordt een base 10 voor het berekenen van de waarde van het logboek.
* ``log10(100) -> 2``
*********************************
<code>lower</code>
==============================
<code><b>lower(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Deze expressie Hiermee stelt u een tekenreeks in kleine letters.
* ``lower('GunChus') -> 'gunchus'``
*********************************
<code>lpad</code>
==============================
<code><b>lpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></code><br/><br/>
Met behulp van de opgegeven opvulling, deze expressie links-buiten bereikt de tekenreeks tot en met de tekenreeks voor een bepaalde periode. Als de tekenreeks gelijk aan of groter is dan de lengte is, heeft deze een geen bewerking (niet-op) beschouwd als.
* ``lpad('great', 10, '-') -> '-----great'``
* ``lpad('great', 4, '-') -> 'great'``
* '' lpad ('geweldige', 8, ' <>') -> ' <><great'``
*********************************
<code>ltrim</code>
==============================
<code><b>ltrim(<i>&lt;string to trim&gt;</i> : string, <i>&lt;trim characters&gt;</i> : string) => string</b></code><br/><br/>
Deze expressie links-verwijdert een reeks tekens leidt. Als een tweede parameter opgegeven is, verwijdert de expressie witruimte. Anders worden het teken dat de tweede parameter wordt verwijderd.
* ``ltrim('!--!wor!ld!', '-!') -> 'wor!ld!'``
*********************************
<code>max</code>
==============================
<code><b>max(<i>&lt;value1&gt;</i> : any) => any</b></code><br/><br/>
Deze expressie wordt de maximale waarde van een kolom.
* ``MAX(sales) -> 12312131.12``
*********************************
<code>maxIf</code>
==============================
<code><b>maxIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Op basis van een criterium, haalt deze expressie de maximumwaarde van een kolom.
* ``maxIf(region == 'West', sales) -> 99999.56``
*********************************
<code>md5</code>
==============================
<code><b>md5(<i>&lt;value1&gt;</i> : any, ...) => string</b></code><br/><br/>
Deze expressie berekent de MD5-samenvatting van een set kolommen van verschillende primitieve gegevenstypen. Deze retourneert een hexadecimale tekenreeks van 32 tekens. 
* ``md5(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 'c1527622a922c83665e49835e46350fe'``

U kunt de **md5** operator voor het berekenen van een vingerafdruk voor een rij.
*********************************
<code>mean</code>
==============================
<code><b>mean(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Deze expressie wordt het gemiddelde van waarden van een kolom. De **betekenen** operator is hetzelfde als de gemiddelde
* ``mean(sales) -> 7523420.234``
*********************************
<code>meanIf</code>
==============================
<code><b>meanIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Deze expressie op basis van een criterium, wordt het gemiddelde van waarden van een kolom opgehaald. De **meanIf** operator is hetzelfde als **avgIf**.
* ``meanIf(region == 'West', sales) -> 7523420.234``
*********************************
<code>min</code>
==============================
<code><b>min(<i>&lt;value1&gt;</i> : any) => any</b></code><br/><br/>
Deze expressie wordt de minimumwaarde van een kolom.
* ``min(sales) -> 00.01``
* ``min(orderDate) -> 12/12/2000``
*********************************
<code>minIf</code>
==============================
<code><b>minIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Op basis van een criterium, haalt deze expressie de minimumwaarde van een kolom.
* ``minIf(region == 'West', sales) -> 00.01``
*********************************
<code>minus</code>
==============================
<code><b>minus(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Deze expressie trekt getallen. Het kan bijvoorbeeld een aantal dagen van een datum aftrekken. De **min** operator is hetzelfde als de **-** operator.
* ``minus(20, 10) -> 10``
* ``20 - 10 -> 10``
* ``minus(toDate('2012-12-15'), 3) -> 2012-12-12 (date value)``
* ``toDate('2012-12-15') - 3 -> 2012-12-13 (date value)``
*********************************
<code>minute</code>
==============================
<code><b>minute(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Deze expressie wordt de minuut waarde van een tijdstempel. 
* ``minute(toTimestamp('2009-07-30T12:58:59')) -> 58``
* ``minute(toTimestamp('2009-07-30T12:58:59', 'PST')) -> 58``

U kunt een optionele tijdzone doorgeven in de vorm van `'GMT'`, `'PST'`, `'UTC'`, `'America/Cayman'`. De lokale tijdzone is de standaardinstelling.
*********************************
<code>mod</code>
==============================
<code><b>mod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
De **mod** operator markeert modulus twee getallen. Dit is hetzelfde als de **%** operator.
* ``mod(20, 8) -> 4``
* ``20 % 8 -> 4``
*********************************
<code>month</code>
==============================
<code><b>month(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Deze expressie wordt de maandwaarde van een datum of tijdstempel.
* ``month(toDate('2012-8-8')) -> 8``
*********************************
<code>monthsBetween</code>
==============================
<code><b>monthsBetween(<i>&lt;from date/timestamp&gt;</i> : datetime, <i>&lt;to date/timestamp&gt;</i> : datetime, [<i>&lt;time zone&gt;</i> : boolean], [<i>&lt;value4&gt;</i> : string]) => double</b></code><br/><br/>
Deze expressie Hiermee haalt u het aantal maanden tussen twee datums. 
* ``monthsBetween(toDate('1997-02-28 10:30:00'), toDate('1996-10-30')) -> 3.94959677``

U kunt een optionele tijdzone doorgeven in de vorm van `'GMT'`, `'PST'`, `'UTC'`, `'America/Cayman'`. De lokale tijdzone is de standaardinstelling.
*********************************
<code>multiply</code>
==============================
<code><b>multiply(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
Deze expressie vermenigvuldigt twee getallen. De **vermenigvuldigen** operator is hetzelfde als de * operator.
* ``multiply(20, 10) -> 200``
* ``20 * 10 -> 200``
*********************************
<code>nTile</code>
==============================
<code><b>nTile([<i>&lt;value1&gt;</i> : integer]) => integer</b></code><br/><br/>
De **nTile** functie verdeelt de rijen voor elke partitie venster in *n* buckets, die van 1 tot en met maximaal variëren *n*. Bucketwaarden verschillen van maximaal 1. Als het aantal rijen in de partitie niet gelijkmatig in het aantal buckets verdelen, wordt elke resterende waarde distributed in een bucket beginnen met de eerste bucket. 

* ``nTile() -> 1``
* ``nTile(numOfBuckets) -> 1``

De **nTile** functie is handig wanneer u nodig hebt om tertiles, kwartielen, deciles en andere algemene samenvattende statistieken te berekenen. De functie berekent twee variabelen die tijdens de initialisatie. Een extra rij wordt toegevoegd aan een reguliere bucket. Beide variabelen zijn gebaseerd op de grootte van de huidige partitie. 

Tijdens de berekening van de functie houdt van het nummer van de huidige rij, het nummer van de huidige bucket en het nummer van de rij die de bucket (bucketThreshold) verandert. Wanneer het rijnummer de bucket-drempel bereikt, wordt de waarde van de bucket verhoogd met één. De drempelwaarde wordt aangepast door de Bucketgrootte, plus één extra als de huidige bucket wordt aangevuld.
*********************************
<code>negate</code>
==============================
<code><b>negate(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Deze expressie genegeerd een getal. Wordt deze positieve getallen naar negatieve getallen, en vice versa.
* ``negate(13) -> -13``
*********************************
<code>nextSequence</code>
==============================
<code><b>nextSequence() => long</b></code><br/><br/>
Deze expressie retourneert de volgende unieke reeks. Het aantal is opeenvolgende alleen binnen een partitie. Het wordt voorafgegaan door `partitionId`.
* ``nextSequence() -> 12313112``
*********************************
<code>normalize</code>
==============================
<code><b>normalize(<i>&lt;String to normalize&gt;</i> : string) => string</b></code><br/><br/>
De tekenreekswaarde naar een afzonderlijke accenten zijn niet toegestaan unicode-tekens normaliseren * ``normalize('boys') -> 'boys'``
*********************************
<code>not</code>
==============================
<code><b>not(<i>&lt;value1&gt;</i> : boolean) => boolean</b></code><br/><br/>
De **niet** operator is een logische onderhandeling-operator.
* ``not(true) -> false``
* ``not(premium)``
*********************************
<code>notEquals</code>
==============================
<code><b>notEquals(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => boolean</b></code><br/><br/>
De **notEquals** operator is een niet-is gelijk aan vergelijkingsoperator. Dit is hetzelfde als de **! =** operator.
* ``12!=24 -> true``
* ``'abc'!='abc' -> false``
*********************************
<code>null</code>
==============================
<code><b>null() => null</b></code><br/><br/>
Deze expressie retourneert een NULL-waarde. 
* ``custId = NULL (for derived field)``
* ``custId == NULL -> NULL``
* ``'nothing' + NULL -> NULL``
* ``10 * NULL -> NULL'``
* ``NULL == '' -> NULL'``

Gebruik de functie **syntax(null())** als de naam van een kolom *null*. Elke bewerking die gebruikmaakt van de NULL-operator, leidt NULL.
*********************************
<code>or</code>
==============================
<code><b>or(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
De **of** operator is een logische **of** operator. Dit is hetzelfde als de **||** operator.
* ``or(true, false) -> true``
* ``true || false -> true``
*********************************
<code>pMod</code>
==============================
<code><b>pMod(<i>&lt;value1&gt;</i> : any, <i>&lt;value2&gt;</i> : any) => any</b></code><br/><br/>
De **pMod** operator markeert de positieve absolute waarde van een combinatie van cijfers.
* ``pmod(-20, 8) -> 4``
*********************************
<code>power</code>
==============================
<code><b>power(<i>&lt;value1&gt;</i> : number, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie wordt een getal tot de macht van een andere gegeven.
* ``power(10, 2) -> 100``
*********************************
<code>rank</code>
==============================
<code><b>rank(<i>&lt;value1&gt;</i> : any, ...) => integer</b></code><br/><br/>
Deze expressie berekent de positie van een waarde in een groep met waarden. Het resultaat is een plus het aantal rijen is voorgaande of gelijk zijn aan de huidige rij in de volgorde van de partitie. De waarden worden gegenereerd, hiaten in de reeks. 
* ``rank(salesQtr, salesAmt) -> 1``

De **positie** operator werkt, zelfs wanneer gegevens niet worden gesorteerd. Wijzigingen in waarden zoekt.
*********************************
<code>regexExtract</code>
==============================
<code><b>regexExtract(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, [<i>&lt;match group 1-based index&gt;</i> : integral]) => string</b></code><br/><br/>
Deze expressie subtekenreeks een overeenkomende voor een bepaalde regex-patroon. De laatste parameter identificeert de match-groep. Als u de laatste parameter niet opgeeft, wordt de standaardwaarde is 1. 
* ``regexExtract('Cost is between 600 and 800 dollars', '(\\d+) and (\\d+)', 2) -> '800'``
* ``regexExtract('Cost is between 600 and 800 dollars', `(\d+) and (\d+)`, 2) -> '800'``

Gebruik `<regex>`(vorige aanhalingsteken) zodat deze overeenkomt met een tekenreeks zonder aanhalingstekens.
*********************************
<code>regexMatch</code>
==============================
<code><b>regexMatch(<i>&lt;string&gt;</i> : string, <i>&lt;regex to match&gt;</i> : string) => boolean</b></code><br/><br/>
Deze expressie wordt gecontroleerd of de tekenreeks die overeenkomt met de opgegeven regex-patroon. 
* ``regexMatch('200.50', '(\\d+).(\\d+)') -> true``
* ``regexMatch('200.50', `(\d+).(\d+)`) -> true``

Gebruik `<regex>`(vorige aanhalingsteken) zodat deze overeenkomt met een tekenreeks zonder aanhalingstekens.
*********************************
<code>regexReplace</code>
==============================
<code><b>regexReplace(<i>&lt;string&gt;</i> : string, <i>&lt;regex to find&gt;</i> : string, <i>&lt;substring to replace&gt;</i> : string) => string</b></code><br/><br/>
Deze expressie vervangt alle instanties van een regex-patroon door een andere subtekenreeks in de opgegeven tekenreeks.
* ``regexReplace('100 and 200', '(\\d+)', 'bojjus') -> 'bojjus and bojjus'``
* ``regexReplace('100 and 200', `(\d+)`, 'gunchus') -> 'gunchus and gunchus'``

Gebruik `<regex>`(vorige aanhalingsteken) zodat deze overeenkomt met een tekenreeks zonder aanhalingstekens.
*********************************
<code>regexSplit</code>
==============================
<code><b>regexSplit(<i>&lt;string to split&gt;</i> : string, <i>&lt;regex expression&gt;</i> : string) => array</b></code><br/><br/>
Deze expressie wordt een tekenreeks op basis van een op basis van de reguliere expressie scheidingsteken gesplitst. Er wordt een matrix met tekenreeksen.
* ``regexSplit('oneAtwoBthreeC', '[CAB]') -> ['one', 'two', 'three']``
* ``regexSplit('oneAtwoBthreeC', '[CAB]')[1] -> 'one'``
* ``regexSplit('oneAtwoBthreeC', '[CAB]')[0] -> NULL``
* ``regexSplit('oneAtwoBthreeC', '[CAB]')[20] -> NULL``
*********************************
<code>replace</code>
==============================
<code><b>replace(<i>&lt;string&gt;</i> : string, <i>&lt;substring to find&gt;</i> : string, <i>&lt;substring to replace&gt;</i> : string) => string</b></code><br/><br/>
Deze expressie worden alle instanties van een subtekenreeks vervangen door een andere subtekenreeks in de opgegeven tekenreeks.
* ``replace('doggie dog', 'dog', 'cat') -> 'catgie cat'``
* ``replace('doggie dog', 'dog', '') -> 'gie'``
*********************************
<code>reverse</code>
==============================
<code><b>reverse(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Deze expressie wordt een tekenreeks is omgekeerd.
* ``reverse('gunchus') -> 'suhcnug'``
*********************************
<code>right</code>
==============================
<code><b>right(<i>&lt;string to subset&gt;</i> : string, <i>&lt;number of characters&gt;</i> : integral) => string</b></code><br/><br/>
Deze expressie subtekenreeks een met het aantal tekens vanaf de rechterkant. 
* ``right('bojjus', 2) -> 'us'``
* ``right('bojjus', 20) -> 'bojjus'``

De **rechts** functie is hetzelfde als de SUBTEKENREEKS (str LENGTH(str) - n, n).
*********************************
<code>rlike</code>
==============================
<code><b>rlike(<i>&lt;string&gt;</i> : string, <i>&lt;pattern match&gt;</i> : string) => boolean</b></code><br/><br/>
Deze expressie wordt gecontroleerd of de tekenreeks die overeenkomt met de opgegeven regex-patroon.
* ``rlike('200.50', '(\d+).(\d+)') -> true``
*********************************
<code>round</code>
==============================
<code><b>round(<i>&lt;number&gt;</i> : number, [<i>&lt;scale to round&gt;</i> : number], [<i>&lt;rounding option&gt;</i> : integral]) => double</b></code><br/><br/>
Deze expressie opgegeven een optionele schaal en een optionele afronding modus, rondt een getal af. Als u de schaal weglaat, wordt de standaardwaarde is 0. Als u de modus weglaat, wordt de standaardwaarde ROUND_HALF_UP(5) is. 

* ``round(100.123) -> 100.0``
* ``round(2.5, 0) -> 3.0``
* ``round(5.3999999999999995, 2, 7) -> 5.40``

Dit zijn de waarden voor de afronding van:
* 1 - ROUND_UP
* 2 - ROUND_DOWN
* 3 - ROUND_CEILING
* 4 - ROUND_FLOOR
* 5 - ROUND_HALF_UP
* 6 - ROUND_HALF_DOWN
* 7 - ROUND_HALF_EVEN
* 8 - ROUND_UNNECESSARY

*********************************
<code>rowNumber</code>
==============================
<code><b>rowNumber() => integer</b></code><br/><br/>
Deze expressie wordt een opeenvolgende rijen nummeren voor rijen in een venster te beginnen met 1 toegewezen.
* ``rowNumber() -> 1``
*********************************
<code>rpad</code>
==============================
<code><b>rpad(<i>&lt;string to pad&gt;</i> : string, <i>&lt;final padded length&gt;</i> : integral, <i>&lt;padding&gt;</i> : string) => string</b></code><br/><br/>
Deze expressie rechts-buiten de tekenreeks met de opgegeven opvulling tot en met de tekenreeks bereikt een bepaalde periode. Als de tekenreeks gelijk aan of groter is dan de lengte is, heeft deze een geen bewerking (niet-op) beschouwd als.
* ``rpad('great', 10, '-') -> 'great-----'``
* ``rpad('great', 4, '-') -> 'great'``
* ``rpad('great', 8, '<>') -> 'great<><'``
*********************************
<code>rtrim</code>rtrim</code>
==============================
<code><b>rtrim(<i>&lt;string to trim&gt;</i> : string, <i>&lt;trim characters&gt;</i> : string) => string</b></code><br/><br/>
Deze expressie rechts-verwijdert een reeks tekens leidt. Als u de tweede parameter niet opgeeft, wordt in de expressie witruimte verwijdert. Anders worden er willekeurig teken dat de tweede parameter wordt verwijderd.
* ``rtrim('!--!wor!ld!', '-!') -> '!--!wor!ld'``
*********************************
<code>second</code>
==============================
<code><b>second(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => integer</b></code><br/><br/>
Deze expressie wordt de tweede waarde van een datum. 
* ``second(toTimestamp('2009-07-30T12:58:59')) -> 59``

U kunt een optionele tijdzone doorgeven in de vorm van `'GMT'`, `'PST'`, `'UTC'`, `'America/Cayman'`. De lokale tijdzone is de standaardinstelling.
*********************************
<code>sha1</code>
==============================
<code><b>sha1(<i>&lt;value1&gt;</i> : any, ...) => string</b></code><br/><br/>
Deze expressie berekent de SHA-1-samenvatting van een set kolommen van verschillende primitieve gegevenstypen. Het resultaat een hexadecimale tekenreeks 40 tekens. 
* ``sha1(5, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> '63849fd2abb65fbc626c60b1f827bd05573f0cea'``

U kunt `sha1` voor het berekenen van een vingerafdruk voor een rij.
*********************************
<code>sha2</code>
==============================
<code><b>sha2(<i>&lt;value1&gt;</i> : integer, <i>&lt;value2&gt;</i> : any, ...) => string</b></code><br/><br/>
Deze expressie berekent de SHA-2-samenvatting van een set kolommen van verschillende primitieve gegevenstypen, krijgt een bitlengte waarvan de waarden alleen 0(256) mogen 224, 256, 384, 512. 
* ``sha2(256, 'gunchus', 8.2, 'bojjus', true, toDate('2010-4-4')) -> 'd3b2bff62c3a00e9370b1ac85e428e661a7df73959fa1a96ae136599e9ee20fd'``

U kunt `sha2` voor het berekenen van een vingerafdruk voor een rij.
*********************************
<code>sin</code>
==============================
<code><b>sin(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie berekent een sinuswaarde.
* ``sin(2) -> 0.90929742682``
*********************************
<code>sinh</code>
==============================
<code><b>sinh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie berekend de waarde van een hyperbolische sinus.
* ``sinh(0) -> 0.0``
*********************************
<code>skewness</code>
==============================
<code><b>skewness(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie haalt de asymmetrie van een kolom.
* ``skewness(sales) -> 122.12``
*********************************
<code>skewnessIf</code>
==============================
<code><b>skewnessIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criterium, haalt deze expressie de asymmetrie van een kolom.
* ``skewnessIf(region == 'West', sales) -> 122.12``
*********************************
<code>slice</code>
==============================
<code><b>slice(<i>&lt;array to slice&gt;</i> : array, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of items&gt;</i> : integral]) => array</b></code><br/><br/>
Deze expressie retourneert een subset van een matrix vanaf een positie. De positie is gebaseerd op 1. Als u de lengte weglaat, wordt standaard het einde van de tekenreeks.
* ``slice([10, 20, 30, 40], 1, 2) -> [10, 20]``
* ``slice([10, 20, 30, 40], 2) -> [20, 30, 40]``
* ``slice([10, 20, 30, 40], 2)[1] -> 20``
* ``slice([10, 20, 30, 40], 2)[0] -> NULL``
* ``slice([10, 20, 30, 40], 2)[20] -> NULL``
* ``slice([10, 20, 30, 40], 8) -> []``
*********************************
<code>soundex</code>
==============================
<code><b>soundex(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Deze expressie haalt de code geluid voor de tekenreeks.
* ``soundex('genius') -> 'G520'``
*********************************
<code>split</code>
==============================
<code><b>split(<i>&lt;string to split&gt;</i> : string, <i>&lt;split characters&gt;</i> : string) => array</b></code><br/><br/>
Deze expressie wordt een tekenreeks op basis van een scheidingsteken gesplitst. Er wordt een matrix met tekenreeksen.
* ``split('100,200,300', ',') -> ['100', '200', '300']``
* ``split('100,200,300', '|') -> ['100,200,300']``
* ``split('100, 200, 300', ', ') -> ['100', '200', '300']``
* ``split('100, 200, 300', ', ')[1] -> '100'``
* ``split('100, 200, 300', ', ')[0] -> NULL``
* ``split('100, 200, 300', ', ')[20] -> NULL``
* ``split('100200300', ',') -> ['100200300']``
*********************************
<code>sqrt</code>
==============================
<code><b>sqrt(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie berekent de vierkantswortel van een getal.
* ``sqrt(9) -> 3``
*********************************
<code>startsWith</code>
==============================
<code><b>startsWith(<i>&lt;string&gt;</i> : string, <i>&lt;substring to check&gt;</i> : string) => boolean</b></code><br/><br/>
Deze expressie wordt gecontroleerd of de tekenreeks met de opgegeven tekenreeks begint.
* ``startsWith('great', 'gr') -> true``
*********************************
<code>stddev</code>
==============================
<code><b>stddev(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie de standaardafwijking van een kolom opgehaald.
* ``stdDev(sales) -> 122.12``
*********************************
<code>stddevIf</code>
==============================
<code><b>stddevIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criterium, haalt deze expressie de standaardafwijking van een kolom.
* ``stddevIf(region == 'West', sales) -> 122.12``
*********************************
<code>stddevPopulation</code>
==============================
<code><b>stddevPopulation(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie de standaardafwijking van een kolom opgehaald.
* ``stddevPopulation(sales) -> 122.12``
*********************************
<code>stddevPopulationIf</code>
==============================
<code><b>stddevPopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criterium, haalt deze expressie de standaardafwijking van een kolom.
* ``stddevPopulationIf(region == 'West', sales) -> 122.12``
*********************************
<code>stddevSample</code>
==============================
<code><b>stddevSample(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie de standaardafwijking van voorbeeld van een kolom opgehaald.
* ``stddevSample(sales) -> 122.12``
*********************************
<code>stddevSampleIf</code>
==============================
<code><b>stddevSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criterium, haalt deze expressie de standaardafwijking van voorbeeld van een kolom.
* ``stddevSampleIf(region == 'West', sales) -> 122.12``
*********************************
<code>subDays</code>
==============================
<code><b>subDays(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;days to subtract&gt;</i> : integral) => datetime</b></code><br/><br/>
Deze expressie trekt maanden vanaf een datum. 
* ``subDays(toDate('2016-08-08'), 1) -> 2016-08-09``
De **subDays** operator is hetzelfde als de **-** operator voor de datum.
*********************************
<code>subMonths</code>
==============================
<code><b>subMonths(<i>&lt;date/timestamp&gt;</i> : datetime, <i>&lt;months to subtract&gt;</i> : integral) => datetime</b></code><br/><br/>
Deze expressie trekt maanden vanaf de datum- of timestamp.
* ``subMonths(toDate('2016-09-30'), 1) -> 2016-08-31``
*********************************
<code>substring</code>
==============================
<code><b>substring(<i>&lt;string to subset&gt;</i> : string, <i>&lt;from 1-based index&gt;</i> : integral, [<i>&lt;number of characters&gt;</i> : integral]) => string</b></code><br/><br/>
Deze expressie subtekenreeks een van een bepaalde lengte van een positie. De positie is gebaseerd op 1. Als u de lengte weglaat, wordt standaard het einde van de tekenreeks.
* ``substring('Cat in the hat', 5, 2) -> 'in'``
* ``substring('Cat in the hat', 5, 100) -> 'in the hat'``
* ``substring('Cat in the hat', 5) -> 'in the hat'``
* ``substring('Cat in the hat', 100, 100) -> ''``
*********************************
<code>sum</code>
==============================
<code><b>sum(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Deze expressie wordt de totale som van een numerieke kolom opgehaald.
* ``sum(col) -> value``
*********************************
<code>sumDistinct</code>
==============================
<code><b>sumDistinct(<i>&lt;value1&gt;</i> : number) => number</b></code><br/><br/>
Deze expressie wordt de totale som van de afzonderlijke waarden van een numerieke kolom opgehaald.
* ``sumDistinct(col) -> value``
*********************************
<code>sumDistinctIf</code>
==============================
<code><b>sumDistinctIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Deze expressie op basis van een criterium, wordt de totale som van een numerieke kolom opgehaald. U kunt de voorwaarde baseren op een andere kolom.
* ``sumDistinctIf(state == 'CA' && commission < 10000, sales) -> value``
* ``sumDistinctIf(true, sales) -> SUM(sales)``
*********************************
<code>sumIf</code>
==============================
<code><b>sumIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => number</b></code><br/><br/>
Deze expressie op basis van criteria, wordt de totale som van een numerieke kolom opgehaald. U kunt de voorwaarde baseren op een andere kolom.
* ``sumIf(state == 'CA' && commission < 10000, sales) -> value``
* ``sumIf(true, sales) -> SUM(sales)``
*********************************
<code>tan</code>
==============================
<code><b>tan(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie berekent een tangenswaarde.
* ``tan(0) -> 0.0``
*********************************
<code>tanh</code>
==============================
<code><b>tanh(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie berekend een hyperbolische tangens waarde.
* ``tanh(0) -> 0.0``
*********************************
<code>toBoolean</code>
==============================
<code><b>toBoolean(<i>&lt;value1&gt;</i> : string) => boolean</b></code><br/><br/>
Deze expressie zet een waarde van `('t', 'true', 'y', 'yes', '1')` op ' True '. Wordt omgezet in `('f', 'false', 'n', 'no', '0')` op onwaar. Voor een andere waarde wordt resultaat het NULL.
* ``toBoolean('true') -> true``
* ``toBoolean('n') -> false``
* ``toBoolean('truthy') -> NULL``
*********************************
<code>toDate</code>
==============================
<code><b>toDate(<i>&lt;string&gt;</i> : any, [<i>&lt;date format&gt;</i> : string]) => date</b></code><br/><br/>
Deze expressie zet opgegeven een optionele datum opmaken, een tekenreeks naar een datum. Raadpleeg Java SimpleDateFormat voor alle mogelijke datumnotaties. 
* ``toDate('2012-8-8') -> 2012-8-8``
* ``toDate('12/12/2012', 'MM/dd/yyyy') -> 2012-12-12``

Als u de datumnotatie weglaat, combinaties van de volgende worden geaccepteerd: [dd, jjjj-[M] M-jjjj - M [M]-[d] d, d jjjj-[M] M-[d], d jjjj-[M] M-[d], jjjj-[M] M-[d] dT *].
*********************************
<code>toDecimal</code>
==============================
<code><b>toDecimal(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : integral], [<i>&lt;value3&gt;</i> : integral], [<i>&lt;value4&gt;</i> : string]) => decimal(10,0)</b></code><br/><br/>
Deze expressie converteert een numerieke waarde of een tekenreeks naar een decimale waarde. 
* ``toDecimal(123.45) -> 123.45``
* ``toDecimal('123.45', 8, 4) -> 123.4500``
* ``toDecimal('$123.45', 8, 4,'$###.00') -> 123.4500``

Als u geen precision en scale opgeeft, wordt standaard (10,2). U kunt een optionele Java decimale notatie gebruiken voor de conversie.

*********************************
<code>toDouble</code>
==============================
<code><b>toDouble(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string]) => double</b></code><br/><br/>
Deze expressie converteert een numerieke waarde of een tekenreeks naar een double-waarde. U kunt een optionele Java decimale notatie gebruiken voor de conversie.
* ``toDouble(123.45) -> 123.45``
* ``toDouble('123.45') -> 123.45``
* ``toDouble('$123.45', '$###.00') -> 123.45``
*********************************
<code>toFloat</code>
==============================
<code><b>toFloat(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string]) => float</b></code><br/><br/>
Deze expressie zet een numerieke waarde of een tekenreeks om in een float-waarde. U kunt een optionele Java decimale notatie gebruiken voor de conversie. 
* ``toFloat(123.45) -> 123.45``
* ``toFloat('123.45') -> 123.45``
* ``toFloat('$123.45', '$###.00') -> 123.45``

De **toFloat** functie kapt een getal met dubbele precisie.
*********************************
<code>toInteger</code>
==============================
<code><b>toInteger(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string]) => integer</b></code><br/><br/>
Deze expressie zet een numerieke waarde of een tekenreeks om een geheel getal. U kunt een optionele Java decimale notatie gebruiken voor de conversie. 
* ``toInteger(123) -> 123``
* ``toInteger('123') -> 123``
* ``toInteger('$123', '$###') -> 123``

De **toInteger** functie kapt een lange, drijvende komma of dubbele.
*********************************
<code>toLong</code>
==============================
<code><b>toLong(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string]) => long</b></code><br/><br/>
Deze expressie zet een numerieke waarde of een tekenreeks om een long-waarde. U kunt een optionele Java decimale notatie gebruiken voor de conversie. 
* ``toLong(123) -> 123``
* ``toLong('123') -> 123``
* ``toLong('$123', '$###') -> 123``

De **toLong** functie kapt een gegevenstype met drijvende komma of verdubbeld.
*********************************
<code>toShort</code>
==============================
<code><b>toShort(<i>&lt;value&gt;</i> : any, [<i>&lt;format&gt;</i> : string]) => short</b></code><br/><br/>
Deze expressie converteert een numerieke waarde of een tekenreeks naar een korte waarde. U kunt een optionele Java decimale notatie gebruiken voor de conversie. 
* ``toShort(123) -> 123``
* ``toShort('123') -> 123``
* ``toShort('$123', '$###') -> 123``

De **toShort** functie kapt een geheel getal, lang, drijvende komma of dubbele.
*********************************
<code>toString</code>
==============================
<code><b>toString(<i>&lt;value&gt;</i> : any, [<i>&lt;number format/date format&gt;</i> : string]) => string</b></code><br/><br/>
Deze expressie wordt een primitief type geconverteerd naar een tekenreeks. 
* ``toString(10) -> '10'``
* ``toString('engineer') -> 'engineer'``
* ``toString(123456.789, '##,###.##') -> '123,456.79'``
* ``toString(123.78, '000000.000') -> '000123.780'``
* ``toString(12345, '##0.#####E0') -> '12.345E3'``
* ``toString(toDate('2018-12-31')) -> '2018-12-31'``
* ``toString(toDate('2018-12-31'), 'MM/dd/yy') -> '12/31/18'``
* ``toString(4 == 20) -> 'false'``

U kunt een indeling voor getallen en datums opgeven. Als u niet een indeling opgeeft, wordt de standaardwaarde wordt gebruikt. De standaardwaarde is `yyyy-MM-dd`. Getallen volgt u de notatie decimaal van Java. Raadpleeg voor alle mogelijke datumnotaties Java SimpleDateFormat. 
*********************************
<code>toTimestamp</code>
==============================
<code><b>toTimestamp(<i>&lt;string&gt;</i> : any, [<i>&lt;timestamp format&gt;</i> : string], [<i>&lt;time zone&gt;</i> : string]) => timestamp</b></code><br/><br/>
Deze expressie converteert opgegeven een optionele tijdstempelnotatie, een tekenreeks naar een datum. 
* ``toTimestamp('2016-12-31 00:12:00') -> 2012-8-8T00:12:00``
* ``toTimestamp('2016/12/31T00:12:00', 'MM/dd/yyyyThh:mm:ss') -> 2012-12-12T00:12:00``

Raadpleeg Java SimpleDateFormat voor alle mogelijke notaties. Als u de tijdstempel, standaardpatroon weglaat `yyyy-[M]M-[d]d hh:mm:ss[.f...]` wordt gebruikt.
*********************************
<code>toUTC</code>
==============================
<code><b>toUTC(<i>&lt;value1&gt;</i> : timestamp, [<i>&lt;value2&gt;</i> : string]) => timestamp</b></code><br/><br/>
Deze expressie worden de tijdstempel geconverteerd naar UTC. 
* ``toUTC(currentTimeStamp()) -> 12-12-2030T19:18:12``
* ``toUTC(currentTimeStamp(), 'Asia/Seoul') -> 12-13-2030T11:18:12``

U kunt een optionele tijdzone doorgeven in de vorm van `'GMT'`, `'PST'`, `'UTC'`, `'America/Cayman'`. De standaardwaarde is de huidige tijdzone.
*********************************
<code>translate</code>
==============================
<code><b>translate(<i>&lt;string to translate&gt;</i> : string, <i>&lt;lookup characters&gt;</i> : string, <i>&lt;replace characters&gt;</i> : string) => string</b></code><br/><br/>
Deze expressie wordt vervangen door een reeks tekens met een andere reeks tekens in de tekenreeks. Tekens hebben een-op-een vervanging.
* ``translate('(Hello)', '()', '[]') -> '[Hello]'``
* ``translate('(Hello)', '()', '[') -> '[Hello'``
*********************************
<code>trim</code>
==============================
<code><b>trim(<i>&lt;string to trim&gt;</i> : string, [<i>&lt;trim characters&gt;</i> : string]) => string</b></code><br/><br/>
Deze expressie verwijdert een tekenreeks voorloop- en volgspaties. 
* ``trim('!--!wor!ld!', '-!') -> 'wor!ld'``

Als u de tweede parameter niet opgeeft, verwijdert de functie witruimte. Anders worden er willekeurig teken dat de tweede parameter wordt verwijderd.

*********************************
<code>true</code>
==============================
<code><b>true() => boolean</b></code><br/><br/>
Altijd deze expressie retourneert een `true` waarde.  
* ``isDiscounted == true()``
* ``isDiscounted() == true``

Als de naam van kolom *waar*, gebruikt u de functie **syntax(true())**.
*********************************
<code>typeMatch</code>
==============================
<code><b>typeMatch(<i>&lt;type&gt;</i> : string, <i>&lt;base type&gt;</i> : string) => boolean</b></code><br/><br/>
Deze expressie komt overeen met het type van de kolom. 
* ``typeMatch(type, 'number') -> true``
* ``typeMatch('date', 'number') -> false``

Gebruik deze functie alleen in expressies voor patroon: nummer overeenkomt met korte, geheel getal, lang, double, drijvende komma of een decimaal, integraal komt overeen met kort gezegd, geheel getal, lange, decimale komt overeen met dubbele, float, decimaal getal en datum/tijd komt overeen met datum- of timestamp-type.
*********************************
<code>upper</code>
==============================
<code><b>upper(<i>&lt;value1&gt;</i> : string) => string</b></code><br/><br/>
Deze expressie wordt een tekenreeks in hoofdletters.
* ``upper('bojjus') -> 'BOJJUS'``
*********************************
<code>variance</code>
==============================
<code><b>variance(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie worden opgehaald van de variantie van een kolom.
* ``variance(sales) -> 122.12``
*********************************
<code>varianceIf</code>
==============================
<code><b>varianceIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criterium, haalt deze expressie de variantie van een kolom.
* ``varianceIf(region == 'West', sales) -> 122.12``
*********************************
<code>variancePopulation</code>
==============================
<code><b>variancePopulation(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie de populatieverschillen van een kolom opgehaald.
* ``variancePopulation(sales) -> 122.12``
*********************************
<code>variancePopulationIf</code>
==============================
<code><b>variancePopulationIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criterium, haalt deze expressie de populatieverschillen van een kolom.
* ``variancePopulationIf(region == 'West', sales) -> 122.12``
*********************************
<code>varianceSample</code>
==============================
<code><b>varianceSample(<i>&lt;value1&gt;</i> : number) => double</b></code><br/><br/>
Deze expressie haalt de zuivere variantie van een kolom.
* ``varianceSample(sales) -> 122.12``
*********************************
<code>varianceSampleIf</code>
==============================
<code><b>varianceSampleIf(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : number) => double</b></code><br/><br/>
Op basis van een criterium, haalt deze expressie de zuivere variantie van een kolom.
* ``varianceSampleIf(region == 'West', sales) -> 122.12``
*********************************
<code>weekOfYear</code>
==============================
<code><b>weekOfYear(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Deze expressie opgegeven een datum, worden de week van het jaar opgehaald.
* ``weekOfYear(toDate('2008-02-20')) -> 8``
*********************************
<code>xor</code>
==============================
<code><b>xor(<i>&lt;value1&gt;</i> : boolean, <i>&lt;value2&gt;</i> : boolean) => boolean</b></code><br/><br/>
Deze expressie maakt gebruik van de logische **xor** operator. Deze operator is hetzelfde als de **^** operator.
* ``xor(true, false) -> true``
* ``xor(true, true) -> false``
* ``true ^ false -> true``
*********************************
<code>year</code>
==============================
<code><b>year(<i>&lt;value1&gt;</i> : datetime) => integer</b></code><br/><br/>
Deze expressie opgegeven een datum, wordt de jaarwaarde opgehaald.
* ``year(toDate('2012-8-8')) -> 2012``

## <a name="next-steps"></a>Volgende stappen

[Informatie over het gebruik van de opbouwfunctie voor expressies](concepts-data-flow-expression-builder.md).
