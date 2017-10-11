---
title: Automatisch schalen rekenknooppunten in een Azure Batch-pool | Microsoft Docs
description: Schakel automatisch schalen op een cloud-pool dynamisch aanpassen zodat het aantal rekenknooppunten in de pool.
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: c624cdfc-c5f2-4d13-a7d7-ae080833b779
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: multiple
ms.date: 06/20/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f0e49cd8a64a48c53f5b6104703164a597c797f0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-automatic-scaling-formula-for-scaling-compute-nodes-in-a-batch-pool"></a>Een automatische schaling formule voor het schalen van rekenknooppunten in een Batch-pool maken

Azure Batch kan pools op basis van de parameters die u definieert automatisch schalen. Met automatisch schalen Batch worden dynamisch knooppunten toegevoegd aan een groep als de taak eisen toename en rekenknooppunten wordt verwijderd als ze afnemen. U kunt zowel tijd en geld opslaan door het automatisch aanpassen van het aantal rekenknooppunten gebruikt door de Batch-toepassing. 

Inschakelen van automatisch schalen op een pool van rekenknooppunten door te koppelen aan een *formule voor automatisch schalen* die u definieert. De Batch-service gebruikt de formule voor automatisch schalen om te bepalen het aantal rekenknooppunten die nodig zijn voor het uitvoeren van uw workload. Knooppunten kunnen worden toegewezen knooppunten COMPUTE of [prioriteit Laag knooppunten](batch-low-pri-vms.md). Batch reageert op service metrische gegevens die regelmatig wordt verzameld. Met deze metrische gegevens Batch Hiermee past u het aantal rekenknooppunten in de groep op basis van de formule en met een configureerbare interval.

U kunt inschakelen automatische schaling tijdens het maken van een groep of op een bestaande toepassingen. U kunt ook een bestaande formule op een groep die is geconfigureerd voor automatisch schalen wijzigen. Batch kunt u kunt uw formules evalueren voordat u eraan toe te wijzen aan groepen en het controleren van de status van automatisch vergroten/verkleinen wordt uitgevoerd.

Dit artikel worden de verschillende entiteiten die gezamenlijk uw formules automatisch schalen, met inbegrip van variabelen, operators, bewerkingen en functies. Het verkrijgen van verschillende compute resource en taak metrische gegevens in Batch besproken. Deze metrische gegevens kunt u een aantal van uw pool knooppunten op basis van gebruiks- en resourcestatus aanpassen. We vervolgens wordt beschreven hoe een formule maken en inschakelen van automatisch schalen op een groep met de Batch REST en de .NET API's. Ten slotte voltooien we met een paar voorbeeld formules.

