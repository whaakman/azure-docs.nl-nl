---
title: Automatisch schalen rekenknooppunten in een Azure Batch-pool | Microsoft Docs
description: Schakel automatisch schalen op een cloud-toepassingen op het aantal rekenknooppunten in de pool dynamisch aanpassen.
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: c624cdfc-c5f2-4d13-a7d7-ae080833b779
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: multiple
ms.date: 06/20/2017
ms.author: lahugh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fa5588ae31e63ae54e654ef26563c7570fe4cd13
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55459839"
---
# <a name="create-an-automatic-scaling-formula-for-scaling-compute-nodes-in-a-batch-pool"></a>Een formule voor automatisch vergroten/verkleinen voor het schalen van rekenknooppunten in een Batch-pool maken

Azure Batch kunt automatisch schalen voor pools op basis van de parameters die u definieert. Met automatisch schalen, knooppunten in Batch dynamisch worden toegevoegd aan een groep als taak eisen toename en rekenknooppunten wordt verwijderd als ze verlagen. U bespaart tijd en geld door het automatisch aanpassen van het aantal rekenknooppunten die worden gebruikt door uw Batch-toepassing. 

Inschakelen van automatisch schalen in een pool van rekenknooppunten door te koppelen aan deze een *formule voor automatisch schalen* die u definieert. De Batch-service gebruikt de formule voor automatisch schalen om te bepalen het aantal rekenknooppunten die nodig zijn voor het uitvoeren van uw workload. COMPUTE-knooppunten kunnen worden toegewezen knooppunten of [knooppunten met lage prioriteit](batch-low-pri-vms.md). Batch reageert op service metrische gegevens die regelmatig worden verzameld. Batch met behulp van deze metrische gegevens, wordt het aantal rekenknooppunten in de pool op basis van de formule en op basis van een configureerbaar interval aangepast.

Automatisch schalen wanneer u een pool maakt of op een bestaande pool, kunt u inschakelen. U kunt ook een bestaande formule op een groep die is geconfigureerd voor automatisch schalen wijzigen. Batch kunt u uw formules evalueren alvorens ze toe te wijzen aan groepen en het controleren van de status van automatisch schalen wordt uitgevoerd.

Dit artikel worden de verschillende entiteiten die gezamenlijk uw formules voor automatisch schalen, met inbegrip van variabelen, operators, bewerkingen en -functies. We bespreken hoe verschillende compute-resource en taak metrics in Batch te verkrijgen. U kunt deze metrische gegevens gebruiken om aan te passen van de pool-knooppunten op basis van gebruik en de taak van de resource. Vervolgens wordt beschreven hoe u een formule en inschakelen van automatisch schalen in een pool met behulp van de Batch REST en .NET-API's. Ten slotte wij sluiten af met maken een paar formules.

