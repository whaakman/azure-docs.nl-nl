---
title: Azure Log Analytics zoeken verwijzing | Microsoft Docs
description: De logboekanalyse zoeken verwijzing beschrijving van de taal van de zoekopdracht en syntaxis van de algemene query-opties kunt gebruiken wanneer gegevens zoeken en filteren van expressies om te helpen uw zoekopdracht te verfijnen.
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: 402615a2-bed0-4831-ba69-53be49059718
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fc9c9b0a6292dab256997a86a6db16367fc48cd3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="log-analytics-search-reference"></a>Log Analytics verwijzing zoeken

>[!NOTE]
> Dit artikel wordt beschreven logboek zoekopdrachten met behulp van de huidige querytaal in logboekanalyse.  Als uw werkruimte is bijgewerkt naar de [querytaal van nieuwe logboekanalyse](log-analytics-log-search-upgrade.md), en u naar verwijzen moet [de Naslaggids voor de nieuwe taal](https://go.microsoft.com/fwlink/?linkid=856079).

Het volgende gedeelte van de naslaginformatie over zoekopdracht taal bevat de algemene syntaxis queryopties kunt u wanneer gegevens zoeken en filteren van expressies om te helpen uw zoekopdracht te verfijnen. Hierin worden ook opdrachten die u gebruiken kunt om actie te ondernemen op de gegevens zijn opgehaald.

U kunt lezen over de velden die worden geretourneerd in zoekopdrachten en de facetten die u helpen bij lees meer over gelijksoortige gegevenscategorieën de [veld zoeken en facet verwijzen naar de sectie](#search-field-and-facet-reference).

## <a name="general-query-syntax"></a>Algemene querysyntaxis
De syntaxis van de algemene query's is als volgt:

```
filterExpression | command1 | command2 …
```

De filterexpressie (`filterExpression`) definieert de 'where' voorwaarde voor de query. De opdrachten van toepassing op de resultaten die door de query zijn geretourneerd. Meerdere opdrachten moeten worden gescheiden door het sluisteken (|).

### <a name="general-syntax-examples"></a>Voorbeelden van de algemene syntaxis
Voorbeelden:

```
system
```

Deze query retourneert resultaten met het woord *system* in elk veld dat is geïndexeerd voor volledige tekst of voorwaarden zoeken.

> [!NOTE]
> Niet alle velden op deze manier worden geïndexeerd, maar de meest voorkomende tekstuele velden meestal (zoals beschrijvingen en namen) zijn.
>
>

```
system error
```

Deze query retourneert resultaten die de woorden bevatten *system* en *fout*.

```
system error | sort ManagementGroupName, TimeGenerated desc | top 10
```

Deze query retourneert resultaten die de woorden bevatten *system* en *fout*. Sorteert vervolgens de resultaten worden op de *ManagementGroupName* veld (in oplopende volgorde), en vervolgens op de *TimeGenerated* veld (in aflopende volgorde). Het duurt alleen de eerste 10 resultaten.

> [!IMPORTANT]
> De veldnamen en de waarden voor de tekenreeks en tekst velden zijn hoofdlettergevoelig.
>
>

## <a name="filter-expressions"></a>Filterexpressies
De volgende subsecties worden de filterexpressies.

### <a name="string-literals"></a>Letterlijke tekenreeks
Een letterlijke tekenreeks is een tekenreeks die niet wordt herkend door de parser als een sleutelwoord of een vooraf gedefinieerde gegevenstype (bijvoorbeeld een getal of een datum).

Voorbeelden:

```
These all are string literals
```

Deze query zoekt naar resultaten die exemplaren van alle vijf woorden bevatten. Om complexe tekenreeks te doorzoeken, moet u de tekenreeks tussen aanhalingstekens. Bijvoorbeeld:

```
"Windows Server"
```

Dit retourneert alleen resultaten met de exacte overeenkomsten voor *Windows Server*.

### <a name="numbers"></a>Cijfers
De parser ondersteunt het geheel getal en de syntaxis van de drijvende-nummer voor numerieke velden.

Voorbeelden:

```
Type:Perf 0.5
```

```
HTTP 500
```

### <a name="dates-and-times"></a>Datums en tijden
Elk onderdeel van de gegevens in het systeem heeft een *TimeGenerated* eigenschap aanduiden, waarbij de oorspronkelijke datum en tijd van de record vertegenwoordigt. Bepaalde typen gegevens kunnen aanvullende datum- en tijdvelden hebben (bijvoorbeeld *LastModified*).

De tijdlijn **grafiek/tijd** selector in Azure-logboekanalyse toont een distributie van resultaten gedurende een bepaalde periode (volgens de huidige query wordt uitgevoerd). Dit is gebaseerd op de *TimeGenerated* veld. Datum en tijd velden hebben een specifieke tekenreeks-indeling die kan worden gebruikt in query's voor de query beperken tot een bepaalde periode. U kunt ook syntaxis gebruiken om te verwijzen naar relatieve tijdsintervallen (bijvoorbeeld ' tussen drie dagen geleden en 2 uur geleden').

Hier volgen geldig vormen van de syntaxis voor datums en tijden:

```
yyyy-mm-ddThh:mm:ss.dddZ
```

```
yyyy-mm-ddThh:mm:ss.ddd
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm
```

```
yyyy-mm-dd
```


Bijvoorbeeld:

```
TimeGenerated:2013-10-01T12:20
```

De vorige opdracht retourneert alleen de records met een *TimeGenerated* waarde exact 12:20 op 1 oktober 2013.

De parser ondersteunt ook het symbool datum/tijd-waarde, nu. (Het lijkt onwaarschijnlijk dat dit geen resultaten leidt tot omdat gegevens niet worden aanbrengen via het systeem dat snelle.)

Deze voorbeelden zijn de bouwstenen voor relatieve en absolute datums. In de volgende drie subsecties ziet u hoe u ze gebruikt in meer geavanceerde filters, met voorbeelden die de relatieve datumbereiken gebruiken.

### <a name="datetime-math"></a>Math datum/tijd
Gebruik de datum/tijd rekenkundige operatoren offset of de datum/tijd-waarde met behulp van eenvoudige datum/tijd-berekeningen afronden.

Syntaxis:

```
datetime/unit
```

```
datetime[+|-]count unit
```

| Operator | Beschrijving |
| --- | --- |
| / |Datum/tijd afgerond op de opgegeven eenheid. Bijvoorbeeld, nu / dag rondt af van de huidige datum en tijd op middernacht van de huidige dag. |
| + of - |Datum/tijd op het opgegeven aantal eenheden, worden verschoven. Bijvoorbeeld, nu + 1 uur wordt de huidige datum en tijd van een uur vooruit. 2013-10-01T12:00-10 dagen wordt de datumwaarde terug van tien dagen. |

U kunt de datum/tijd rekenkundige operatoren elkaar koppelen. Bijvoorbeeld:

```
NOW+1HOUR-10MONTHS/MINUTE
```

De volgende tabel bevat de ondersteunde datum/tijd-eenheden.

| Datum/tijd-eenheid | Beschrijving |
| --- | --- |
| JAAR, JAAR |Wordt afgerond op het huidige jaar, of het opgegeven aantal jaren, worden verschoven. |
| MAAND MAANDEN |Wordt afgerond op de huidige maand of het opgegeven aantal maanden worden verschoven. |
| DAG, DAGEN, DATUM |Wordt afgerond op de huidige dag van de maand of het opgegeven aantal dagen, worden verschoven. |
| UUR, UREN |Wordt afgerond op het huidige uur of het opgegeven aantal uren worden verschoven. |
| MINUUT, MINUTEN |Wordt afgerond op de huidige minuut of het opgegeven aantal minuten worden verschoven. |
| TEN TWEEDE SECONDEN |Rondt af naar huidige tweede of het opgegeven aantal seconden worden verschoven. |
| MILLISECONDE MILLISECONDEN MILLI, MILLIS |Wordt afgerond op de huidige milliseconde of het opgegeven aantal milliseconden worden verschoven. |

### <a name="field-facets"></a>Veld facetten
U kunt het zoekcriterium voor specifieke velden en hun precieze waarden opgeven met behulp van veld facetten. Dit wijkt af van het schrijven van query's 'vrije tekst' voor verschillende voorwaarden in de index. U hebt al gezien deze techniek in verschillende voorgaande voorbeelden. Hier volgen complexere voorbeelden.

**Syntaxis**

```
field:value
```

```
field=value
```

**Beschrijving**

Het veld voor de specifieke waarde zoekt. De waarde kan een letterlijke tekenreeks, getal, of datum en tijd liggen.

Bijvoorbeeld:

```
TimeGenerated:NOW
```

```
ObjectDisplayName:"server01.contoso.com"
```

```
SampleValue:0.3
```

**Syntaxis**

*veld > waarde*

*veld < waarde*

*veld > = waarde*

*veld < = waarde*

*veld! = waarde*

**Beschrijving**

Biedt vergelijkingen.

Bijvoorbeeld:

```
TimeGenerated>NOW+2HOURS
```

**Syntaxis**

```
field:[from..to]
```

**Beschrijving**

Biedt bereik facetten.

Bijvoorbeeld:

```
TimeGenerated:[NOW..NOW+1DAY]
```

```
SampleValue:[0..2]
```

### <a name="in"></a>IN
De **IN** sleutelwoord kunt u selecteren uit een lijst met waarden. Dit kan een eenvoudige lijst met waarden die u opgeeft, of een lijst met waarden van een aggregatie zijn afhankelijk van de syntaxis die u gebruikt.

Syntaxis 1:

```
field IN {value1,value2,value3,...}
```

Deze syntaxis kunt u alle waarden in een eenvoudige lijst opgenomen.



Voorbeelden:

```
EventID IN {1201,1204,1210}
```

```
Computer IN {"srv01.contoso.com","srv02.contoso.com"}
```

Syntaxis 2:

```
(Outer query) (Field to use with inner query results) IN {Inner query | measure count() by (Field to send to outer query)} (rest  of outer query)  
```

Deze syntaxis kunt u een aggregatie van maken. U kunt vervolgens de lijst met waarden feed vanuit die verwijst naar een andere outer (primaire) zoeken die er ongeveer voor gebeurtenissen met deze waarde uitziet. U doen dit door de binnenste zoekopdracht insluitende accolades en de resultaten voeding als mogelijke waarden voor een veld in het buitenste zoeken met behulp van de operator.

Interne query-voorbeeld: *computers momenteel ontbrekende beveiligingsupdates* met de volgende globalisatie-query:

```
Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer
```    

De laatste query waarmee wordt gezocht naar *alle Windows-gebeurtenissen voor computers die momenteel ontbrekende beveiligingsupdates* lijkt op het volgende:

```
Type=Event Computer IN {Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer}
```

### <a name="contains"></a>Contains
De **bevat** sleutelwoord kunt u filters voor records met een veld dat een opgegeven tekenreeks bevat. Dit is hoofdlettergevoelig, werkt alleen met tekenreeksvelden en mag escape-tekens bevatten.

Syntaxis:

```
field:contains("string")
```

Voorbeeld:

```
Type:contains("Event")
```

Dit resulteert in records met een type dat de tekenreeks 'Gebeurtenis'. Voorbeelden zijn onder meer **gebeurtenis**, **SecurityEvent**, en **ServiceFabricOperationEvent**.



### <a name="regular-expressions"></a>Reguliere expressies
U kunt een zoekcriterium voor een veld met een reguliere expressie opgeven met behulp van de **Regex** sleutelwoord. Zie voor een volledige beschrijving van de syntaxis die u in reguliere expressies gebruiken kunt [reguliere expressies gebruiken voor het filteren van logboek van zoekopdrachten in logboekanalyse](log-analytics-log-searches-regex.md).

Syntaxis:

```
field:Regex("Regular Expression")
```

Voorbeeld:

```
Computer:Regex("^C.*")
```

### <a name="logical-operators"></a>Logische operators
De querytalen ondersteuning voor de logische operators (*en*, *of*, en *niet*) en hun C-stijl-aliassen (*&&*,  *||* , en *!*respectievelijk). Haakjes kunt u deze operators groep.

Voorbeelden:

```
system OR error

```

```
Type:Alert AND NOT(Severity:1 OR ObjectId:"8066bbc0-9ec8-ca83-1edc-6f30d4779bcb8066bbc0-9ec8-ca83-1edc-6f30d4779bcb")
```

U kunt de logische operator voor de site op het hoogste filter argumenten weglaten. In dit geval wordt de operator AND verondersteld.

| Filterexpressie | Is gelijk aan |
| --- | --- |
| systeemfout |systeem en opstaan |
| systeem 'Windows Server' of ernst: 1 |systeem- en ('Windows Server' of ernst: 1) |

### <a name="wildcarding"></a>Gebruik van jokertekens
De query language ondersteunt het gebruik van de ( \* ) teken voor een of meer tekens voor een waarde in een query.

Voorbeeld:

 Alle computers met 'SQL' in de naam, zoals 'Redmond SQL' vinden.

```
Type=Event Computer=*SQL*
```

> [!NOTE]
> Op dit moment worden niet jokertekens gebruikt binnen offertes. Bijvoorbeeld, het bericht `"*This text*"` beschouwt de (\*) gebruikt als een letterlijke waarde (\*) teken.


## <a name="commands"></a>Opdrachten


De opdrachten van toepassing op de resultaten die door de query zijn geretourneerd. Gebruik het sluisteken (|) een opdracht toepassen op de opgehaalde resultaten. Meerdere opdrachten moeten worden gescheiden door het sluisteken.

> [!NOTE]
> Namen van opdrachten kunnen worden geschreven in hoofdletters of kleine letters, in tegenstelling tot de veldnamen en de gegevens.
>
>

### <a name="sort"></a>Sorteren
Syntaxis:

    sort field1 asc|desc, field2 asc|desc, …

Sorteert de resultaten worden op bepaalde velden. De asc/desc-achtervoegsel voor de resultaten in oplopende of aflopende volgorde sorteren is optioneel. Als dit wordt weggelaten, de *asc* sorteervolgorde wordt verondersteld. Voor de **TimeGenerated** veld *desc* wordt uitgegaan van sorteervolgorde, zodat deze de meest recente resultaten eerst standaard retourneert.

### <a name="toplimit"></a>Top/limiet
Syntaxis:

    top number


    limit number
Het antwoord op de bovenste N resultaten beperkt.

Voorbeeld:

    Type:Alert errors detected | top 10

Retourneert de bovenste 10 overeenkomende resultaten.

### <a name="skip"></a>Overslaan
Syntaxis:

    skip number

Slaat het aantal resultaten weergegeven.

Voorbeeld:

    Type:Alert errors detected | top 10 | skip 200

Retourneert de bovenste 10 overeenkomende resultaten vanaf resultaat 200.

### <a name="select"></a>Selecteer
Syntaxis:

    select field1, field2, ...

Resultaten beperkt tot de velden die u kiest.

Voorbeeld:

    Type:Alert errors detected | select Name, Severity

De velden die geretourneerde resultaten beperkt *naam* en *ernst*.

### <a name="measure"></a>meting
De *meting* opdracht wordt gebruikt voor statistische functies zijn van toepassing op de onbewerkte zoekresultaten. Dit is zeer nuttig zijn om op te halen *groeperen op* weergaven van de gegevens. Wanneer u de opdracht meting gebruikt, wordt een tabel met cumulatieve resultaten weergegeven in logboekanalyse zoeken.

**Syntaxis:**

    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]] by groupField1 [, groupField2 [, groupField3]]  [interval interval]


    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]]  interval interval