> [!IMPORTANT]
> Wanneer u een Batch-account maakt, kunt u de [accountconfiguratie](batch-api-basics.md#account), waarmee wordt bepaald of groepen in een Batch-service-abonnement (de standaardinstelling) of in uw gebruikerabonnement zijn toegewezen. Als u uw Batch-account hebt gemaakt met de standaardconfiguratie van de Batch-Service, zijn uw account is beperkt tot een maximum aantal kernen die kunnen worden gebruikt voor verwerking. De Batch-service schaalt rekenknooppunten alleen tot die core is bereikt. Daarom kan de Batch-service niet het doelaantal rekenknooppunten opgegeven met een formule voor automatisch schalen bereikt. Zie [quota en limieten voor de Azure Batch-service](batch-quota-limit.md) voor informatie over het weergeven en uw accountquota verhogen.
>
>Als u uw account hebt gemaakt met de configuratie van het abonnement van de gebruiker, klikt u vervolgens deelt uw account in het quotum voor kernen voor het abonnement. Zie [Virtual Machines limits](../azure-subscription-service-limits.md#virtual-machines-limits) (Limieten voor Virtuele Machines) in [Azure subscription and service limits, quotas, and constraints](../azure-subscription-service-limits.md) (Azure-abonnement en servicelimieten, -quota en -beperkingen) voor meer informatie.
>
>

## <a name="automatic-scaling-formulas"></a>Automatische schaling formules
Een formule voor automatisch schalen is een string-waarde die u definieert die een of meer instructies bevat. De formule voor automatisch schalen is toegewezen aan een pool [autoScaleFormula] [ rest_autoscaleformula] element (Batch REST) of [CloudPool.AutoScaleFormula] [ net_cloudpool_autoscaleformula] eigenschap (Batch .NET). De Batch-service gebruikt de formule om te bepalen van het doelaantal rekenknooppunten in de groep voor het volgende interval wordt verwerkt. De formule tekenreeks mag niet meer dan 8 KB, kan maximaal 100 instructies die worden gescheiden door puntkomma's en kunnen regeleinden en -opmerkingen bevatten bevatten.

U kunt automatische schaling formules beschouwen als een Batch automatisch schalen 'taal'. Formule-instructies zijn gratis ingedeelde expressies die zowel de service gedefinieerde variabelen (variabelen die zijn gedefinieerd door de Batch-service) en de gebruiker gedefinieerde variabelen (variabelen die u definieert) kunnen opnemen. Ze kunnen verschillende bewerkingen op deze waarden uitvoeren met behulp van de ingebouwde typen, operators en functies. Bijvoorbeeld, een instructie kan duren voordat de volgende notatie:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Formules bevatten doorgaans meerdere instructies die bewerkingen uitvoeren op waarden die zijn verkregen in de vorige instructies. Bijvoorbeeld, eerst we een waarde voor aanvragen `variable1`, vervolgens doorgeven aan een functie voor het vullen van `variable2`:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Deze instructies bevatten in de formule voor automatisch schalen die moet worden uitgevoerd op een doel-nummer van rekenknooppunten. Toegewezen knooppunten en knooppunten met lage prioriteit hebben hun eigen doelinstellingen zodat u kunt een doel voor elk type knooppunt definiëren. Een formule voor automatisch schalen kan een doelwaarde voor speciale knooppunten of een doelwaarde voor prioriteit Laag knooppunten bevatten.

Het doelaantal knooppunten is mogelijk hoger, kleine of hetzelfde als het huidige aantal knooppunten van dat type in de groep. Batch evalueert formule voor een pool automatisch schalen met een bepaald interval (Zie [automatisch vergroten / verkleinen intervallen](#automatic-scaling-interval)). Batch Hiermee past u het nummer van het doel van elk type knooppunt in de pool aan het getal waarmee de formule voor automatisch schalen op het moment van evaluatie.

### <a name="sample-autoscale-formula"></a>Formule voor automatisch schalen

Hier volgt een voorbeeld van een formule voor automatisch schalen die u voor de meeste scenario's kan worden aangepast. De variabelen `startingNumberOfVMs` en `maxNumberofVMs` in het voorbeeld formule kan worden aangepast aan uw behoeften. Deze formule schaalt toegewezen knooppunten, maar kan worden gewijzigd om te passen op prioriteit Laag knooppunten ook schaal. 

```
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicatedNodes=min(maxNumberofVMs, pendingTaskSamples);
```

Met deze formule voor automatisch schalen is in eerste instantie de toepassingen gemaakt met één VM. De `$PendingTasks` metriek definieert het aantal taken die uitgevoerd of in de wachtrij zijn. De formule Hiermee zoekt u het gemiddelde aantal in behandeling zijnde taken in de laatste 180 seconden en wordt de `$TargetDedicatedNodes` variabele dienovereenkomstig. De formule zorgt ervoor dat het doelaantal knooppunten toegewezen nooit meer dan 25 virtuele machines. Als nieuwe taken worden verzonden, wordt de groep automatisch groeit. Als de taken zijn voltooid, virtuele machines gratis één voor één en worden de formule voor automatisch schalen verkleint de pool.

## <a name="variables"></a>Variabelen
U kunt beide **service gedefinieerde** en **gebruiker gedefinieerde** variabelen in formules automatisch schalen. De service gedefinieerde variabelen zijn ingebouwd in de Batch-service. Sommige service gedefinieerde variabelen zijn alleen-lezen en sommige alleen-lezen zijn. Gebruiker gedefinieerde variabelen zijn variabelen die u definieert. In de voorbeeldformule weergegeven in de vorige sectie `$TargetDedicatedNodes` en `$PendingTasks` service gedefinieerde variabelen zijn. Variabelen `startingNumberOfVMs` en `maxNumberofVMs` zijn van de gebruiker gedefinieerde variabelen.

> [!NOTE]
> Service gedefinieerde variabelen worden altijd voorafgegaan door een dollarteken ($). Voor de gebruiker gedefinieerde variabelen is het dollarteken optioneel.
>
>

De volgende tabellen geven alleen-lezen en alleen-lezen-variabelen die zijn gedefinieerd door de Batch-service.

U kunt ophalen en stel de waarden van deze service gedefinieerde variabelen voor het beheren van het aantal rekenknooppunten in een groep:

| Lees-/ service gedefinieerde variabelen | Beschrijving |
| --- | --- |
| $TargetDedicatedNodes |Het doelaantal toegewezen rekenknooppunten voor de pool. Het aantal toegewezen knooppunten is opgegeven als een doel omdat het gewenste aantal knooppunten niet altijd door een groep bereiken mogelijk. Bijvoorbeeld, als het doelaantal toegewezen knooppunten is gewijzigd door een evaluatie met automatisch schalen voordat de groep het eerste doel is bereikt, klikt u vervolgens de groep bereikt mogelijk niet het doel. <br /><br /> Een groep in een account dat is gemaakt met de configuratie van de Batch-Service kan het doel niet bereiken als het doel een Batch-account knooppunt of core-quota overschrijdt. Een groep in een account dat is gemaakt met de configuratie van het abonnement van de gebruiker kan het doel niet bereiken als het doel het quotum voor gedeelde kernen voor het abonnement overschrijdt.|
| $TargetLowPriorityNodes |Het doelaantal prioriteit Laag rekenknooppunten voor de pool. Het aantal knooppunten met lage prioriteit is opgegeven als een doel omdat het gewenste aantal knooppunten niet altijd door een groep bereiken mogelijk. Bijvoorbeeld, als het doelaantal knooppunten prioriteit laag is gewijzigd door een evaluatie met automatisch schalen voordat de groep het eerste doel is bereikt, klikt u vervolgens de groep bereikt mogelijk niet het doel. Een pool kan ook niet het doel bereiken als het doel een Batch-account knooppunt of core-quota overschrijdt. <br /><br /> Zie voor meer informatie op prioriteit Laag rekenknooppunten [met lage prioriteit virtuele machines, met Batch (Preview)](batch-low-pri-vms.md). |
| $NodeDeallocationOption |De actie die optreedt wanneer de rekenknooppunten worden verwijderd uit een groep. Mogelijke waarden zijn:<ul><li>**requeue**--taken onmiddellijk beëindigd en weer in de wachtrij plaatst, zodat ze opnieuw worden gepland.<li>**beëindigen**--taken onmiddellijk beëindigd en worden deze verwijderd uit de wachtrij.<li>**taskcompletion**--wacht voor momenteel wordt uitgevoerd voor het voltooien van taken en vervolgens verwijdert u het knooppunt uit de groep.<li>**retaineddata**--wordt gewacht op alle lokale taak bewaard gegevens op het knooppunt dat moet worden opgeruimd voordat het knooppunt wordt verwijderd uit de groep.</ul> |

U kunt de waarde van deze service gedefinieerde variabelen moet aanbrengen op basis van de metrische gegevens van de Batch-service krijgen:

| Alleen-lezen service gedefinieerde variabelen | Beschrijving |
| --- | --- |
| $CPUPercent |Het gemiddelde percentage van CPU-gebruik. |
| $WallClockSeconds |Het aantal seconden verbruikt. |
| $MemoryBytes |Het gemiddelde aantal megabytes gebruikt. |
| $DiskBytes |Het gemiddelde aantal gigabyte gebruikt op de lokale schijven. |
| $DiskReadBytes |Het aantal bytes dat is gelezen. |
| $DiskWriteBytes |Het aantal geschreven bytes. |
| $DiskReadOps |Het aantal gelezen schijfbewerkingen die worden uitgevoerd. |
| $DiskWriteOps |Het aantal schrijfbewerkingen schijf uitgevoerd. |
| $NetworkInBytes |Het aantal inkomende bytes. |
| $NetworkOutBytes |Het aantal uitgaande bytes. |
| $SampleNodeCount |Het aantal rekenknooppunten. |
| $ActiveTasks |Het aantal taken die kan worden uitgevoerd, maar nog niet zijn uitgevoerd. Het aantal $ActiveTasks bevat alle taken die zijn opgenomen in de actieve status heeft en waarvan afhankelijkheden is voldaan. Alle taken die zich in de actieve status maar waarvan afhankelijkheden niet is voldaan, worden uitgesloten van het aantal $ActiveTasks.|
| $RunningTasks |Het aantal taken actief is. |
| $PendingTasks |De som van $ActiveTasks en $RunningTasks. |
| $SucceededTasks |Het aantal taken die voltooid is. |
| $FailedTasks |Het aantal taken dat is mislukt. |
| $CurrentDedicatedNodes |Het huidige aantal toegewezen rekenknooppunten. |
| $CurrentLowPriorityNodes |Het huidige aantal prioriteit Laag rekenknooppunten, met inbegrip van alle knooppunten die verschoven zijn. |
| $PreemptedNodeCount | Het aantal knooppunten in de groep die zich in een status verschoven. |

> [!TIP]
> De alleen-lezen, service gedefinieerde variabelen die worden weergegeven in de vorige tabel zijn *objecten* die verschillende methoden voor toegang tot gegevens die zijn gekoppeld aan elk bieden. Zie voor meer informatie [verkrijgen voorbeeldgegevens](#getsampledata) verderop in dit artikel.
>
>

## <a name="types"></a>Typen
Deze typen worden ondersteund in een formule:

* dubbele
* doubleVec
* doubleVecList
* Tekenreeks
* tijdstempel--tijdstempel is een samengestelde structuur met de volgende leden:

  * jaar
  * maand (1-12)
  * dag (1-31)
  * werkdag (in de indeling van het nummer; bijvoorbeeld: 1 voor maandag)
  * uur (in 24-uurs notatie, bijvoorbeeld: 13 betekent 1 uur)
  * minuten (00-59)
  * tweede (00-59)
* TimeInterval

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
| dubbele *operator* dubbele |+, -, *, / |dubbele |
| dubbele *operator* timeinterval |* |TimeInterval |
| doubleVec *operator* dubbele |+, -, *, / |doubleVec |
| doubleVec *operator* doubleVec |+, -, *, / |doubleVec |
| TimeInterval *operator* dubbele |*, / |TimeInterval |
| TimeInterval *operator* timeinterval |+, - |TimeInterval |
| TimeInterval *operator* tijdstempel |+ |tijdstempel |
| tijdstempel *operator* timeinterval |+ |tijdstempel |
| tijdstempel *operator* tijdstempel |- |TimeInterval |
| *operator*dubbele |-, ! |dubbele |
| *operator*timeinterval |- |TimeInterval |
| dubbele *operator* dubbele |<, <=, ==, >=, >, != |dubbele |
| tekenreeks *operator* tekenreeks |<, <=, ==, >=, >, != |dubbele |
| tijdstempel *operator* tijdstempel |<, <=, ==, >=, >, != |dubbele |
| TimeInterval *operator* timeinterval |<, <=, ==, >=, >, != |dubbele |
| dubbele *operator* dubbele |&&, &#124;&#124; |dubbele |

Bij het testen van een Double-waarde met een ternaire operator (`double ? statement1 : statement2`), niet nul is **true**, en nul is **false**.

## <a name="functions"></a>Functies
Deze vooraf gedefinieerde **functies** zijn beschikbaar voor gebruik bij het definiëren van een formule voor automatisch schalen.

| Functie | Retourtype | Beschrijving |
| --- | --- | --- |
| Avg(doubleVecList) |dubbele |Retourneert de gemiddelde waarde voor alle waarden in de doubleVecList. |
| Len(doubleVecList) |dubbele |Retourneert de lengte van de vector die wordt gemaakt van de doubleVecList. |
| LG(Double) |dubbele |Retourneert het logboek grondtal 2 van de dubbele waarde. |
| LG(doubleVecList) |doubleVec |Retourneert het component-wise logboek grondtal 2 van de doubleVecList. Een vec(double) moet expliciet voor de parameter worden doorgegeven. Anders wordt wordt de versie van de dubbele lg(double) verondersteld. |
| ln(Double) |dubbele |Retourneert het natuurlijke logboek van de dubbele waarde. |
| ln(doubleVecList) |doubleVec |Retourneert het component-wise logboek grondtal 2 van de doubleVecList. Een vec(double) moet expliciet voor de parameter worden doorgegeven. Anders wordt wordt de versie van de dubbele lg(double) verondersteld. |
| log(Double) |dubbele |Retourneert het logboek grondtal 10 van de dubbele waarde. |
| log(doubleVecList) |doubleVec |Retourneert het component-wise logboek grondtal 10 van de doubleVecList. Een vec(double) moet expliciet voor de één dubbele parameter worden doorgegeven. Anders wordt wordt de versie van de dubbele log(double) verondersteld. |
| Max(doubleVecList) |dubbele |Retourneert de maximumwaarde in de doubleVecList. |
| min(doubleVecList) |dubbele |Retourneert de minimumwaarde in het doubleVecList. |
| norm(doubleVecList) |dubbele |Retourneert de twee-norm van de vector die wordt gemaakt van de doubleVecList. |
| percentiel (v doubleVec, dubbele p) |dubbele |Retourneert het percentiel-element van de vector v. |
| ASELECT() |dubbele |Retourneert een willekeurige waarde tussen 0,0 en 1,0 liggen. |
| Range(doubleVecList) |dubbele |Retourneert het verschil tussen de min en max-waarden in de doubleVecList. |
| Std(doubleVecList) |dubbele |Retourneert de standaardafwijking van voorbeeld van de waarden in de doubleVecList. |
| Stop() | |Evaluatie van de expressie voor automatisch schalen stopt. |
| SUM(doubleVecList) |dubbele |Retourneert de som van alle onderdelen van de doubleVecList. |
| tijd (string, dateTime = "") |tijdstempel |Retourneert de tijdstempel van de huidige tijd als er geen parameters worden doorgegeven of het tijdstempel van de datum/tijd-tekenreeks als deze wordt doorgegeven. Ondersteunde datum-/ tijdindelingen zijn W3C-DTF- en RFC 1123. |
| waarde opnemen (v doubleVec, dubbele i) |dubbele |Retourneert de waarde van het element dat is op locatie i in vector v, met een startIndex gelijk is aan nul. |

Enkele van de functies die worden beschreven in de vorige tabel kan een lijst accepteren als een argument. De lijst met door komma's gescheiden is een combinatie van *dubbele* en *doubleVec*. Bijvoorbeeld:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

De *doubleVecList* wordt omgezet in een enkel *doubleVec* voordat de evaluatie. Bijvoorbeeld, als `v = [1,2,3]`, klikt u vervolgens het aanroepen van `avg(v)` is gelijk aan het aanroepen van `avg(1,2,3)`. Het aanroepen van `avg(v, 7)` is gelijk aan het aanroepen van `avg(1,2,3,7)`.

## <a name="getsampledata"></a>Voorbeeldgegevens downloaden
Automatisch schalen formules fungeren op metrische gegevens (voorbeelden) die wordt geleverd door de Batch-service. Een formule groeit of krimpt poolgrootte op basis van de waarden die wordt verkregen van de service. De service gedefinieerde variabelen die zijn beschreven zijn eerder objecten waarmee u verschillende methoden voor toegang tot gegevens die is gekoppeld aan dit object. De volgende expressie ziet u bijvoorbeeld een aanvraag voor het ophalen van de laatste vijf minuten van CPU-gebruik:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| Methode | Beschrijving |
| --- | --- |
| GetSample() |De `GetSample()` methode retourneert een vector van voorbeelden van gegevens.<br/><br/>Een voorbeeld is 30 seconden metrische gegevens. Voorbeelden zijn met andere woorden, elke 30 seconden opgehaald. Maar zoals hieronder aangegeven, er is een vertraging tussen wanneer een voorbeeld van een worden verzameld en wanneer deze beschikbaar is om een formule. Als zodanig kunnen niet alle voorbeelden voor een bepaalde periode niet beschikbaar voor evaluatie van een formule.<ul><li>`doubleVec GetSample(double count)`<br/>Hiermee geeft u het aantal steekproeven te halen van de meest recente voorbeelden die zijn verzameld.<br/><br/>`GetSample(1)`retourneert het laatste voorbeeld beschikbaar. Metrische gegevens zoals `$CPUPercent`, maar dit mag niet worden gebruikt omdat het niet mogelijk om te weten *wanneer* het voorbeeld is verzameld. Kan het zijn recente of vanwege problemen met het systeem, kan het veel ouder zijn. Het is beter in dergelijke gevallen te gebruiken een tijdsinterval zoals hieronder wordt weergegeven.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Hiermee geeft u een tijdsbestek voor het verzamelen van voorbeeldgegevens. Eventueel specificeert het ook het percentage van de voorbeelden die beschikbaar zijn in het aangevraagde tijdsbestek.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)`Voorbeelden van 20 zou worden geretourneerd als alle voorbeelden voor de afgelopen 10 minuten in de geschiedenis CPUPercent aanwezig zijn. Als de laatste minuut van de geschiedenis niet beschikbaar was, echter zou alleen 18 voorbeelden worden geretourneerd. In dit geval:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)`mislukken omdat slechts 90 procent van de voorbeelden beschikbaar zijn.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)`zou slagen.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Hiermee geeft u een tijdsbestek voor het verzamelen van gegevens met een begintijd en eindtijd.<br/><br/>Zoals eerder vermeld, is er een vertraging tussen wanneer een voorbeeld van een worden verzameld en wanneer deze beschikbaar is om een formule. Houd rekening met deze vertraging wanneer u de `GetSample` methode. Zie `GetSamplePercent` hieronder. |
| GetSamplePeriod() |Retourneert de periode van voorbeelden die zijn uitgevoerd in een historische voorbeeldgegevensset. |
| Count() |Retourneert het totale aantal steekproeven in de geschiedenis van de metrische gegevens. |
| HistoryBeginTime() |Retourneert de tijdstempel van de oudste voorbeeld van de beschikbare gegevens voor de metriek. |
| GetSamplePercent() |Retourneert het percentage van de voorbeelden die beschikbaar voor een bepaald tijdsinterval zijn. Bijvoorbeeld:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Omdat de `GetSample` methode mislukt als het percentage van de voorbeelden geretourneerd is kleiner dan de `samplePercent` opgegeven, kunt u de `GetSamplePercent` methode om eerst te controleren. Vervolgens kunt u een alternatieve actie uitvoeren onvoldoende voorbeelden aanwezig zijn, zonder het stoppen van de automatische schaling evaluatie. |

### <a name="samples-sample-percentage-and-the-getsample-method"></a>Voorbeelden, voorbeeld percentage, en de *GetSample()* methode
De hoofdbewerking van een formule voor automatisch schalen is het verkrijgen van metrische gegevens voor taken en resources en past u de poolgrootte op basis van die gegevens. Hierdoor is het belangrijk dat u een duidelijk beeld hebben van de wisselwerking tussen de formules automatisch schalen met metrische gegevens (voorbeelden).

**Voorbeelden**

De Batch-service wordt regelmatig duurt voorbeelden van taken en resources metrische gegevens en beschikbaar maken voor automatisch schalen formules. Deze voorbeelden worden elke 30 seconden geregistreerd door de Batch-service. Er is echter meestal een vertraging tussen wanneer deze voorbeelden zijn vastgelegd, en wanneer ze beschikbaar worden gesteld op (en kunnen worden gelezen door) automatisch schalen formules. Bovendien, als gevolg van verschillende factoren zoals netwerk- of andere problemen infrastructuur steekproeven mogelijk niet geregistreerd voor een bepaalde interval.

**Percentage van de steekproef**

Wanneer `samplePercent` wordt doorgegeven aan de `GetSample()` methode of de `GetSamplePercent()` methode wordt aangeroepen, _procent_ verwijst naar een vergelijking tussen het totale aantal voorbeelden die zijn vastgelegd door de Batch-service en het aantal steekproeven die beschikbaar voor de formule voor automatisch schalen zijn.

We bekijken een timespan 10 minuten als voorbeeld. Omdat voorbeelden worden elke 30 seconden binnen een timespan 10 minuten vastgelegd, zou het maximum aantal voorbeelden die zijn vastgelegd door de Batch 20 steekproeven (2 per minuut) zijn. Echter, vanwege de intrinsieke latentie van het mechanisme voor rapportage en andere problemen in Azure, kunnen er alleen 15 voorbeelden die beschikbaar voor de formule voor automatisch schalen om te lezen zijn. Dus bijvoorbeeld voor deze periode 10 minuten kunnen alleen 75% van het totale aantal steekproeven vastgelegd mogelijk beschikbaar aan uw formule.

**GetSample() en voorbeeld-bereiken**

Automatisch schalen formules gaat groeit en uw pools verkleinen &mdash; knooppunten toe te voegen of te verwijderen van knooppunten. Omdat knooppunten geld kosten, die u wilt zorgen dat uw formules een intelligente methode van de analyse op basis van voldoende gegevens gebruikt. Daarom raden we u een trending-type-analyse gebruiken in uw formules. Dit type groeit en uw pools op basis van een bereik van verzamelde steekproeven verkleind.

Gebruiken om dit te doen `GetSample(interval look-back start, interval look-back end)` te retourneren van een vector van voorbeelden:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Wanneer de bovenstaande regel wordt geëvalueerd door de Batch, wordt een reeks voorbeelden als vector van waarden. Bijvoorbeeld:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Zodra u de vector van voorbeelden hebt verzameld, vervolgens kunt u functies zoals `min()`, `max()`, en `avg()` worden afgeleid van zinvolle waarden van het verzamelde bereik.

Voor extra beveiliging kunt u een formule-evaluatie mislukken als er minder dan een bepaald percentage van het voorbeeld is beschikbaar voor een bepaalde periode afdwingen. Wanneer u een formule-evaluatie mislukken afdwingt, vertelt u Batch niet meer verdere evaluatie van de formule als het opgegeven percentage van de voorbeelden niet beschikbaar is. In dit geval is niet gewijzigd in de poolgrootte. Geef een vereiste percentage van steekproeven voor de evaluatie te kunnen uitvoeren door dit opgeven als de derde parameter `GetSample()`. Hier wordt een vereiste van 75 procent van de voorbeelden is opgegeven:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

Omdat er een vertraging optreden in de steekproef beschikbaarheid, is het belangrijk dat u altijd een tijdsperiode opgeven met een uiterlijk-back-begintijd die ouder is dan een minuut. Het duurt ongeveer een minuut voor voorbeelden worden doorgegeven via het systeem, dus-voorbeelden in het bereik `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` mogelijk niet beschikbaar. Nogmaals, kunt u de parameter percentage van `GetSample()` om af te dwingen een bepaalde voorbeeld percentage vereiste.

> [!IMPORTANT]
> We **aangeraden** die u hebt **voorkomen relying *alleen* op `GetSample(1)` in formules automatisch schalen**. Dit komt doordat `GetSample(1)` in wezen aangegeven dat u moet de Batch-service "Geef me de laatste steekproef u hebt, ongeacht hoe lang geleden u opgehaald." Omdat dit slechts een enkele voorbeeld is en kan een oudere voorbeeld, mogelijk vertegenwoordiger van de grotere afbeelding van recente taak of resource staat niet meer. Als u gebruik maakt van `GetSample(1)`, zorg ervoor dat het deel uitmaakt van een grotere instructie en niet de enige gegevenspunt dat afhankelijk is van de formule.
>
>

## <a name="metrics"></a>Metrische gegevens
U kunt zowel de bron- als de taak metrische gegevens gebruiken wanneer u bij het definiëren van een formule. U aanpassen het doelaantal toegewezen knooppunten in de groep op basis van de metrische gegevens die u ophalen en evalueren. Zie de [variabelen](#variables) sectie hierboven voor meer informatie over elke metriek.

<table>
  <tr>
    <th>Gegevens</th>
    <th>Beschrijving</th>
  </tr>
  <tr>
    <td><b>Resource</b></td>
    <td><p>Metrische gegevens voor resources zijn gebaseerd op de CPU, de bandbreedte, het geheugengebruik van rekenknooppunten en het aantal knooppunten.</p>
        <p> Deze service gedefinieerde variabelen zijn handig voor het uitvoeren van aanpassingen op basis van het aantal knooppunten:</p>
    <p><ul>
            <li>$TargetDedicatedNodes</li>
            <li>$TargetLowPriorityNodes</li>
            <li>$CurrentDedicatedNodes</li>
            <li>$CurrentLowPriorityNodes</li>
            <li>$preemptedNodeCount</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Deze service gedefinieerde variabelen zijn nuttig voor het uitvoeren van aanpassingen op basis van knooppunt Resourcegebruik:</p>
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
    <td><p>Taak metrische gegevens op basis van de status van taken, zoals actief, in behandeling, en voltooid. De volgende service gedefinieerde variabelen zijn handig voor groepsgrootte aanpassingen op basis van de taak metrische gegevens:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Schrijven van een formule voor automatisch schalen
U een formule voor automatisch schalen verder door de instructies die gebruikmaken van de bovenstaande onderdelen vormen en vervolgens deze instructies naar een volledige formule combineren. In deze sectie maken we een formule voor het automatisch schalen van voorbeeld die een echte vergroten/verkleinen beslissingen kunt uitvoeren.

Eerst gaan we de vereisten voor onze nieuwe formule voor automatisch schalen worden gedefinieerd. De formule moet het volgende doen:

1. Vergroot het doelaantal knooppunten in een pool speciale berekenings als het CPU-gebruik is hoog.
2. Het doelaantal knooppunten in een pool speciale berekenings afnemen als CPU-gebruik laag is.
3. Altijd het maximum aantal toegewezen knooppunten tot 400 beperken.

Voor een verhoging van het aantal knooppunten tijdens hoog CPU-gebruik, definieert u de instructie die de gebruiker gedefinieerde variabele vult (`$totalDedicatedNodes`) met een waarde die is 110 procent van het huidige doelaantal knooppunten toegewezen, maar alleen als het minimale gemiddelde CPU-gebruik tijdens de afgelopen tien minuten is hoger dan 70 procent. Gebruik anders de waarde voor het huidige aantal toegewezen knooppunten.

```
$totalDedicatedNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicatedNodes * 1.1) : $CurrentDedicatedNodes;
```

Naar *verkleinen* het aantal toegewezen knooppunten tijdens het lage CPU-gebruik de volgende instructie in onze formule ingesteld dezelfde `$totalDedicatedNodes` variabele à 90 procent van het huidige doelaantal toegewezen knooppunten als het gemiddelde CPU-gebruik in de afgelopen 60 minuten onder 20 procent is. Anders gebruikt u de huidige waarde van `$totalDedicatedNodes` die we in de bovenstaande instructie ingevuld.

```
$totalDedicatedNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicatedNodes * 0.9) : $totalDedicatedNodes;
```

Nu het doelaantal knooppunten tot een maximum van 400 speciale berekenings beperken:

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

## <a name="create-an-autoscale-enabled-pool-with-net"></a>Een pool automatisch schalen ingeschakeld maken met .NET

Maakt een groep met automatisch schalen die zijn ingeschakeld in .NET, de volgende stappen uit:

1. Maken van de groep met [BatchClient.PoolOperations.CreatePool](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.createpool).
2. Stel de [CloudPool.AutoScaleEnabled](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleenabled) eigenschap `true`.
3. Stel de [CloudPool.AutoScaleFormula](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleformula) eigenschap met de formule voor automatisch schalen.
4. (Optioneel) Stel de [CloudPool.AutoScaleEvaluationInterval](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval) eigenschap (de standaardwaarde is 15 minuten).
5. De groep met doorvoeren [CloudPool.Commit](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commit) of [CommitAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.commitasync).

Het volgende codefragment maakt een groep voor automatisch schalen die zijn ingeschakeld in .NET. De formule voor automatisch schalen van de pool wordt het doelaantal toegewezen knooppunten tot en met 5 op maandag 1 op om de dag van de week. De [automatisch vergroten/verkleinen interval](#automatic-scaling-interval) is ingesteld op 30 minuten. In dit document en de andere C# codefragmenten in dit artikel `myBatchClient` is een goed geïnitialiseerd exemplaar van de [BatchClient] [ net_batchclient] klasse.

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool(
                    poolId: "mypool",
                    virtualMachineSize: "small", // single-core, 1.75 GB memory, 225 GB disk
                    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));    
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
await pool.CommitAsync();
```

> [!IMPORTANT]
> Wanneer u een groep automatisch schalen ingeschakeld maakt, geef de _targetDedicatedComputeNodes_ parameter of de _targetLowPriorityComputeNodes_ parameter bij het aanroepen van **CreatePool**. Geef in plaats daarvan de **AutoScaleEnabled** en **AutoScaleFormula** eigenschappen in de groep. De waarden voor deze eigenschappen bepalen het doelaantal van elk type knooppunt. Ook aan handmatig een automatisch schalen ingeschakeld formaat toepassingen (bijvoorbeeld met [BatchClient.PoolOperations.ResizePoolAsync][net_poolops_resizepoolasync]), eerste **uitschakelen** automatisch schalen aan de groep, klikt u vervolgens het formaat.
>
>

Naast Batch .NET, kunt u een van de andere [Batch-SDK's](batch-apis-tools.md#azure-accounts-for-batch-development), [Batch REST](https://docs.microsoft.com/rest/api/batchservice/), [Batch PowerShell-cmdlets](batch-powershell-cmdlets-get-started.md), en de [Batch CLI](batch-cli-get-started.md) automatisch schalen configureren.


### <a name="automatic-scaling-interval"></a>Interval voor automatisch schalen
Standaard aangepast de Batch-service een groepsgrootte volgens de formule voor automatisch schalen om de 15 minuten. Dit interval kan worden geconfigureerd met behulp van de eigenschappen van de volgende toepassingen:

* [CloudPool.AutoScaleEvaluationInterval] [ net_cloudpool_autoscaleevalinterval] (Batch .NET)
* [autoScaleEvaluationInterval] [ rest_autoscaleinterval] (REST-API)

Het minimale interval is vijf minuten en de maximumwaarde is 168 uur. Als een interval buiten dit bereik is opgegeven, geeft de Batch-service een fout Ongeldige aanvraag (400).

> [!NOTE]
> Automatisch schalen is momenteel niet bedoeld om te reageren op wijzigingen in minder dan een minuut, maar in plaats daarvan is bedoeld voor het aanpassen van de grootte van uw groep geleidelijk als u een werkbelasting uitvoeren.
>
>

## <a name="enable-autoscaling-on-an-existing-pool"></a>Automatisch schalen op een bestaande toepassingen inschakelen

Elke Batch-SDK biedt een manier om in te schakelen automatisch schalen. Bijvoorbeeld:

* [BatchClient.PoolOperations.EnableAutoScaleAsync] [ net_enableautoscaleasync] (Batch .NET)
* [Inschakelen van automatisch schalen op een pool] [ rest_enableautoscale] (REST-API)

Wanneer u automatisch schalen op een bestaande pool inschakelt, houd er rekening mee houden de volgende punten:

* Als automatische schaling is momenteel uitgeschakeld op de groep wanneer u de aanvraag voor het inschakelen van automatisch schalen verzendt, moet u een formule voor automatisch schalen die geldige opgeven wanneer u de aanvraag verzendt. U kunt optioneel een evaluatie-interval voor automatisch schalen opgeven. Als u een interval niet opgeeft, wordt de standaardwaarde van 15 minuten gebruikt.
* Als voor automatisch schalen die momenteel is ingeschakeld op de groep, kunt u een formule voor automatisch schalen, een evaluatie-interval of beide opgeven. U moet ten minste één van deze eigenschappen opgeven.

  * Als u een nieuwe evaluatie interval voor automatisch schalen opgeeft, wordt de bestaande evaluatieplanning gestopt en een nieuw schema wordt gestart. Begintijd van de nieuwe planning is de tijd waarop de aanvraag voor het inschakelen van automatisch schalen is uitgegeven.
  * Als u de formule voor automatisch schalen of evaluatie-interval, de Batch-service weglaat blijft de huidige waarde van de instelling te gebruiken.

> [!NOTE]
> Als u opgegeven waarden voor de *targetDedicatedComputeNodes* of *targetLowPriorityComputeNodes* parameters van de **CreatePool** methode wanneer u de groep hebt gemaakt in .NET of voor de vergelijkbare parameters in een andere taal, worden deze waarden genegeerd als de formule voor automatisch vergroten/verkleinen wordt geëvalueerd.
>
>

Dit C#-codefragment maakt gebruik van de [Batch .NET] [ net_api] bibliotheek automatisch schalen op een bestaande pool inschakelen:

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicatedNodes = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Bijwerken van een formule voor automatisch schalen

Voor het bijwerken van de formule op een bestaande pool voor automatisch schalen is ingeschakeld, moet u de bewerking zodat automatisch schalen opnieuw met de nieuwe formule aanroepen. Bijvoorbeeld, als u automatisch schalen is al ingeschakeld op `myexistingpool` wanneer de volgende .NET-code wordt uitgevoerd, de formule voor automatisch schalen is vervangen door de inhoud van `myNewFormula`.

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a>Het interval voor automatisch schalen

Voor het bijwerken van het interval voor automatisch schalen evaluatie van een bestaande pool voor automatisch schalen is ingeschakeld, moet u de bewerking zodat automatisch schalen opnieuw met het nieuwe interval aanroepen. Als u bijvoorbeeld het interval van de evaluatie voor automatisch schalen instellen en 60 minuten voor een pool die al automatisch schalen in .NET is ingeschakeld:

```csharp
await myBatchClient.PoolOperations.EnableAutoScaleAsync(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Evalueren van een formule voor automatisch schalen

U kunt een formule evalueren voordat u deze toepast op een groep. Op deze manier kunt u de formule om te zien hoe de instructies evalueren voordat u de formule plaatsen in productie testen.

Om een formule voor automatisch schalen, moet u eerst automatisch schalen op de groep met een geldige formule inschakelen. Als u wilt testen een formule in een groep die nog geen automatisch schalen die zijn ingeschakeld, gebruikt u de formule van één regel `$TargetDedicatedNodes = 0` wanneer u automatisch schalen voor het eerst inschakelt. Gebruik vervolgens een van de volgende evalueren van de formule die u wilt testen:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscale) of [EvaluateAutoScaleAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.evaluateautoscaleasync)

    Deze methoden Batch .NET vereist de ID van een bestaande pool en een tekenreeks met de formule voor automatisch schalen om te evalueren.

* [Evalueren van een formule voor automatisch schalen](https://docs.microsoft.com/rest/api/batchservice/evaluate-an-automatic-scaling-formula)

    In deze REST-API-aanvraag opgeven dat de pool-ID in de URI en de formule voor automatisch schalen in de *autoScaleFormula* element van de aanvraagtekst. Het antwoord van de bewerking bevat informatie over de fout die mogelijk zijn gerelateerd aan de formule.

In deze [Batch .NET] [ net_api] codefragment, we een formule voor automatisch schalen evalueren. Als de groep geen automatisch schalen die zijn ingeschakeld, wordt dit inschakelen eerst.

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
        autoscaleFormula: "$TargetDedicatedNodes = {pool.CurrentDedicatedNodes};",
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

Geslaagde evaluatie van de formule weergegeven in dit codefragment levert resultaten vergelijkbaar met:

```
AutoScaleRun.Results:
    $TargetDedicatedNodes=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Informatie ophalen over automatisch schalen die wordt uitgevoerd

Om ervoor te zorgen dat de formule wordt uitgevoerd zoals verwacht, is het raadzaam dat u de resultaten van het automatisch schalen wordt uitgevoerd waarmee Batch wordt uitgevoerd op uw pool periodiek controleren. Dus ophalen (of vernieuwen) een verwijzing naar de groep, en bekijk de eigenschappen van de laatste automatisch schalen die wordt uitgevoerd.

In de Batch .NET, de [CloudPool.AutoScaleRun](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.cloudpool.autoscalerun) eigenschap heeft verschillende eigenschappen die informatie geven over de meest recente automatische schaling uitgevoerd worden uitgevoerd op de groep:

* [AutoScaleRun.Timestamp](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.timestamp)
* [AutoScaleRun.Results](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.results)
* [AutoScaleRun.Error](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.autoscalerun.error)

In de REST API, de [informatie ophalen over een pool](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool) aanvraag retourneert informatie over de groep, waaronder de meest recente automatische schaling uitgevoerd van de informatie de [autoScaleRun](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-pool#bk_autrun) eigenschap.

De volgende C#-codefragment maakt gebruik van de Batch .NET-bibliotheek om informatie over de laatste automatisch schalen die worden uitgevoerd op de groep van toepassingen te drukken _myPool_:

```csharp
await Cloud pool = myBatchClient.PoolOperations.GetPoolAsync("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Voorbeeld van uitvoer van het vorige codefragment:

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
Bekijken we enkele formules die verschillende manieren om aan te passen, de hoeveelheid rekenresources in een groep weergeven.

### <a name="example-1-time-based-adjustment"></a>Voorbeeld 1: Op basis van tijd aanpassing
Stel dat u wilt aanpassen van de grootte van de groep op basis van de dag van de week en het tijdstip van de dag. In dit voorbeeld laat zien hoe vergroten of verkleinen het aantal knooppunten in de pool dienovereenkomstig.

De formule verkrijgt eerst de huidige tijd. Als dit een weekdag (1-5 is) en binnen werkuren (8 AM tot 6 uur), wordt de grootte van de doel-groep is ingesteld op 20 knooppunten. Anders wordt ingesteld op 10 knooppunten.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicatedNodes = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>Voorbeeld 2: Taakgebaseerde aanpassing
In dit voorbeeld is de poolgrootte aangepast op basis van het aantal taken in de wachtrij. Alle opmerkingen en regeleinden zijn acceptabel in formule tekenreeksen.

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

### <a name="example-3-accounting-for-parallel-tasks"></a>Voorbeeld 3: Accounting voor parallelle taken
Dit voorbeeld wordt de grootte van de groep op basis van het aantal taken. Deze formule ook rekening met de [MaxTasksPerComputeNode] [ net_maxtasks] waarde die is ingesteld voor de pool. Deze methode is handig in situaties waar [parallelle uitvoering van de taak](batch-parallel-node-tasks.md) is ingeschakeld op uw pool.

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

### <a name="example-4-setting-an-initial-pool-size"></a>Voorbeeld 4: De grootte van een eerste groep van toepassingen instellen
Dit voorbeeld toont een C#-codefragment met een formule voor automatisch schalen die de poolgrootte op een opgegeven aantal knooppunten voor een eerste periode instelt. Vervolgens wordt de grootte van de groep op basis van het nummer van het uitvoeren en actieve deze aangepast taken nadat de eerste periode is verstreken.

De formule in het volgende codefragment:

* Hiermee stelt u de initiële poolgrootte met vier knooppunten.
* Grootte van de groep niet aangepast tijdens de eerste 10 minuten van de levenscyclus van de groep.
* Na 10 minuten verkrijgt de max-waarde van het aantal actieve en actieve taken in de afgelopen 60 minuten.
  * Als beide waarden zijn 0 (waarmee wordt aangegeven dat er geen taken uitgevoerd of actief is in de afgelopen 60 minuten zijn), wordt de poolgrootte ingesteld op 0.
  * Als een waarde groter dan nul is, worden niet gewijzigd.

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
* [Azure Batch compute Resourcegebruik met gelijktijdige knooppunt taken maximaliseren](batch-parallel-node-tasks.md) bevat details over hoe u meerdere taken tegelijkertijd op de rekenknooppunten in uw groep uitvoeren kunt. Naast het automatisch schalen helpen deze functie mogelijk te verlagen duur van de taak voor sommige werkbelastingen, u geld besparen.
* Voor een andere efficiëntie booster, moet u ervoor zorgen dat uw Batch-toepassing query's in de Batch-service op optimale wijze. Zie [efficiënt query uitvoeren op de Azure Batch-service](batch-efficient-list-queries.md) voor informatie over het beperken van de hoeveelheid gegevens die over de kabel wanneer u de status van mogelijk duizenden rekenknooppunten of taken opvragen.

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