> [!IMPORTANT]
> Wanneer u een Batch-account maakt, kunt u de [accountconfiguratie](batch-api-basics.md#account), die bepaalt of pools worden toegewezen in een Batch-service-abonnement (de standaardinstelling) of in uw gebruikersabonnement. Als u uw Batch-account met de standaardconfiguratie van de Batch-Service gemaakt, zijn uw account is beperkt tot een maximum aantal kernen dat kan worden gebruikt voor verwerking. De Batch-service kan worden geschaald rekenknooppunten tot deze limiet core. Daarom kan de Batch-service het doelaantal rekenknooppunten dat is opgegeven door een formule voor automatisch schalen niet bereiken. Zie [quota en limieten voor de Azure Batch-service](batch-quota-limit.md) voor informatie over het weergeven en uw accountquota verhogen.
>
>Als u uw account hebt gemaakt met de accountconfiguratie gebruikersabonnement, zijn uw account van bestandsshares in het quotum voor kernen voor het abonnement. Zie [Virtual Machines limits](../azure-subscription-service-limits.md#virtual-machines-limits) (Limieten voor Virtuele Machines) in [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md) (Azure-abonnement en servicelimieten, -quota en -beperkingen) voor meer informatie.
>
>

## <a name="automatic-scaling-formulas"></a>Automatisch vergroten/verkleinen formules
Een formule voor automatisch vergroten/verkleinen is een tekenreekswaarde die u definieert die een of meer instructies bevat. De formule voor automatisch schalen is toegewezen aan een groep van toepassingen [autoScaleFormula] [ rest_autoscaleformula] element (Batch REST) of [CloudPool.AutoScaleFormula] [ net_cloudpool_autoscaleformula] de eigenschap (Batch .NET). De Batch-service gebruikt de formule om te bepalen het doelaantal rekenknooppunten in de groep voor het volgende interval van de verwerking. De formule tekenreeks mag niet meer dan 8 KB, tot 100 instructies die worden gescheiden door puntkomma's, en mag regeleinden en opmerkingen kunt opnemen.

U kunt automatisch vergroten/verkleinen formules beschouwen als een functie voor automatisch schalen Batch 'taal'. Formule-instructies zijn gratis gevormd expressies die zowel de service gedefinieerde variabelen (variabelen gedefinieerd door de Batch-service) en de gebruiker gedefinieerde variabelen (variabelen die u definieert) kunnen bevatten. Ze kunnen verschillende bewerkingen op basis van deze waarden met behulp van ingebouwde typen, operatoren en functies uitvoeren. Bijvoorbeeld, een instructie kan duren voordat de volgende notatie:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Formules bevatten gewoonlijk meerdere instructies die bewerkingen uitvoeren op waarden die zijn verkregen in de vorige instructies. Bijvoorbeeld eerst we een waarde voor verkrijgen `variable1`, klikt u vervolgens doorgeven aan een functie voor het vullen van `variable2`:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Deze instructies uit in de formule voor automatisch schalen om naar een doelaantal rekenknooppunten opnemen. Toegewezen knooppunten en knooppunten met lage prioriteit hebben hun eigen doelinstellingen, zodat u kunt een doel voor elk type knooppunt definiëren. Een formule voor automatisch schalen kan de doelwaarde van een toegewezen knooppunten en/of een doelwaarde voor knooppunten met lage prioriteit bevatten.

Het doelaantal knooppunten kan hoger zijn, lager of gelijk zijn aan het huidige aantal knooppunten van dat type in de groep. Batch evalueert de formule voor automatisch schalen van een pool in een bepaald interval (Zie [automatisch vergroten/verkleinen intervallen](#automatic-scaling-interval)). Batch past het doelgetal van elk type knooppunt in de groep van het getal dat de formule voor automatisch schalen Hiermee geeft u op het moment van de evaluatie.

### <a name="sample-autoscale-formula"></a>Formule voor automatisch schalen

Hier volgt een voorbeeld van een formule voor automatisch schalen die u voor de meeste scenario's kan worden aangepast. De variabelen `startingNumberOfVMs` en `maxNumberofVMs` in het voorbeeld formule kan worden aangepast aan uw behoeften. Deze formule toegewezen knooppunten schaalt, maar kan worden gewijzigd om toe te passen op schaal met lage prioriteit knooppunten ook. 

```
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicatedNodes=min(maxNumberofVMs, pendingTaskSamples);
```

Met deze formule voor automatisch schalen wordt in eerste instantie de pool gemaakt met een enkele virtuele machine. De `$PendingTasks` metric definieert het aantal taken die uitgevoerd of in de wachtrij worden. De formule wordt het gemiddelde aantal in behandeling zijnde taken gevonden in de afgelopen 180 seconden en stelt de `$TargetDedicatedNodes` variabele dienovereenkomstig. De formule zorgt ervoor dat doel toegewezen knooppunten nooit meer dan 25 VM's. Als nieuwe taken worden verzonden, wordt de pool automatisch vergroot. Als de taken zijn voltooid, virtuele machines gratis één voor één en worden de formule voor automatisch schalen verkleint de pool.

## <a name="variables"></a>Variabelen
U kunt beide **gedefinieerde** en **zelfgedefinieerde** variabelen in uw formules voor automatisch schalen. De service gedefinieerde variabelen zijn ingebouwd in de Batch-service. Sommige variabelen voor de service gedefinieerde lezen / schrijven zijn, en andere alleen-lezen. Variabelen voor de gebruiker gedefinieerde zijn variabelen die u definieert. In het van de voorbeeldformule wordt weergegeven in de vorige sectie `$TargetDedicatedNodes` en `$PendingTasks` zijn gedefinieerde variabelen. Variabelen `startingNumberOfVMs` en `maxNumberofVMs` zijn van de gebruiker gedefinieerde variabelen.

> [!NOTE]
> Variabelen voor de service gedefinieerde worden altijd voorafgegaan door een dollarteken ($). Voor de gebruiker gedefinieerde variabelen is het dollarteken optioneel.
>
>

De volgende tabellen ziet lezen / schrijven en alleen-lezen-variabelen die zijn gedefinieerd door de Batch-service.

U kunt ophalen en instellen van de waarden van deze variabelen gedefinieerd met een service voor het beheren van het aantal rekenknooppunten in een pool:

| Variabelen voor de service gedefinieerde lezen / schrijven | Description |
| --- | --- |
| $TargetDedicatedNodes |Het doelaantal toegewezen rekenknooppunten voor de pool. Het aantal toegewezen knooppunten is opgegeven als doel, omdat het gewenste aantal knooppunten niet altijd door een groep bereiken mogelijk. Bijvoorbeeld, als het doelaantal toegewezen knooppunten wordt gewijzigd door een evaluatie voor automatisch schalen voordat de groep van het eerste doel is bereikt, klikt u vervolgens de pool bereikt mogelijk niet het doel. <br /><br /> Een pool in die zijn gemaakt met de configuratie van de Batch-Service kan het doel niet bereiken als het doel een Batch-account knooppunt of core-quotum overschrijdt. Een pool in die zijn gemaakt met de accountconfiguratie gebruikersabonnement kan het doel niet bereiken als het doel is groter dan het gedeelde kernquotum voor het abonnement.|
| $TargetLowPriorityNodes |Het doelaantal met lage prioriteit rekenknooppunten voor de pool. Het aantal knooppunten met lage prioriteit is opgegeven als doel, omdat het gewenste aantal knooppunten niet altijd door een groep bereiken mogelijk. Bijvoorbeeld, als het doelaantal knooppunten met lage prioriteit wordt gewijzigd door een evaluatie voor automatisch schalen voordat de groep van het eerste doel is bereikt, klikt u vervolgens de pool bereikt mogelijk niet het doel. Een pool kan ook niet het doel bereiken als het doel een Batch-account knooppunt of core-quotum overschrijdt. <br /><br /> Zie voor meer informatie over rekenknooppunten met lage prioriteit [VM's met lage prioriteit gebruiken met Batch (Preview)](batch-low-pri-vms.md). |
| $NodeDeallocationOption |De actie die wordt uitgevoerd wanneer de rekenknooppunten zijn verwijderd uit een pool. Mogelijke waarden zijn:<ul><li>**taak**--taken onmiddellijk beëindigd en weer in de wachtrij geplaatst, zodat ze opnieuw worden gepland.<li>**beëindigen**--taken onmiddellijk beëindigd en worden deze verwijderd uit de wachtrij.<li>**taskcompletion**--moet wachten voor actieve taken om te voltooien en vervolgens het knooppunt verwijderd uit de groep.<li>**retaineddata**--alle lokale taak behouden gegevens wacht op het knooppunt om te worden opgeschoond voordat u het knooppunt wordt verwijderd uit de groep.</ul> |

U kunt de waarde van deze service gedefinieerde variabelen aan te passen op basis van metrische gegevens van de Batch-service krijgen:

| Alleen-lezen service gedefinieerde variabelen | Description |
| --- | --- |
| $CPUPercent |Het gemiddelde percentage van CPU-gebruik. |
| $WallClockSeconds |Het aantal seconden verbruikt. |
| $MemoryBytes |Het gemiddelde aantal megabytes gebruikt. |
| $DiskBytes |Het gemiddelde aantal GB op de lokale schijven gebruikt. |
| $DiskReadBytes |Het aantal gelezen bytes. |
| $DiskWriteBytes |Het aantal bytes dat is geschreven. |
| $DiskReadOps |Het aantal gelezen schijfbewerkingen die worden uitgevoerd. |
| $DiskWriteOps |Het aantal schrijfbewerkingen schijf uitgevoerd. |
| $NetworkInBytes |Het aantal inkomende bytes. |
| $NetworkOutBytes |Het aantal uitgaande bytes. |
| $SampleNodeCount |Het aantal rekenknooppunten. |
| $ActiveTasks |Het aantal taken die zijn gereed om uit te voeren, maar nog niet zijn uitgevoerd. Het aantal $ActiveTasks bevat alle taken die zijn opgenomen in de actieve status en waarvan afhankelijkheden is voldaan. Alle taken die zijn opgenomen in de status actief, maar waarvoor afhankelijkheden zijn niet tevreden zijn uitgesloten van het aantal $ActiveTasks.|
| $RunningTasks |Het aantal taken in een status running doorbrengt. |
| $PendingTasks |De som van $ActiveTasks en $RunningTasks. |
| $SucceededTasks |Het aantal taken dat voltooid is. |
| $FailedTasks |Het aantal taken dat is mislukt. |
| $CurrentDedicatedNodes |Het huidige aantal toegewezen rekenknooppunten. |
| $CurrentLowPriorityNodes |Het huidige aantal met lage prioriteit compute-knooppunten, met inbegrip van knooppunten die bezet zijn. |
| $PreemptedNodeCount | Het aantal knooppunten in de pool die zich in een bezet staat. |

> [!TIP]
> De alleen-lezen, de service gedefinieerde variabelen die worden weergegeven in de vorige tabel zijn *objecten* die verschillende methoden voor toegang tot gegevens die zijn gekoppeld aan elk bieden. Zie voor meer informatie, [verkrijgen voorbeeldgegevens](#getsampledata) verderop in dit artikel.
>
>

## <a name="types"></a>Typen
Deze typen worden ondersteund in een formule:

* double
* doubleVec
* doubleVecList
* string
* tijdstempel--tijdstempel is een samengestelde structuur met de volgende leden:

  * jaar
  * maand (1-12)
  * dag (1-31)
  * weekdag (in de indeling van het nummer van; bijvoorbeeld: 1 voor maandag)
  * uur (in 24-uurs notatie, bijvoorbeeld: 13 betekent dat 1 uur)
  * minuten (00: 59)
  * tweede (00: 59)
* timeinterval

  * TimeInterval_Zero
  * TimeInterval_100ns
  * TimeInterval_Microsecond
  * TimeInterval_Millisecond
  * TimeInterval_Second
  * TimeInterval_Minute
  * TimeInterval_Hour
  * TimeInterval_Day
  * TimeInterval_Week
  * TimeInterval_Year

## <a name="operations"></a>Bewerkingen
Deze bewerkingen zijn toegestaan voor de typen die worden vermeld in de vorige sectie.

| Bewerking | Ondersteunde operators | Resultaattype |
| --- | --- | --- |
| dubbele *operator* dubbele |+, -, *, / |double |
| dubbele *operator* timeinterval |* |timeinterval |
| doubleVec *operator* dubbele |+, -, *, / |doubleVec |
| doubleVec *operator* doubleVec |+, -, *, / |doubleVec |
| TimeInterval *operator* dubbele |*, / |timeinterval |
| TimeInterval *operator* timeinterval |+, - |timeinterval |
| TimeInterval *operator* timestamp |+ |tijdstempel |
| tijdstempel *operator* timeinterval |+ |tijdstempel |
| tijdstempel *operator* timestamp |- |timeinterval |
| *operator*double |-, ! |double |
| *operator*timeinterval |- |timeinterval |
| dubbele *operator* dubbele |<, <=, ==, >=, >, != |double |
| tekenreeks *operator* tekenreeks |<, <=, ==, >=, >, != |double |
| tijdstempel *operator* timestamp |<, <=, ==, >=, >, != |double |
| TimeInterval *operator* timeinterval |<, <=, ==, >=, >, != |double |
| dubbele *operator* dubbele |&&, &#124;&#124; |double |

Bij het testen van een Double-waarde met een ternaire operator (`double ? statement1 : statement2`), niet nul is **waar**, en is nul **false**.

## <a name="functions"></a>Functions
Deze vooraf gedefinieerde **functies** zijn beschikbaar voor gebruik bij het definiëren van een formule voor automatisch vergroten/verkleinen.

| Function | Retourtype | Description |
| --- | --- | --- |
| avg(doubleVecList) |double |Retourneert de gemiddelde waarde voor alle waarden in de doubleVecList. |
| len(doubleVecList) |double |Retourneert de lengte van de vector die is gemaakt op basis van de doubleVecList. |
| LG(Double) |double |Retourneert het logboek grondtal 2 van de dubbele waarde. |
| lg(doubleVecList) |doubleVec |Retourneert het component-wise logboek grondtal 2 van de doubleVecList. Een vec(double) moeten expliciet worden doorgegeven voor de parameter. Anders wordt wordt de versie van de dubbele lg(double) verondersteld. |
| ln(Double) |double |Retourneert het natuurlijke logboek van de dubbele waarde. |
| ln(doubleVecList) |doubleVec |Retourneert het component-wise logboek grondtal 2 van de doubleVecList. Een vec(double) moeten expliciet worden doorgegeven voor de parameter. Anders wordt wordt de versie van de dubbele lg(double) verondersteld. |
| log(Double) |double |Retourneert het logboek grondtal 10 van de dubbele waarde. |
| log(doubleVecList) |doubleVec |Retourneert het component-wise logboek grondtal 10 van de doubleVecList. Een vec(double) moeten expliciet worden doorgegeven voor de één dubbele parameter. Anders wordt wordt de versie van de dubbele log(double) verondersteld. |
| Max(doubleVecList) |double |Retourneert de maximumwaarde in de doubleVecList. |
| min(doubleVecList) |double |Retourneert de minimumwaarde in de doubleVecList. |
| norm(doubleVecList) |double |Retourneert de twee-norm van de vector die is gemaakt op basis van de doubleVecList. |
| percentiel (v doubleVec, dubbele p) |double |Het element percentiel van de vector v geretourneerd. |
| ASELECT() |double |Retourneert een willekeurige waarde tussen 0,0 en 1,0 liggen. |
| range(doubleVecList) |double |Retourneert het verschil tussen de minimale en maximale waarden in de doubleVecList. |
| std(doubleVecList) |double |Retourneert de standaardafwijking van voorbeeld van de waarden in de doubleVecList. |
| stop() | |Stopt de evaluatie van de expressie voor automatisch schalen. |
| SUM(doubleVecList) |double |Retourneert de som van alle onderdelen van de doubleVecList. |
| time(string dateTime="") |tijdstempel |De tijdstempel van de huidige tijd als er geen parameters worden doorgegeven of het tijdstempel van de datum/tijd-tekenreeks geretourneerd als deze wordt doorgegeven. Ondersteunde datum-/ tijdindelingen zijn W3C-DTF- en RFC 1123. |
| Val (v doubleVec, dubbele i) |double |Retourneert de waarde van het element dat op locatie i in vector v, met een startIndex gelijk is aan nul. |

Enkele van de functies die worden beschreven in de vorige tabel kunt een lijst accepteren als argument. De lijst met door komma's gescheiden is een combinatie van *dubbele* en *doubleVec*. Bijvoorbeeld:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

De *doubleVecList* waarde wordt geconverteerd naar een enkel *doubleVec* voor de evaluatie. Bijvoorbeeld, als `v = [1,2,3]`, klikt u vervolgens aanroepen `avg(v)` is gelijk aan aanroepen `avg(1,2,3)`. Aanroepen van `avg(v, 7)` is gelijk aan aanroepen `avg(1,2,3,7)`.

## <a name="getsampledata"></a>Voorbeeldgegevens downloaden
Automatisch schalen formules reageren op metrische gegevens (voorbeelden) die wordt geleverd door de Batch-service. Een formule groeit of krimpt poolgrootte op basis van de waarden die wordt verkregen van de service. De service gedefinieerde variabelen die zijn beschreven zijn eerder objecten die verschillende methoden bieden voor toegang tot gegevens die is gekoppeld aan dit object. De volgende expressie ziet u bijvoorbeeld een aanvraag voor het ophalen van de laatste vijf minuten van CPU-gebruik:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| Methode | Description |
| --- | --- |
| GetSample() |De `GetSample()` methode retourneert een vector van voorbeelden van gegevens.<br/><br/>Een voorbeeld is 30 seconden metrische gegevens. Voorbeelden zijn met andere woorden, elke 30 seconden verkregen. Maar, zoals hieronder vermeld, is er een vertraging tussen wanneer een voorbeeld dat wordt verzameld en wanneer deze is beschikbaar op een formule. Daarom mogelijk niet alle voorbeelden voor een bepaalde periode zijn beschikbaar voor de evaluatie van een formule.<ul><li>`doubleVec GetSample(double count)`<br/>Hiermee geeft u het aantal voorbeelden voor het verkrijgen van de meest recente voorbeelden die zijn verzameld.<br/><br/>`GetSample(1)` retourneert de laatste steekproef beschikbaar. Metrische gegevens zoals `$CPUPercent`, maar deze moet niet worden gebruikt omdat het is niet mogelijk om te weten *wanneer* het voorbeeld is verzameld. Kan het zijn recente of, vanwege problemen met het systeem, is het mogelijk veel ouder. Is het beter in dergelijke gevallen voor het gebruik van een bepaalde periode, zoals hieronder weergegeven.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Hiermee geeft u een tijdsbestek voor het verzamelen van voorbeeldgegevens. (Optioneel) geeft het ook het percentage van de voorbeelden die beschikbaar in het aangevraagde tijdsbestek zijn moeten.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)` Voorbeelden van 20 zou worden geretourneerd als alle voorbeelden voor de afgelopen 10 minuten aanwezig zijn in de geschiedenis CPUPercent. Als de laatste minuut van de geschiedenis niet beschikbaar is, echter zou alleen 18 voorbeelden worden geretourneerd. In dit geval:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)` mislukken omdat alleen 90 procent van de voorbeelden beschikbaar zijn.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)` zou slagen.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Hiermee geeft u een tijdsbestek voor het verzamelen van gegevens, met zowel een begintijd en eindtijd.<br/><br/>Zoals eerder vermeld, is er een vertraging tussen wanneer een voorbeeld dat wordt verzameld en wanneer deze is beschikbaar op een formule. Houd rekening met deze vertraging op wanneer u de `GetSample` methode. Zie `GetSamplePercent` hieronder. |
| GetSamplePeriod() |Retourneert de periode van voorbeelden die zijn uitgevoerd in een verzameling historische voorbeeldgegevens. |
| Count() |Retourneert het totale aantal voorbeelden in de geschiedenis van metrische gegevens. |
| HistoryBeginTime() |Retourneert het tijdstempel van de oudste voorbeeld van de beschikbare gegevens voor de metrische gegevens. |
| GetSamplePercent() |Retourneert het percentage van de voorbeelden die beschikbaar voor een bepaald tijdsinterval zijn. Bijvoorbeeld:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Omdat de `GetSample` methode mislukt als het percentage van de voorbeelden die wordt geretourneerd is kleiner dan de `samplePercent` opgegeven, kunt u de `GetSamplePercent` methode om eerst te controleren. Vervolgens kunt u een alternatieve actie uitvoeren als er onvoldoende voorbeelden aanwezig is, zijn zonder stopgezet, kunt u de automatische schaling evaluatie. |

### <a name="samples-sample-percentage-and-the-getsample-method"></a>Voorbeelden, voorbeeld-percentage, en de *GetSample()* methode
De hoofdbewerking van een formule voor automatisch schalen is het verkrijgen van metrische gegevens, taken en resources en pas vervolgens de grootte van de opslaggroep op basis van die gegevens. Hierdoor is het belangrijk dat u een duidelijk beeld krijgt van de interactie van formules voor automatisch schalen met metrische gegevens (voorbeelden).

**Voorbeelden**

De Batch-service wordt regelmatig duurt voorbeelden van taken en resources metrische gegevens en maakt ze beschikbaar voor uw formules voor automatisch schalen. Deze voorbeelden worden elke 30 seconden worden vastgelegd door de Batch-service. Er is echter meestal een vertraging tussen wanneer deze voorbeelden zijn vastgelegd en wanneer ze beschikbaar worden gesteld aan (en kunnen worden gelezen door) uw formules voor automatisch schalen. Bovendien verschillende factoren, zoals netwerk- of andere problemen infrastructuur, kunnen voorbeelden niet worden geregistreerd voor een bepaald interval.

**Voorbeeld-percentage**

Wanneer `samplePercent` wordt doorgegeven aan de `GetSample()` methode of de `GetSamplePercent()` methode wordt aangeroepen, _procent_ verwijst naar een vergelijking tussen het totale aantal voorbeelden die worden vastgelegd door de Batch-service en het aantal voorbeelden die beschikbaar voor de formule voor automatisch schalen zijn.

We bekijken een timespan 10 minuten als voorbeeld. Omdat voorbeelden worden elke 30 seconden binnen een periode van 10 minuten geregistreerd, is het maximum aantal voorbeelden die worden vastgelegd door Batch 20 voorbeelden (2 per minuut). Echter, vanwege de intrinsieke latentie van het mechanisme voor rapportage en andere problemen binnen Azure, kunnen er alleen 15 voorbeelden die beschikbaar voor de formule voor automatisch schalen om te lezen zijn. Dus, bijvoorbeeld voor deze periode 10 minuten alleen 75% van het totale aantal steekproeven vastgelegd mogelijk beschikbaar op uw formule.

**Voorbeeld van GetSample() en bereiken**

Uw formules voor automatisch schalen gaat worden vergroten en verkleinen uw pools &mdash; knooppunten toe te voegen of te verwijderen van knooppunten. Omdat knooppunten u geld kost, die u wilt ervoor zorgen dat uw formules een intelligente methode van de analyse op basis van voldoende gegevens gebruikt. Daarom raden wij aan dat u een analyse-trends in uw formules. Dit type meegroeit en -krimpt van uw toepassingen op basis van tal van voorbeelden die worden verzameld.

Om dit te doen, gebruik `GetSample(interval look-back start, interval look-back end)` om terug te keren een vector van voorbeelden:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Als de bovenstaande regel wordt geëvalueerd door de Batch, wordt een reeks voorbeelden als vector van waarden geretourneerd. Bijvoorbeeld:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Zodra u de vector van voorbeelden hebt verzameld, kunt u vervolgens functies zoals `min()`, `max()`, en `avg()` voor het afleiden van betekenisvolle waarden uit de verzamelde bereik.

Voor extra beveiliging kunt u afdwingen dat een formule-evaluatie mislukt als minder dan een bepaald percentage van het voorbeeld beschikbaar voor een bepaalde periode is. Wanneer u een formule-evaluatie mislukt afdwingen, u opdracht geven Batch gestaakt verder evaluatie van de formule als het opgegeven percentage van de voorbeelden niet beschikbaar is. In dit geval wordt geen wijziging aangebracht in grootte van de groep. Als u een vereiste percentage van de voorbeelden voor de evaluatie te voltooien, dit opgeven als de derde parameter `GetSample()`. Hier, is een vereiste van 75 procent van de voorbeelden opgegeven:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

Omdat er mogelijk een vertraging in de voorbeeld-beschikbaarheid, is het belangrijk dat u altijd een tijdsperiode opgeven met een look-back-begintijd die ouder is dan een minuut. Het duurt ongeveer een minuut voor voorbeelden worden doorgegeven via het systeem, dus voorbeelden in het bereik `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` mogelijk niet beschikbaar. Nogmaals, kunt u de parameter percentage van `GetSample()` om af te dwingen een bepaalde voorbeeld percentage vereiste.

> [!IMPORTANT]
> We **wordt ten zeerste aangeraden** die u **voorkomen relying *alleen* op `GetSample(1)` in uw formules voor automatisch schalen**. Dit komt doordat `GetSample(1)` in feite vertelt naar de Batch-service, "Geven mij het laatste voorbeeld u hebben, ongeacht hoe lang geleden u deze hebt opgehaald." Omdat dit slechts een enkele voorbeeld is en kan een oudere voorbeeld, mogelijk vertegenwoordiger van de grotere afbeelding van recente taak of de status van resource niet meer. Als u gebruik `GetSample(1)`, zorg ervoor dat het deel uitmaakt van een grotere instructie en niet de enige gegevenspunt die afhankelijk van uw formule.
>
>

## <a name="metrics"></a>Metrische gegevens
U kunt metrische gegevens over zowel de bron en de taak gebruiken tijdens het definiëren van een formule. U aanpassen het doelaantal toegewezen knooppunten in de groep op basis van de metrische gegevens die u verkrijgen en evalueren. Zie de [variabelen](#variables) sectie hierboven voor meer informatie over alle gegevens.

<table>
  <tr>
    <th>Gegevens</th>
    <th>Description</th>
  </tr>
  <tr>
    <td><b>Resource</b></td>
    <td><p>Metrische gegevens voor resources zijn gebaseerd op de CPU, de bandbreedte, het geheugengebruik van compute-knooppunten en het aantal knooppunten.</p>
        <p> Deze service gedefinieerde variabelen zijn handig voor het maken van aanpassingen op basis van aantal knooppunten:</p>
    <p><ul>
            <li>$TargetDedicatedNodes</li>
            <li>$TargetLowPriorityNodes</li>
            <li>$CurrentDedicatedNodes</li>
            <li>$CurrentLowPriorityNodes</li>
            <li>$preemptedNodeCount</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Deze service gedefinieerde variabelen zijn handig voor het maken van aanpassingen op basis van Resourcegebruik knooppunt:</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>Taak</b></td>
    <td><p>Taak metrische gegevens zijn gebaseerd op de status van taken, zoals actief, in behandeling, en voltooid. De volgende service gedefinieerde variabelen zijn handig voor poolgrootte aanpassingen op basis van metrische gegevens van taak:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Een formule voor automatisch schalen schrijven
U een formule voor automatisch schalen kunt maken met de instructies die gebruikmaken van de bovenstaande onderdelen gevormd en vervolgens combineren die instructies in een formule voltooien. In deze sectie maken we een voorbeeld voor automatisch schalen-formule die enkele echte vergroten/verkleinen beslissingen kunt uitvoeren.

Eerst gaan we de vereisten voor onze nieuwe formule voor automatisch schalen definiëren. De formule moet:

1. Vergroot het doelaantal toegewezen rekenknooppunten in een groep als CPU-gebruik hoog is.
2. Het beoogd aantal toegewezen rekenknooppunten in een pool afnemen wanneer het CPU-gebruik laag is.
3. Altijd het maximum aantal toegewezen knooppunten tot 400 beperken.

Als u wilt het aantal knooppunten tijdens hoog CPU-gebruik verhogen, definieert u de instructie die een gebruiker gedefinieerde variabele gevuld (`$totalDedicatedNodes`) met een waarde die is 110 procent van het huidige doelaantal toegewezen knooppunten, maar alleen als de minimale gemiddelde CPU-gebruik tijdens de de laatste tien minuten is meer dan 70 procent. Gebruik anders de waarde voor het huidige aantal toegewezen knooppunten.

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
```

Naar *verlagen* het aantal toegewezen knooppunten tijdens het lage CPU-gebruik, de volgende instructie in de formule stelt hetzelfde `$totalDedicatedNodes` variabele à 90 procent van de huidige beoogd aantal toegewezen knooppunten als de gemiddelde CPU-gebruik in het verleden 60 minuten is dan 20 procent. Gebruik anders de huidige waarde van `$totalDedicatedNodes` die we in de instructie hiervoor hebt ingevuld.

```
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
```

Nu het doelaantal toegewezen rekenknooppunten tot een maximum van 400 beperken:

```
$TargetDedicatedNodes = min(400, $totalDedicatedNodes)
```

Dit is de volledige formule:

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
$TargetDedicatedNodes = min(400, $totalDedicatedNodes)
```

## <a name="create-an-autoscale-enabled-pool-with-net"></a>Een pool voor automatisch schalen ingeschakeld maken met .NET

Een groep gemaakt met automatisch schalen ingeschakeld in .NET, de volgende stappen uit:

1. Maken van de groep met [BatchClient.PoolOperations.CreatePool](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.createpool).
2. Stel de [CloudPool.AutoScaleEnabled](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleenabled) eigenschap `true`.
3. Stel de [CloudPool.AutoScaleFormula](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula) eigenschap met de formule voor automatisch schalen.
4. (Optioneel) Stel de [CloudPool.AutoScaleEvaluationInterval](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval) eigenschap (de standaardwaarde is 15 minuten).
5. Doorvoeren van de groep met [CloudPool.Commit](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commit) of [CommitAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commitasync).

Het volgende codefragment maakt een pool voor automatisch schalen is ingeschakeld in .NET. De formule voor automatisch schalen van de pool wordt het doelaantal toegewezen knooppunten tot en met 5 op maandag 1 op om de dag van de week. De [interval voor automatisch vergroten/verkleinen](#automatic-scaling-interval) is ingesteld op 30 minuten. In dit document en de andere C#-fragmenten in dit artikel `myBatchClient` is een goed geïnitialiseerd exemplaar van de [BatchClient] [ net_batchclient] klasse.

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool(
                    poolId: "mypool",
                    virtualMachineSize: "standard_d1_v2",
                    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));    
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
await pool.CommitAsync();
```

> [!IMPORTANT]
> Niet opgeeft wanneer u een pool voor automatisch schalen is ingeschakeld, de _targetDedicatedComputeNodes_ parameter of het _targetLowPriorityComputeNodes_ parameter bij het aanroepen van **CreatePool** . Geef in plaats daarvan de **AutoScaleEnabled** en **AutoScaleFormula** eigenschappen in de pool. De waarden voor deze eigenschappen bepalen het doelgetal van elk type knooppunt. Ook aan handmatig een automatisch schalen ingeschakeld formaat pool (bijvoorbeeld met [BatchClient.PoolOperations.ResizePoolAsync][net_poolops_resizepoolasync]), eerste **uitschakelen** automatisch schalen op het een pool en wijzig de grootte ervan.
>
>

Naast Batch .NET, kunt u een van de andere [Batch-SDK's](batch-apis-tools.md#azure-accounts-for-batch-development), [Batch REST](https://docs.microsoft.com/rest/api/batchservice/), [Batch PowerShell-cmdlets](batch-powershell-cmdlets-get-started.md), en de [Batch CLI](batch-cli-get-started.md) naar automatisch schalen configureren.


### <a name="automatic-scaling-interval"></a>Interval voor automatisch vergroten/verkleinen
Standaard past de Batch-service een poolgrootte op basis van een formule voor automatisch schalen om de 15 minuten. Dit interval kan worden geconfigureerd met behulp van de volgende eigenschappen van de groep van toepassingen:

* [CloudPool.AutoScaleEvaluationInterval][net_cloudpool_autoscaleevalinterval] (Batch .NET)
* [autoScaleEvaluationInterval][rest_autoscaleinterval] (REST API)

Het minimale interval is vijf minuten en de maximumwaarde is 168 uur. Als een interval buiten dit bereik is opgegeven, retourneert de Batch-service een fout met ongeldige aanvraag (400).

> [!NOTE]
> Automatisch schalen is momenteel niet bedoeld om te reageren op wijzigingen in minder dan een minuut, maar in plaats daarvan is bedoeld voor het aanpassen van de grootte van uw groep geleidelijk als u een workload wordt uitgevoerd.
>
>

## <a name="enable-autoscaling-on-an-existing-pool"></a>Schakel automatisch schalen op een bestaande pool

Elke Batch-SDK biedt een manier om in te schakelen van automatisch schalen. Bijvoorbeeld:

* [BatchClient.PoolOperations.EnableAutoScaleAsync][net_enableautoscaleasync] (Batch .NET)
* [Inschakelen van automatisch schalen in een pool] [ rest_enableautoscale] (REST-API)

Wanneer u automatisch schalen op een bestaande pool inschakelt, houd rekening met de volgende punten:

* Als automatisch schalen is momenteel uitgeschakeld op de pool wanneer u de aanvraag voor het inschakelen van automatisch schalen, moet u een formule voor automatisch schalen die geldige opgeven wanneer u de aanvraag. U kunt optioneel een evaluatie-interval voor automatisch schalen opgeven. Als u een interval niet opgeeft, wordt de standaardwaarde van 15 minuten gebruikt.
* Als voor automatisch schalen is momenteel ingeschakeld op de groep, kunt u een formule voor automatisch schalen, een evaluatie-interval of beide opgeven. U moet ten minste één van deze eigenschappen.

  * Als u een nieuwe evaluatie-interval voor automatisch schalen opgeeft, zijn de bestaande planning voor de evaluatie is gestopt en een nieuw schema wordt gestart. Begintijd van het nieuwe schema is de tijd waarop de aanvraag voor het inschakelen van automatisch schalen is uitgegeven.
  * Als u de formule voor automatisch schalen of evaluatie-interval, de Batch-service weglaat gaat u door met de huidige waarde van de instelling.

> [!NOTE]
> Als u hebt opgegeven waarden voor de *targetDedicatedComputeNodes* of *targetLowPriorityComputeNodes* parameters van de **CreatePool** methode tijdens het maken van de groep in .NET, of voor de vergelijkbare parameters in een andere taal, klikt u vervolgens deze waarden worden genegeerd tijdens het automatisch vergroten/verkleinen formule wordt geëvalueerd.
>
>

Dit C#-codefragment maakt gebruik van de [Batch .NET] [ net_api] bibliotheek om in te schakelen van automatisch schalen op een bestaande pool:

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Een formule voor automatisch schalen bijwerken

Voor het bijwerken van de formule op een bestaande pool voor automatisch schalen is ingeschakeld, moet u de bewerking voor het inschakelen van automatisch schalen opnieuw met de nieuwe formule aanroepen. Bijvoorbeeld, als automatisch schalen is al ingeschakeld op `myexistingpool` wanneer de volgende .NET-code wordt uitgevoerd, een formule voor automatisch schalen is vervangen door de inhoud van `myNewFormula`.

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a>Het interval voor automatisch schalen bijwerken

Voor het bijwerken van de evaluatie-interval voor automatisch schalen van een bestaande pool voor automatisch schalen is ingeschakeld, moet u de bewerking voor het inschakelen van automatisch schalen opnieuw met het nieuwe interval aanroepen. Als u bijvoorbeeld het instellen van de evaluatie-interval voor automatisch schalen en 60 minuten voor een groep die al voor automatisch schalen is ingeschakeld in .NET:

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Evalueren van een formule voor automatisch schalen

U kunt een formule evalueren voordat dit aan een groep wordt toegepast. Op deze manier kunt u de formule om te zien hoe de instructies evalueren voordat u de formule in productie testen.

Als u wilt evalueren in een formule voor automatisch schalen, moet u eerst automatisch schalen in de pool met een geldige formule inschakelen. Als u wilt testen op een formule in een groep die nog geen automatisch schalen die zijn ingeschakeld, gebruikt u de formule één regel `$TargetDedicatedNodes = 0` wanneer u automatisch schalen voor het eerst inschakelt. Gebruik vervolgens een van de volgende voor de evaluatie van de formule die u wilt testen:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscale) of [EvaluateAutoScaleAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscaleasync)

    Deze Batch .NET-methoden moet de ID van een bestaande pool en een tekenreeks met de formule voor automatisch schalen om te evalueren.

* [Evalueren van een formule voor automatisch vergroten/verkleinen](https://docs.microsoft.com/rest/api/batchservice/evaluate-an-automatic-scaling-formula)

    In deze REST-API-aanvraag, geeft de groeps-ID in de URI en de formule voor automatisch schalen in de *autoScaleFormula* element van de aanvraagtekst. Het antwoord van de bewerking bevat foutgegevens die hebben mogelijk betrekking op de formule.

In deze [Batch .NET] [ net_api] codefragment, we een formule voor automatisch schalen beoordelen. Als de groep heeft geen automatisch schalen die zijn ingeschakeld, wordt deze optie eerst inschakelen.

```csharp
// First obtain a reference to an existing pool
CloudPool pool = await batchClient.PoolOperations.GetPoolAsync("myExistingPool");

// If autoscaling isn't already enabled on the pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula to enable autoscaling on the
    // pool. This formula is valid, but won't resize the pool:
    await pool.EnableAutoScaleAsync(
        autoscaleFormula: "$TargetDedicatedNodes = $CurrentDedicatedNodes;",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScaleAsync calls to once every 30 seconds.
    // Because we want to apply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here to ensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh the properties of the pool so that we've got the
    // latest value for AutoScaleEnabled
    await pool.RefreshAsync();
}

// We must ensure that autoscaling is enabled on the pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // The formula to evaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicatedNodes = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform the autoscale formula evaluation. Note that this code does not
    // actually apply the formula to the pool.
    AutoScaleRun eval =
        await batchClient.PoolOperations.EvaluateAutoScaleAsync(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print the results of the AutoScaleRun.
        // This will display the values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply the formula to the pool since it evaluated successfully
        await batchClient.PoolOperations.EnableAutoScaleAsync(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output the message associated with the error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

Geslaagde evaluatie van de formule in dit codefragment wordt getoond geeft een resultaat is vergelijkbaar met:

```
AutoScaleRun.Results:
    $TargetDedicatedNodes=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Informatie over automatisch schalen wordt uitgevoerd

Om ervoor te zorgen dat de formule wordt uitgevoerd zoals verwacht, is het raadzaam dat u de resultaten van de uitvoeringen van automatisch schalen die Batch in de pool uitvoert periodiek controleren. Dus, (of vernieuwen) een verwijzing naar de groep, en controleert u de eigenschappen van de laatste automatisch schalen worden uitgevoerd.

In Batch .NET, de [CloudPool.AutoScaleRun](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscalerun) eigenschap heeft een aantal eigenschappen die informatie geven over de meest recente automatische schaalaanpassing uitgevoerd in de pool worden uitgevoerd:

* [AutoScaleRun.Timestamp](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.timestamp)
* [AutoScaleRun.Results](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.results)
* [AutoScaleRun.Error](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.error)

In de REST-API, de [informatie ophalen over een pool](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool) aanvraag retourneert informatie over de groep van toepassingen, waaronder de meest recente automatisch schalen die informatie worden uitgevoerd de [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool#bk_autrun) eigenschap.

De volgende C#-codefragment maakt gebruik van de Batch .NET-bibliotheek om informatie over de laatste automatische schaalaanpassing uitgevoerd op de pool af te drukken _myPool_:

```csharp
await Cloud pool = myBatchClient.PoolOperations.GetPoolAsync("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Voorbeeld van de uitvoer van het bovenstaande codefragment:

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicatedNodes=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>Voorbeeld voor automatisch schalen formules
We bekijken een paar formules die tonen verschillende manieren om aan te passen van de hoeveelheid rekenresources in een pool.

### <a name="example-1-time-based-adjustment"></a>Voorbeeld 1: Op basis van tijd aanpassing
Stel dat u wilt aanpassen van de grootte van de groep op basis van de dag van de week en tijd van de dag. In dit voorbeeld laat zien hoe het vergroten of verkleinen het aantal knooppunten in de pool dienovereenkomstig.

De formule verkrijgt eerst de huidige tijd. Als het een dag (1-5), en binnen de werkuren (8 AM tot 6 uur), wordt de grootte van de doel-groep is ingesteld op 20 knooppunten. Anders wordt ingesteld op 10 knooppunten.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicatedNodes = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>Voorbeeld 2: Taakgebaseerde aanpassing
In dit voorbeeld wordt de grootte van de groep aangepast op basis van het aantal taken in de wachtrij. Alle opmerkingen en regeleinden zijn acceptabel in formule tekenreeksen.

```csharp
// Get pending tasks for the past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use the last sample point,
// otherwise we use the maximum of last sample point and the history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicatedNodes/2);
// The pool size is capped at 20, if target VM value is more than that, set it
// to 20. This value should be adjusted according to your use case.
$TargetDedicatedNodes = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>Voorbeeld 3: De administratieve verwerking van parallelle taken
Dit voorbeeld wordt de grootte van de groep op basis van het aantal taken. Deze formule ook rekening met de [MaxTasksPerComputeNode] [ net_maxtasks] waarde die is ingesteld voor de pool. Deze methode is nuttig in situaties waar [parallelle uitvoering van de taak](batch-parallel-node-tasks.md) is ingeschakeld in de pool.

```csharp
// Determine whether 70 percent of the samples have been recorded in the past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set to 4, adjust this number
// for your use case)
$cores = $TargetDedicatedNodes * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicatedNodes + $extraVMs);
// Attempt to grow the number of compute nodes to match the number of active
// tasks, with a maximum of 3
$TargetDedicatedNodes = max(0,min($targetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>Voorbeeld 4: De groepsgrootte van een eerste instellen
In dit voorbeeld ziet u een C#-codefragment met een formule voor automatisch schalen die Hiermee stelt u de grootte van de groep op een opgegeven aantal knooppunten voor een initiële periode. En vervolgens het past u de grootte van de groep op basis van het aantal die wordt uitgevoerd en actieve taken na de eerste periode is verstreken.

De formule in het volgende codefragment:

* Hiermee stelt u de groepsgrootte van de eerste met vier knooppunten.
* Grootte van de groep niet aangepast binnen de eerste 10 minuten van de levenscyclus van de pool.
* Na 10 minuten, verkrijgt u de maximale waarde van het aantal actieve en actieve taken in de afgelopen 60 minuten.
  * Als beide waarden zijn 0 (waarmee wordt aangegeven dat er geen taken uitgevoerd of is actief in de afgelopen 60 minuten zijn), wordt de groepsgrootte die is ingesteld op 0.
  * Als een waarde groter dan nul is, wordt er geen wijziging wordt aangebracht.

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicatedNodes = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicatedNodes = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicatedNodes) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>Volgende stappen
* [Gebruik van Azure Batch compute-bronnen met gelijktijdige knooppunttaken maximaliseren](batch-parallel-node-tasks.md) bevat details over hoe u meerdere taken tegelijkertijd op de rekenknooppunten in uw pool uitvoeren kunt. Naast het automatisch schalen kunt deze functie te verlagen, de duur van de taak voor sommige werkbelastingen, waardoor u geld bespaart.
* Voor een andere efficiëntie booster, moet u ervoor zorgen dat uw Batch-toepassing query's voor de Batch-service in de meest optimale manier. Zie [efficiënt query uitvoeren op de Azure Batch-service](batch-efficient-list-queries.md) voor informatie over het beperken van de hoeveelheid gegevens die de filteracties op wanneer u de status van misschien wel duizenden rekenknooppunten of taken niet opvragen.

[net_api]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch
[net_batchclient]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.batchclient
[net_cloudpool_autoscaleformula]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula
[net_cloudpool_autoscaleevalinterval]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval
[net_enableautoscaleasync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.enableautoscaleasync
[net_maxtasks]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.maxtaskspercomputenode
[net_poolops_resizepoolasync]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.resizepoolasync

[rest_api]: https://docs.microsoft.com/rest/api/batchservice/
[rest_autoscaleformula]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
[rest_autoscaleinterval]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
[rest_enableautoscale]: https://docs.microsoft.com/rest/api/batchservice/enable-automatic-scaling-on-a-pool