De resultaten worden op aggregeert *groupField*, en wordt de geaggregeerde meetwaarden berekend met behulp van *aggregatedField*.

| Statistische functie van meting | Beschrijving |
| --- | --- |
| *de statistische functie* |De naam van de statistische functie (hoofdlettergevoelig). De volgende statistische functies worden ondersteund: aantal, MAX, MIN, SUM, AVG, STDDEV, COUNTDISTINCT, percentiel ## of PCT ## (## is een getal tussen 1 en 99). |
| *aggregatedField* |Het veld dat is worden geaggregeerd. Dit veld is optioneel voor de statistische functie COUNT, maar moet een bestaand numeriek veld voor de som, MAX, MIN, AVG, STDEV, percentiel ## of PCT ## (## is een getal tussen 1 en 99). De aggregatedField kan ook worden een van de **uitbreiden** ondersteunde functies. |
| *fieldAlias* |De alias (optioneel) voor de berekende waarde geaggregeerd. Als niet wordt opgegeven, is de veldnaam **AggregatedValue**. |
| *groupField* |De naam van het veld die het resultaat ingesteld worden gegroepeerd per. |
| *Interval* |Het tijdsinterval, in de indeling:**nnnNAME**. **nnn**het getal positief geheel getal is. **NAAM** is de naam van het interval. Het interval namen zijn hoofdlettergevoelig en omvatten: MILLISECONDE [S] [S] seconde minuut [S] [S] uur dag [S] [S] maand en jaar [S]. |

De Intervaloptie kan alleen worden gebruikt in velden voor datum/tijd-groep (zoals *TimeGenerated* en *TimeCreated*). Op dit moment is dit niet wordt afgedwongen door de service, maar veld zonder datum/tijd die wordt doorgegeven aan de back-end zorgt ervoor dat een runtime-fout. Wanneer de schemavalidatie is geïmplementeerd, worden query's die gebruikmaken van velden zonder datum/tijd voor de aggregatie interval geweigerd door de API-service. De huidige *meting* implementatie ondersteunt groepering interval voor een statistische functie.

Als de BY-component wordt weggelaten, maar een interval dat is opgegeven (als een tweede syntaxis), de *TimeGenerated* veld wordt aangenomen dat standaard.

Voorbeelden:

**Voorbeeld 1**

    Type:Alert | measure count() as Count by ObjectId

Groepen van de waarschuwingen door *ObjectID*, en het aantal waarschuwingen voor elke groep wordt berekend. De toegevoegde waarde wordt geretourneerd als de *aantal* veld (alias).

**Voorbeeld 2**

    Type:Alert | measure count() interval 1HOUR

De waarschuwingen door 1 uur groepen met behulp van de *TimeGenerated* veld is en retourneert het aantal waarschuwingen in elk interval.

**Voorbeeld 3**

    Type:Alert | measure count() as AlertsPerHour interval 1HOUR

Hetzelfde als het vorige voorbeeld, maar met een alias geaggregeerd veld (*AlertsPerHour*).

**Voorbeeld 4**

    * | meting count() door TimeCreated interval 5DAYS

De resultaten door 5 dagintervallen groepen met behulp van de *TimeCreated* veld is en retourneert het aantal resultaten in elk interval.

**Voorbeeld 5**

    Type:Alert | measure max(Severity) by WorkflowName

Groepen van de waarschuwingen met de naam van de werkbelasting en retourneert de waarde van de maximale ernst van waarschuwing voor elke werkstroom.

**Voorbeeld 6**

    Type:Alert | measure min(Severity) by WorkflowName

Hetzelfde als het vorige voorbeeld, maar met de *min* functie geaggregeerd.

**Voorbeeld 7**

    Type:Perf | measure avg(CounterValue) by Computer

Perf gegroepeerd op de computer en wordt het gemiddelde (Gem) berekend.

**Voorbeeld 8**

    Type:Perf | measure sum(CounterValue) by Computer

Hetzelfde als het vorige voorbeeld, maar gebruikt *som*.

**Voorbeeld 9**

    Type:Perf | measure stddev(CounterValue) by Computer

Hetzelfde als het vorige voorbeeld, maar gebruikt *stddev*.

**Voorbeeld 10**

    Type:Perf | measure percentile70(CounterValue) by Computer

Hetzelfde als het vorige voorbeeld, maar gebruikt *percentile70*.

**Voorbeeld 11**

    Type:Perf | measure pct70(CounterValue) by Computer

Hetzelfde als het vorige voorbeeld, maar gebruikt *pct70*. Houd er rekening mee dat *PCT ##* is alleen een alias voor *percentiel ##* functie.

**Voorbeeld 12**

    Type:Perf | measure avg(CounterValue) by Computer, CounterName

Perf eerst gegroepeerd op de Computer en vervolgens CounterName en berekent de gemiddelde (Gem.).

**Voorbeeld 13**

    Type:Alert | measure count() as Count by WorkflowName | sort Count desc | top 5

Hiermee haalt u de bovenste vijf werkstromen met het maximum aantal waarschuwingen.

**Voorbeeld 14**

    * | meting countdistinct(Computer) per Type

Telt het aantal unieke computers melden voor elk Type.

**Voorbeeld 15**

    * | meting countdistinct(Computer) Interval 1 uur

Telt het aantal unieke computers melden voor elk uur.

**Voorbeeld 16**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total” | measure avg(CounterValue) by Computer Interval 1HOUR
```

Percentage processortijd door Computer groepen en retourneert het gemiddelde voor elk uur.

**Voorbeeld 17**

    Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES

W3CIISLog gegroepeerd op methode en retourneert de maximumwaarde voor elke 5 minuten.

**Voorbeeld 18**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer Interval 1HOUR
```

Percentage processortijd door computer groepen en retourneert de minimale, gemiddelde 75e percentiel en maximaal voor elk uur.

**Voorbeeld 19**

```
Type:Perf CounterName=”% Processor Time”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer, InstanceName Interval 1HOUR
```

Percentage processortijd eerst op de computer en vervolgens op exemplaarnaam groepen en retourneert de minimale, gemiddelde 75e percentiel en maximum voor elk uur.

**Voorbeeld 20**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | measure max(product(CounterValue,60)) as MaxDWPerMin by InstanceName Interval 1HOUR
```

Berekent het maximum van schrijfbewerkingen per minuut voor elke schijf op uw computer.

### <a name="where"></a>waar
Syntaxis:

```
**where** AggregatedValue>20
```

Kan alleen worden gebruikt na een *meting* opdracht nog verder filteren de geaggregeerde resulteert die de *meting* aggregatiefunctie heeft geproduceerd.

Voorbeelden:

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) as MAXCPU by Computer

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) by Computer | where (AggregatedValue>50 and AggregatedValue<90)



### <a name="dedup"></a>Ontdubbeling
Syntaxis:

    Dedup FieldName

Retourneert het eerste document gevonden voor elke unieke waarde van het opgegeven veld.

Voorbeeld:

    Type=Event | Dedup EventID | sort TimeGenerated DESC

In dit voorbeeld retourneert één gebeurtenis (de laatste gebeurtenis) per EventID.

### <a name="join"></a>Koppelen
Koppelt de resultaten van twee query vormt een set enkelvoudig resultaat wordt verkregen.  Biedt ondersteuning voor meerdere join-typen in de volgende tabel beschreven.

| Type samenvoeging | Beschrijving |
|:--|:--|
| interne | Alleen records met een overeenkomende waarde in beide query's retourneren. |
| buitenste | Alle records van beide query's worden geretourneerd.  |
| Links  | Alle records uit links query en overeenkomende records uit de juiste query retourneren. |


- Joins bieden momenteel geen ondersteuning voor query's met de **IN** sleutelwoord, de **meting** opdracht of het **uitbreiden** opdracht als deze is bedoeld voor een veld van de juiste query.
- U kunt slechts één veld momenteel opnemen in een join.
- Een enkele zoekactie mag niet meer dan één join inhouden.

**Syntaxis**

```
<left-query> | JOIN <join-type> <left-query-field-name> (<right-query>) <right-query-field-name>
```

**Voorbeelden**

U kunt lid worden van een gegevenstype verzameld van een aangepaste logboek MyBackup_CL aangeroepen met de heartbeat voor elke computer ter illustratie van de verschillende join-typen.  Deze gegevenstypen hebben de volgende gegevens.

`Type = MyBackup_CL`

| TimeGenerated | Computer | LastBackupStatus |
|:---|:---|:---|
| 4/20/2017 01:26:32.137 AM | Srv01.contoso.com | Geslaagd |
| 4/20/2017 02:13:12.381 AM | SRV02.contoso.com | Geslaagd |
| 4/20/2017 02:13:12.381 AM | srv03.contoso.com | Fout |

`Type = Hearbeat`(Alleen een subset van de velden die worden weergegeven)

| TimeGenerated | Computer | ComputerIP |
|:---|:---|:---|
| 21-4/2017 12:01:34.482 PM | Srv01.contoso.com | 10.10.100.1 |
| 21-4/2017 12:02:21.916 PM | SRV02.contoso.com | 10.10.100.2 |
| 21-4/2017 12:01:47.373 PM | srv04.contoso.com | 10.10.100.4 |

#### <a name="inner-join"></a>inner join

`Type=MyBackup_CL | join inner Computer (Type=Heartbeat) Computer`

Retourneert de volgende records waarbij het veld computer overeenkomt met voor beide gegevenstypen.

| Computer| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| Srv01.contoso.com | 4/20/2017 01:26:32.137 AM | Geslaagd | 21-4/2017 12:01:34.482 PM | 10.10.100.1 | Heartbeat |
| SRV02.contoso.com | 4/20/2017 02:13:12.381 AM | Geslaagd | 21-4/2017 12:02:21.916 PM | 10.10.100.2 | Heartbeat |


#### <a name="outer-join"></a>outer join

`Type=MyBackup_CL | join outer Computer (Type=Heartbeat) Computer`

Retourneert de volgende records voor beide gegevenstypen.

| Computer| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| Srv01.contoso.com | 4/20/2017 01:26:32.137 AM | Geslaagd  | 21-4/2017 12:01:34.482 PM | 10.10.100.1 | Heartbeat |
| SRV02.contoso.com | 4/20/2017 02:14:12.381 AM | Geslaagd  | 21-4/2017 12:02:21.916 PM | 10.10.100.2 | Heartbeat |
| srv03.contoso.com | 4/20/2017 01:33:35.974 AM | Fout  | 21-4/2017 12:01:47.373 PM | | |
| srv04.contoso.com |                           |          | 21-4/2017 12:01:47.373 PM | 10.10.100.2 | Heartbeat |



#### <a name="left-join"></a>linker join

`Type=MyBackup_CL | join left Computer (Type=Heartbeat) Computer`

Retourneert de volgende records van MyBackup_CL met een overeenkomende velden uit de Heartbeat.

| Computer| TimeGenerated | LastBackupStatus | TimeGenerated_joined | ComputerIP_joined | Type_joined |
|:---|:---|:---|:---|:---|:---|
| Srv01.contoso.com | 4/20/2017 01:26:32.137 AM | Geslaagd | 21-4/2017 12:01:34.482 PM | 10.10.100.1 | Heartbeat |
| SRV02.contoso.com | 4/20/2017 02:13:12.381 AM | Geslaagd | 21-4/2017 12:02:21.916 PM | 10.10.100.2 | Heartbeat |
| srv03.contoso.com | 4/20/2017 02:13:12.381 AM | Fout | | | |


### <a name="extend"></a>Breid uit
Hiermee kunt u de runtime-velden in query's maken. Houd er rekening mee dat runtime-velden met de opdracht meting kunnen niet worden gebruikt voor het uitvoeren van aggregatiebewerkingen.

**Voorbeeld 1**

    Type=SQLAssessmentRecommendation | Extend product(RecommendationScore, RecommendationWeight) AS RecommendationWeightedScore
Toont gewogen score aanbeveling voor aanbevelingen voor SQL-evaluatie.

**Voorbeeld 2**

    Type=Perf CounterName="Private Bytes" | EXTEND div(CounterValue,1024) AS KBs | Select CounterValue,Computer,KBs
Geeft de waarde van het prestatiemeteritem in kB in plaats van bytes.

**Voorbeeld 3**

    Type=WireData | EXTEND scale(TotalBytes,0,100) AS ScaledTotalBytes | Select ScaledTotalBytes,TotalBytes | SORT TotalBytes DESC
De waarde van WireData TotalBytes, schaalt zodat alle resultaten tussen 0 en 100 liggen zijn.

**Voorbeeld 4**

```
Type=Perf CounterName="% Processor Time" | EXTEND if(map(CounterValue,0,50,0,1),"HIGH","LOW") as UTILIZATION
```
Labels Perf teller waarden bevat die kleiner is dan 50 procent als laag en anderen zo hoog.

**Voorbeeld 5**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | Extend product(CounterValue,60) as DWPerMin| measure max(DWPerMin) by InstanceName Interval 1HOUR
```
Berekent het maximum van schrijfbewerkingen per minuut voor elke schijf op uw computer.

**Ondersteunde functies**

| Functie | Beschrijving | Syntaxisvoorbeelden |
| --- | --- | --- |
| ABS |Retourneert de absolute waarde van de opgegeven waarde of functie. |`abs(x)` <br> `abs(-5)` |
| BOOGCOS |Retourneert de arccosinus van een waarde of een functie. |`acos(x)` |
| en |Retourneert de waarde true en alleen als alle van de operands in waar resulteren. |`and(not(exists(popularity)),exists(price))` |
| ASIN |Retourneert de arcsinus van een waarde of een functie. |`asin(x)` |
| BOOGTAN |Retourneert de arctangens van een waarde of een functie. |`atan(x)` |
| BOOGTAN2 |Retourneert de hoek die voortvloeien uit de conversie van de coördinaten vierkant x, y naar poolgrafiek coördinaten. |`atan2(x,y)` |
| cbrt |Hoofdmap van de kubus. |`cbrt(x)` |
| ceil |Rondt af naar boven op een geheel getal. |`ceil(x)`  <br> `ceil(5.6)`Retourneert 6 |
| CO 's |Retourneert de cosinus van een hoek. |`cos(x)` |
| COSH |Retourneert de hyperbolische cosinus van een hoek. |`cosh(x)` |
| def |Afkorting voor standaard. Retourneert de waarde van veld"". Als het veld niet bestaat, retourneert de opgegeven standaardwaarde en levert de eerste waarde waar: `exists()==true`. |`def(rating,5)`. Deze functie def() resultaat de classificatie, of als er geen classificatie is opgegeven in het document 5. <br> `def(myfield, 1.0)`is gelijk aan `if(exists(myfield),myfield,1.0)`. |
| graden |Converteert radialen naar graden. |`deg(x)` |
| div |`div(x,y)`verdeelt x y. |`div(1,y)` <br> `div(sum(x,100),max(y,1))` |
| distributie |Retourneert de afstand tussen twee aanvalsvectoren, (punten) in een n-dimensionale ruimte. Wordt in de stroom, plus twee of meer exemplaren van ValueSource en wordt de afstand tussen de twee vectoren berekend. Elke ValueSource moet een getal zijn. Er moet een even aantal exemplaren van ValueSource doorgegeven en de methode wordt ervan uitgegaan dat de eerste helft vertegenwoordigen de eerste vector en de tweede helft vertegenwoordigen de tweede vector. |`dist(2, x, y, 0, 0)`Berekent de Euclidean afstand tussen (0,0) en (x, y) voor elk document. <br> `dist(1, x, y, 0, 0)`Berekent de afstand Manhattan (taxi) tussen (0,0) en (x, y) voor elk document. <br> `dist(2,,x,y,z,0,0,0)`Euclidean afstand tussen (0,0,0) en (x, y, z) voor elk document.<br>`dist(1,x,y,z,e,f,g)`Afstand tussen Manhattan (x, y, z) en (e, f, g), waarbij elke letter veldnaam is. |
| Er bestaat |Retourneert waar als een lid van het veld bestaat. |`exists(author)`Retourneert TRUE zijn voor elk document met een waarde in het veld 'auteur'.<br>`exists(query(price:5.00))`Retourneert ' True ' als 'price' overeenkomt, '5,00'. |
| EXP |Retourneert de Euler getal tot de macht x. |`exp(x)` |
| Floor |Rondt af naar beneden naar een geheel getal. |`floor(x)`  <br> `floor(5.6)`Retourneert 5 |
| hypo |Retourneert sqrt(sum(pow(x,2),pow(y,2))) zonder tussenliggende overloop of negatieve overloop. |`hypo(x,y)`  <br> ` |
| Als |Hiermee kunt voorwaardelijke functie query's. In `if(test,value1,value2)`, test of verwijst naar een logische waarde of expressie die als resultaat van een logische waarde (TRUE of FALSE). `value1`de waarde geretourneerd door de functie als test resulteert in waar. `value2`de waarde geretourneerd door de functie als test resulteert in FALSE. Een expressie kan een functie die Boole-waarden levert zijn. Het kan ook een functie retourneren numerieke waarden, waarin de case-waarde 0 wordt geïnterpreteerd als onwaar of retourneren tekenreeksen, in welk geval lege tekenreeks wordt geïnterpreteerd als onwaar zijn. |`if(termfreq(cat,'electronics'),popularity,42)`Deze functie wordt gecontroleerd of elk document om te zien of deze de term 'electronics' in het veld cat bevat. Als dit het geval is, wordt de waarde van het veld populariteit geretourneerd. Anders wordt de waarde van 42 geretourneerd. |
| Lineair |Implements `m*x+c`, m en c constanten zijn waarbij x staat voor een willekeurige functie. Dit is gelijkwaardig aan `sum(product(m,x),c)`, maar enigszins efficiënter zoals deze is geïmplementeerd als een enkele functie. |`linear(x,m,c) linear(x,2,4)`retourneert`2*x+4` |
| Rg |Retourneert het natuurlijke logboek van de opgegeven functie. |`ln(x)` |
| Logboek |Retourneert het logboek grondtal 10 van de opgegeven functie. |`log(x)   log(sum(x,100))` |
| Kaart |Alle waarden van een invoer x-functie die min en max, inclusief met het opgegeven doel vallen wordt toegewezen. De argumenten min en max moeten constanten zijn. Het doel van de argumenten en standaard kunnen zich constanten of functies. Als de waarde van x niet tussen min en max ligt, vervolgens wordt de waarde van x geretourneerd, of een standaardwaarde wordt geretourneerd als de opgegeven als een argument van 5e. |`map(x,min,max,target) map(x,0,0,1)`Eventuele waarden van 0 tot en met 1 gewijzigd. Dit kan nuttig zijn bij het verwerken van standaardwaarden 0 zijn.<br> `map(x,min,max,target,default)    map(x,0,100,1,-1)`Wijzigt een waarden tussen 0 en 100-1 en alle andere waarden op-1.<br>  `map(x,0,100,sum(x,599),docfreq(text,solr))`Eventuele waarden tussen 0 en 100 tot en met x + 599 en alle andere waarden voor de frequentie van de term 'solr' in de tekst van het gewijzigd. |
| Maximum aantal |Retourneert de maximumwaarde numerieke van meerdere geneste functies of constanten die worden opgegeven als argumenten: `max(x,y,...)`. De functie max kan ook nuttig zijn voor 'bottoming out' een andere functie of veld op sommige opgegeven constante.  Gebruik de `field(myfield,max)` syntaxis voor het selecteren van de maximumwaarde van één veld met meerdere waarden. |`max(myfield,myotherfield,0)` |
| min. |Retourneert de laagste numerieke waarde van meerdere geneste functies van constanten die worden opgegeven als argumenten: `min(x,y,...)`. De functie min kan ook nuttig voor het ontwikkelen van een 'bovengrens' op een functie met een constante zijn. Gebruik de `field(myfield,min)` syntaxis voor het selecteren van de minimumwaarde van één veld met meerdere waarden. |`min(myfield,myotherfield,0)` |
| Mod |Berekent de absolute waarde van de functie x door de functie y. |`mod(1,x)` <br> `mod(sum(x,100), max(y,1))` |
| MS |Retourneert de milliseconden van verschil tussen de argumenten. Datums zijn ten opzichte van de Unix- of POSIX-epoche tijd, middernacht, 1 januari 1970 UTC. Argumenten kunnen de naam van een geïndexeerde TrieDateField of datum math op basis van een constante datum of nu. `ms()`is gelijk aan `ms(NOW)`, aantal milliseconden dat sinds de epoche. `ms(a)`retourneert het aantal milliseconden sinds de epoche die staat voor het argument. `ms(a,b)`retourneert het aantal milliseconden dat b vindt plaats voordat een, namelijk `a - b`. |`ms(NOW/DAY)`<br>`ms(2000-01-01T00:00:00Z)`<br>`ms(mydatefield)`<br>`ms(NOW,mydatefield)`<br>`ms(mydatefield,2000-01-01T00:00:00Z)`<br>`ms(datefield1,datefield2)` |
| niet |De logisch genegeerde waarde van de ingepakte functie. |`not(exists(author))`GELDT alleen wanneer `exists(author)` is ingesteld op false. |
| of |Een logische splitsing. |`or(value1,value2)`De waarde TRUE wanneer beide value1 of value2 is ingesteld op true. |
| Pow |Genereert het opgegeven grondtal tot de opgegeven macht. `pow(x,y)`genereert x tot de macht van y. |`pow(x,y)`<br>`pow(x,log(y))`<br>`pow(x,0.5)`Hetzelfde als sqrt. |
| Product |Retourneert het product van meerdere waarden of functies, die zijn opgegeven in een door komma's gescheiden lijst. `mul(...)`kan ook worden gebruikt als een alias voor deze functie. |`product(x,y,...)`<br>`product(x,2)`<br>`product(x,y)`<br>`mul(x,y)` |
| recip |Een wederzijdse werkt met `recip(x,m,a,b)` implementeren `a/(m*x+b)`, waarbij m a en b zijn constanten en x staat voor een willekeurig complexe functie. Wanneer een b zijn gelijk en x > = 0, deze functie heeft een maximale waarde van 1 zakt x toeneemt. Vergroot de waarde van een en b samen resulteert in een verplaatsing van de gehele functie houden deel uit van de curve. Deze eigenschappen kunnen ervoor zorgen dat dit een ideaal werkt voor versterking van meer recente documenten, waarbij x `rord(datefield)`. |`recip(myfield,m,a,b)`<br>`recip(rord(creationDate),1,1000,1000)` |
| RAD |Converteert graden naar radialen. |`rad(x)` |
| afdrukken |Rondt af naar het dichtstbijzijnde gehele getal. |`rint(x)`  <br> `rint(5.6)`Retourneert 6 |
| sin |Retourneert de sinus van een hoek. |`sin(x)` |
| SINH |Retourneert de hyperbolische sinus van een hoek. |`sinh(x)` |
| Schaal |Schaalt waarden van de functie x, zodat ze tussen de opgegeven minTarget en maxTarget liggen vallen. De huidige implementatie passeert alle van de waarden van de functie min en max, verkrijgen, zodat deze de juiste schaal kunt selecteren. De huidige implementatie kan niet worden onderscheiden wanneer documenten zijn verwijderd, of de documenten die geen waarde. 0,0 waarden wordt gebruikt voor deze aanvragen. Dit betekent dat als waarden gewoonlijk alle groter zijn dan 0,0 zijn, een nog steeds eindigen met 0,0 als de min-waarde van kan waaruit moet worden toegewezen. In dergelijke gevallen een geschikte `map()` functie kan worden gebruikt als een tijdelijke oplossing 0,0 wijzigen in een waarde in het echte bereik, zoals hier wordt weergegeven:`scale(map(x,0,0,5),1,2)` |`scale(x,minTarget,maxTarget)`<br>`scale(x,1,2)`De waarden van de x-en schaalt zodat alle waarden tussen 1 en 2 liggen zijn. |
| SQRT |Retourneert de vierkantswortel van de opgegeven waarde of functie. |`sqrt(x)`<br>`sqrt(100)`<br>`sqrt(sum(x,100))` |
| strdist |Berekent de afstand tussen de twee tekenreeksen. De interface Lucene spellen checker StringDistance gebruikt en ondersteunt alle van de implementaties die beschikbaar zijn in dit pakket. U kunt ook zoektoepassingen in hun eigen via Solr van resource mogelijkheden laden. strdist duurt `(string1, string2, distance measure)`. Mogelijke waarden voor de afstand meting zijn:<ul><li>jw: Jaro Winkler</li><li>bewerken: afstand Levenstein of bewerken</li><li>ngram: de NGramDistance indien opgegeven, kan eventueel doorgeven in de grootte van de ngram te. De standaardwaarde is 2.</li><li>FQN: Volledig gekwalificeerde naam voor een implementatie van de interface StringDistance klasse. Moet een niet-arg constructor hebben.</li></ul> |`strdist("SOLR",id,edit)` |
| Sub |Retourneert de x-y van `sub(x,y)`. |`sub(myfield,myfield2)`<br>`sub(100,sqrt(myfield))` |
| Som |Retourneert de som van meerdere waarden of functies, die zijn opgegeven in een door komma's gescheiden lijst. `add(...)`kan worden gebruikt als een alias voor deze functie. |`sum(x,y,...)`<br>`sum(x,1)`<br>`sum(x,y)`<br>`sum(sqrt(x),log(y),z,0.5)`<br>`add(x,y)` |
| termfreq |Retourneert het aantal keren dat de term wordt weergegeven in het veld voor het document. |termfreq(Text,'memory') |
| Tan |Retourneert de tangens van een hoek. |`tan(x)` |
| TANH |Retourneert de hyperbolische tangens van een hoek. |`tanh(x)` |

## <a name="search-field-and-facet-reference"></a>Het veld en facet verwijzing zoeken
Wanneer u logboek zoeken gebruikt om gegevens te zoeken, worden de resultaten weergegeven verschillende veld en facetten. Enkele van de gegevens mogelijk niet erg beschrijvende weergegeven. Gebruik de volgende informatie om te begrijpen van de resultaten.

| Veld | Zoektype | Beschrijving |
| --- | --- | --- |
| TenantId |Alle |Gebruikt voor partitiegegevens. |
| TimeGenerated |Alle |Gebruikt om de tijdlijn, timeselectors (in de zoekopdracht en andere schermen). Dit vertegenwoordigt wanneer het gegevensitem is gegenereerd (meestal op de agent). De tijd wordt uitgedrukt in de ISO-indeling en is altijd UTC. In het geval van typen die zijn gebaseerd op bestaande instrumentation (dat wil zeggen, de gebeurtenissen in een logboek), is dit meestal de echte moment dat de vermelding/regel/logboekrecord werd geregistreerd. Voor sommige van de typen die worden gemaakt via management packs of in de cloud (bijvoorbeeld aanbevelingen of waarschuwingen), vertegenwoordigt de tijd een iets andere betekenis. Dit is de tijd wanneer deze nieuwe gegevensitem met een momentopname van een configuratie van sommige sorteren is verzameld, of een aanbeveling/waarschuwing is gemaakt op basis van deze. |
| Gebeurtenis-id |Gebeurtenis |Gebeurtenis-id in het Windows-gebeurtenislogboek. |
| Gebeurtenislogboek |Gebeurtenis |Gebeurtenislogboek waar de gebeurtenis is vastgelegd door Windows. |
| EventLevelName |Gebeurtenis |Kritieke/waarschuwing/informatie/geslaagd |
| eventLevel |Gebeurtenis |Numerieke waarde voor kritieke/waarschuwing/informatie/succes (EventLevelName in plaats daarvan gebruiken voor query's eenvoudiger/beter leesbaar). |
| SourceSystem |Alle |Waar de gegevens vandaan (een in termen van koppeling modus met de service). Voorbeelden zijn Microsoft System Center Operations Manager en Azure Storage. |
| Objectnaam |PerfHourly |Naam van het prestatieobject Windows. |
| InstanceName |PerfHourly |Windowsnaam van het exemplaar. |
| CounteName |PerfHourly |Naam Prestatiemeter Windows. |
| ObjectDisplayName |PerfHourly, ConfigurationAlert, ConfigurationObject, ConfigurationObjectProperty |Weergavenaam van het object waarop een prestatieverzamelingsregel in Operations Manager. Ook kan worden van de weergavenaam van het object dat is gedetecteerd door operationeel inzicht, of dat de waarschuwing is gegenereerd. |
| RootObjectName |PerfHourly, ConfigurationAlert, ConfigurationObject, ConfigurationObjectProperty |Weergavenaam van het bovenliggende lid van de bovenliggende (in een dubbele hosting-relatie) van het object waarop een prestatieverzamelingsregel in Operations Manager. Ook kan worden van de weergavenaam van het object dat is gedetecteerd door operationeel inzicht, of dat de waarschuwing is gegenereerd. |
| Computer |De meeste typen |De computernaam die de gegevens bij hoort. |
| Apparaatnaam |ProtectionStatus |De gegevens de naam van computer behoort tot (zelfde als 'Computer'). |
| DetectionId |ProtectionStatus | |
| ThreatStatusRank |ProtectionStatus |Threat status rang is een numerieke representatie van de status van de threat. Vergelijkbaar met HTTP-antwoordcodes, is de volgorde onderbrekingen tussen de getallen (daarom geen bedreigingen is 150 en niet 100 of 0), waardoor er ruimte overblijft om toe te voegen nieuwe statussen. Voor een rollup threat status en de beveiligingsstatus van is de bedoeling om de slechtste status dat de computer de geselecteerde periode is weer te geven. De getallen rangschikken de verschillende toestanden, zodat u de record met het hoogste aantal zoeken kunt. |
| ThreatStatus |ProtectionStatus |Beschrijving van ThreatStatus, 1:1 met ThreatStatusRank toegewezen. |
| TypeofProtection |ProtectionStatus |Antimalware-product dat is gedetecteerd in de computer: none, Microsoft Malware-hulpprogramma, Forefront, enzovoort. |
| ScanDate |ProtectionStatus | |
| SourceHealthServiceId |ProtectionStatus, RequiredUpdate |HealthService-ID voor de agent van deze computer. |
| HealthServiceId |De meeste typen |HealthService-ID voor de agent van deze computer. |
| ManagementGroupName |De meeste typen |Naam beheergroep voor agents met Operations Manager zijn gekoppeld. Anders is null/leeg. |
| objectType |ConfigurationObject |Typ (zoals in Operations Manager management pack-type/klasse) voor dit object, door logboekanalyse configuratie assessment gedetecteerd. |
| UpdateTitle |RequiredUpdate |De naam van de update die gevonden is niet geïnstalleerd. |
| PublishDate |RequiredUpdate |Wanneer de update is gepubliceerd op Microsoft Update. |
| Server |RequiredUpdate |De gegevens de naam van computer behoort tot (zelfde als 'Computer'). |
| Product |RequiredUpdate |Product dat de update van toepassing op. |
| UpdateClassification |RequiredUpdate |Type update (bijvoorbeeld updatepakket of service pack). |
| KBID |RequiredUpdate |KB-artikel-ID die deze best practice of update beschrijft. |
| WorkflowName |ConfigurationAlert |Naam van de regel of monitor die de waarschuwing geproduceerd. |
| Ernst |ConfigurationAlert |Ernst van de waarschuwing. |
| Prioriteit |ConfigurationAlert |De prioriteit van de waarschuwing. |
| IsMonitorAlert |ConfigurationAlert |Is deze waarschuwing gegenereerd door een monitor (true) of een regel (ONWAAR)? |
| AlertParameters |ConfigurationAlert |De XML met de parameters van de waarschuwing logboekanalyse. |
| Context |ConfigurationAlert |De XML met de context van de waarschuwing logboekanalyse. |
| Workload |ConfigurationAlert |Technologie of werkbelasting dat de waarschuwing naar verwijst. |
| AdvisorWorkload |Aanbeveling |Technologie of werkbelasting die de aanbeveling naar verwijst. |
| Beschrijving |ConfigurationAlert |Beschrijving van waarschuwing (korte). |
| DaysSinceLastUpdate |UpdateAgent |Hoeveel dagen geleden (ten opzichte van TimeGenerated van deze record) deze agent het een update van Windows Server Update Service (WSUS) of Microsoft Update installeren? |
| DaysSinceLastUpdateBucket |UpdateAgent |Op basis van DaysSinceLastUpdate, een categorisatie in tijd buckets van hoe lang geleden een computer installatie van de vorige updates vanaf WSUS of Microsoft Update. |
| AutomaticUpdateEnabled |UpdateAgent |Automatische updates controleren in- of uitgeschakeld voor deze agent? |
| AutomaticUpdateValue |UpdateAgent |Controleert Automatische updates is ingesteld op automatisch downloaden en installeren, alleen downloaden of alleen controleren? |
| WindowsUpdateAgentVersion |UpdateAgent |Het versienummer van de Microsoft Update-agent. |
| WSUSServer |UpdateAgent |Welke WSUS-server is gericht voor deze update-agent? |
| OSVersion |UpdateAgent |De versie van het besturingssysteem dat op deze update-agent wordt uitgevoerd. |
| Naam |Aanbeveling, ConfigurationObjectProperty |Naam/titel van de aanbeveling of de naam van de eigenschap Log Analytics configuratie evaluatie. |
| Waarde |ConfigurationObjectProperty |De waarde van een eigenschap van logboekanalyse configuratie bepalen. |
| KBLink |Aanbeveling |URL naar het KB-artikel die deze best practice of update beschrijft. |
| RecommendationStatus |Aanbeveling |Aanbevelingen zijn tussen de enkele typen waarvan de records wordt bijgewerkt, niet alleen toegevoegd aan de search-index. Deze status wordt gewijzigd of de aanbeveling is actief/openen, of als logboekanalyse detecteert is opgelost. |
| RenderedDescription |Gebeurtenis |Beschrijving (opnieuw gebruikte tekst met ingevulde parameters) van een Windows-gebeurtenis weergegeven. |
| ParameterXml |Gebeurtenis |De XML met de parameters in de gegevenssectie van een Windows-gebeurtenis (zoals te zien in Logboeken). |
| EventData |Gebeurtenis |De XML met de hele gegevenssectie van een Windows-gebeurtenis (zoals te zien in Logboeken). |
| Bron |Gebeurtenis |Gebeurtenislogboek bron van de gebeurtenis heeft gegenereerd. |
| Culture |Gebeurtenis |De categorie van de gebeurtenis, rechtstreeks vanuit de Windows-gebeurtenislogboek. |
| Gebruikersnaam |Gebeurtenis |Gebruikersnaam van de Windows-gebeurtenis (meestal NT AUTHORITY\LOCALSYSTEM). |
| SampleValue |PerfHourly |Gemiddelde waarde voor de aggregatie op uurbasis van een prestatiemeteritem. |
| Min |PerfHourly |De minimumwaarde in het elk uur een prestaties teller Uurlijkse aggregatie-interval. |
| Max. |PerfHourly |Maximale waarde in het elk uur een prestaties teller Uurlijkse aggregatie-interval. |
| Percentile95 |PerfHourly |De 95e percentielwaarde voor de per uur een prestaties teller Uurlijkse aggregatie-interval. |
| SampleCount |PerfHourly |Voorbeelden van de teller hoeveel prestatie Onverwerkt werden gebruikt voor het produceren van dit per uur cumulatieve record. |
| Threat |ProtectionStatus |De naam van malware. |
| StorageAccount |W3CIISLog |Azure Storage-account het logboek is gelezen uit. |
| AzureDeploymentID |W3CIISLog |Azure-implementatie-ID van de cloudservice het logboek behoort. |
| Rol |W3CIISLog |Functie van de Azure-cloud-service het logboek behoort. |
| RoleInstance |W3CIISLog |RoleInstance van de Azure-functie die deel uitmaakt van het logboek op. |
| sSiteName |W3CIISLog |IIS-website die deel uitmaakt van het logboek voor (metabase notatie); het veld s-sitename in het oorspronkelijke logboek. |
| sComputerName |W3CIISLog |Het veld s-computername in het oorspronkelijke logboek. |
| sIP |W3CIISLog |Server IP-adres van de HTTP-aanvraag is geadresseerd aan. Het veld s-ip in het oorspronkelijke logboek. |
| csMethod |W3CIISLog |HTTP-methode (bijvoorbeeld GET/POST) die wordt gebruikt door de client in de HTTP-aanvraag. De cs-methode in de oorspronkelijke logboek. |
| Overschrijving |W3CIISLog |IP-adres van de HTTP-aanvraag van de client van afkomstig is. Het veld c-ip in het oorspronkelijke logboek. |
| csUserAgent |W3CIISLog |HTTP-gebruikersagent gedeclareerd door de client (browser of anderszins). Cs-user-agent in het oorspronkelijke logboek. |
| scStatus |W3CIISLog |HTTP-statuscode (bijvoorbeeld: 200/403/500) geretourneerd door de server naar de client. De cs-status in het oorspronkelijke logboek. |
| timeTaken |W3CIISLog |Hoe lang (in milliseconden) dat de aanvraag heeft geduurd om te voltooien. Het veld timetaken in het oorspronkelijke logboek. |
| csUriStem |W3CIISLog |Relatieve URI (zonder hostadres, die is, / zoeken) die is aangevraagd. Het veld cs uristem in het oorspronkelijke logboek. |
| csUriQuery |W3CIISLog |URI-query. URI-query's zijn alleen nodig voor dynamische pagina's, zoals ASP-pagina's, zodat dit veld meestal een koppelteken voor statische's bevat. |
| sPort |W3CIISLog |Server-poort die de HTTP-aanvraag is verzonden naar (en dat IIS luistert naar, aangezien deze opgepikt). |
| csUserName |W3CIISLog |Naam van gebruiker geverifieerd als de aanvraag geverifieerd en niet-anonieme is. |
| csVersion |W3CIISLog |HTTP-protocolversie die wordt gebruikt in de aanvraag (bijvoorbeeld HTTP/1.1). |
| csCookie |W3CIISLog |Informatie van de cookie. |
| csReferer |W3CIISLog |De site die de gebruiker voor het laatst heeft bezocht. Deze site bevat een koppeling naar de huidige site. |
| csHost |W3CIISLog |Host-header (bijvoorbeeld www.mysite.com) die is aangevraagd. |
| scSubStatus |W3CIISLog |Fout-substatuscode geregistreerd. |
| scWin32Status |W3CIISLog |Windows-statuscode geregistreerd. |
| csBytes |W3CIISLog |Bytes verzonden in de aanvraag van de client naar de server. |
| scBytes |W3CIISLog |Bytes terug in het antwoord van de server naar de client geretourneerd. |
| ConfigChangeType |Configuratiewijziging |Type wijziging (bijvoorbeeld WindowsServices/Software). |
| ChangeCategory |Configuratiewijziging |De categorie van de wijziging (toegevoegde-Modified/verwijderd). |
| SoftwareType |Configuratiewijziging |Type software (Update/toepassing). |
| SoftwareName |Configuratiewijziging |De naam van de software (alleen van toepassing op wijzigingen in de software). |
| Uitgever |Configuratiewijziging |Leverancier die de software (alleen van toepassing op wijzigingen in de software) publiceert. |
| SvcChangeType |Configuratiewijziging |Type wijziging die is toegepast op een Windows-service (status/StartupType/pad/ServiceAccount). Dit is alleen van toepassing op Windows-servicewijzigingen. |
| SvcDisplayName |Configuratiewijziging |Weergavenaam van de service die is gewijzigd. |
| SvcName |Configuratiewijziging |Naam van de service die is gewijzigd. |
| SvcState |Configuratiewijziging |Nieuwe (huidige) status van de service. |
| SvcPreviousState |Configuratiewijziging |Vorige bekende status van de service (alleen van toepassing als de status van service gewijzigd). |
| SvcStartupType |Configuratiewijziging |Opstarttype van service. |
| SvcPreviousStartupType |Configuratiewijziging |Vorige opstarttype (alleen van toepassing als opstarttype gewijzigd). |
| SvcAccount |Configuratiewijziging |Service-account. |
| SvcPreviousAccount |Configuratiewijziging |Vorige serviceaccount (alleen van toepassing als serviceaccount gewijzigd). |
| SvcPath |Configuratiewijziging |Pad naar het uitvoerbare bestand van de Windows-service. |
| SvcPreviousPath |Configuratiewijziging |Vorige pad van het uitvoerbare bestand voor de Windows-service (alleen van toepassing als deze gewijzigd). |
| SvcDescription |Configuratiewijziging |Beschrijving van de service. |
| Vorige |Configuratiewijziging |Eerdere status van deze software (geïnstalleerde/niet geïnstalleerde vorige versie). |
| Huidige |Configuratiewijziging |Meest recente toestand van deze software (geïnstalleerde/niet-geïnstalleerde/huidige versie). |

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over logboek zoekacties:

* Vertrouwd raken met [zoekopdrachten naar logboeken](log-analytics-log-searches.md) om gedetailleerde informatie te bekijken die is verzameld door oplossingen.
* Gebruik [aangepaste velden in logboekanalyse](log-analytics-custom-fields.md) logboek zoekopdrachten uitbreiden.
