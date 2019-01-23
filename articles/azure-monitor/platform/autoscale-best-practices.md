---
title: Aanbevolen procedures voor automatisch schalen
description: Patronen voor automatisch schalen in Azure voor Web-Apps, Virtual Machine Scale sets en Cloud Services
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/07/2017
ms.author: ancav
ms.subservice: autoscale
ms.openlocfilehash: 85a326c97ecf8476bdd802a718e082d0e5c7a89c
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54467351"
---
# <a name="best-practices-for-autoscale"></a>Aanbevolen procedures voor Automatisch schalen
Automatisch schalen van Azure Monitor is alleen bedoeld voor [Virtual Machine Scale Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloudservices](https://azure.microsoft.com/services/cloud-services/), [App Service - Web-Apps](https://azure.microsoft.com/services/app-service/web/), en [API Management-services](https://docs.microsoft.com/azure/api-management/api-management-key-concepts).

## <a name="autoscale-concepts"></a>Concepten voor automatisch schalen
* Een resource kan alleen hebben *één* instelling voor automatisch schalen
* Een instelling voor automatisch schalen kan een of meer profielen en elk profiel kunnen een of meer regels voor automatisch schalen hebben.
* Een instelling voor automatisch schalen kan worden geschaald exemplaren horizontaal, die is *uit* door het verhogen van de instanties en *in* door te verminderen van het aantal exemplaren.
  Een instelling voor automatisch schalen heeft een maximum, minimum en de standaardwaarde van exemplaren.
* Een taak voor automatisch schalen leest altijd de bijbehorende metrische gegevens om te schalen, controleren of deze de geconfigureerde drempelwaarde voor scale-out of schaal is gepasseerd. U kunt een lijst weergeven van metrische gegevens die voor automatisch schalen kan worden geschaald door op [Azure Monitor autoscaling common metrics](autoscale-common-metrics.md).
* Alle drempelwaarden worden berekend op het exemplaarniveau van een. Bijvoorbeeld, "uitschalen naar plus één exemplaar wanneer Gemiddeld CPU > 80% wanneer het aantal exemplaren is 2", scale-out betekent dat als de gemiddelde CPU voor alle instanties hoger is dan 80%.
* Alle fouten voor automatisch schalen worden geregistreerd in het activiteitenlogboek. U kunt vervolgens een [waarschuwing voor activiteitenlogboek](./../../azure-monitor/platform/activity-log-alerts.md) zodat u via e-mail, SMS of webhooks worden gewaarschuwd wanneer er sprake een fout voor automatisch schalen is.
* Op deze manier worden alle geslaagde schaalacties geplaatst in het activiteitenlogboek. U kunt vervolgens een waarschuwing voor activiteitenlogboek configureren zodat u via e-mail, SMS of webhooks worden gewaarschuwd wanneer er sprake is van een actie voltooid voor automatisch schalen. U kunt ook e-mailadres of webhook-meldingen om te worden geïnformeerd voor geslaagde schaalacties via op het tabblad meldingen van de instelling voor automatisch schalen configureren.

## <a name="autoscale-best-practices"></a>Aanbevolen procedures voor automatisch schalen
Gebruik de volgende aanbevolen procedures als u automatisch schalen gebruiken.

### <a name="ensure-the-maximum-and-minimum-values-are-different-and-have-an-adequate-margin-between-them"></a>Zorg ervoor dat de maximale en minimale waarden zijn verschillend en een voldoende marge daartussen hebben
Hebt u een instelling met minimale = 2, maximum = 2 en het huidige aantal exemplaren is 2, geen schaalactie kan zich voordoen. Houd een voldoende marge tussen het aantal maximale en minimale instanties, die ook meegerekend. Automatisch schalen geschaald altijd tussen deze limieten.

### <a name="manual-scaling-is-reset-by-autoscale-min-and-max"></a>Handmatig schalen opnieuw wordt ingesteld door de functie voor automatisch schalen min en max
Als u handmatig het aantal exemplaren op een waarde boven of onder de maximale bijwerkt, wordt automatisch de engine voor automatisch schalen geschaald terug naar de minimale (als hieronder) of het maximum (indien hierboven). Bijvoorbeeld, instellen u het bereik tussen 3 en 6. Als u een actief exemplaar hebt, de engine voor automatisch schalen kan worden geschaald naar drie exemplaren op de volgende keer wordt uitgevoerd. Op dezelfde manier als u de schaal handmatig op acht instanties instelt, op de volgende uitvoering voor automatisch schalen wordt de schaal terug zes exemplaren van de volgende keer wordt uitgevoerd.  Handmatig schalen is tijdelijk, tenzij u de regels voor automatisch schalen ook opnieuw instellen.

### <a name="always-use-a-scale-out-and-scale-in-rule-combination-that-performs-an-increase-and-decrease"></a>Gebruik altijd de combinatie van een scale-out en schaal in regel die een vergroten en verkleinen
Als u alleen een deel van de combinatie gebruikt, wordt voor automatisch schalen alleen actie ondernemen in één richting (schaal uit of in) totdat het maximum is bereikt of minimale _A telt van gedefinieerd in het profiel. Dit is niet optimaal, in het ideale geval wilt u uw resource om omhoog te schalen op tijdstippen met een hoog gebruik om beschikbaarheid te garanderen. Op dezelfde manier soms van laag gebruik die u wilt dat uw resource om omlaag te schalen, zodat u kunt kosten besparen.

### <a name="choose-the-appropriate-statistic-for-your-diagnostics-metric"></a>Kies de juiste statistieken voor uw diagnostische gegevens over metrische gegevens
Voor metrische gegevens, kunt u kiezen tussen *gemiddelde*, *Minimum*, *maximale* en *totale* als een metrische waarde om te schalen door. De meest voorkomende statistiek is *gemiddelde*.

### <a name="choose-the-thresholds-carefully-for-all-metric-types"></a>Kies de drempelwaarden voor alle metrische typen
Het is raadzaam om verschillende drempelwaarden voor scale-out en inschalen op basis van praktische situaties zorgvuldig te kiezen.

We *wordt niet aanbevolen* instellingen voor automatisch schalen, zoals de onderstaande voorbeelden met de dezelfde of een vergelijkbaar drempelwaarden voor uit- en voorwaarden:

* Aantal planexemplaren Verhoog met 1 tellen als aantal threads < = 600
* Instanties met 1 verlagen tellen als aantal threads > 600 =

Bekijk een voorbeeld van wat kan leiden tot een probleem dat verwarrend lijkt. Houd rekening met de volgende reeks.

1. Wordt ervan uitgegaan dat er twee exemplaren begint met zijn en vervolgens het gemiddelde aantal threads per exemplaar tot 625 groeit.
2. Automatisch schalen kan worden geschaald om het toevoegen van een derde exemplaar.
3. Vervolgens wordt ervan uitgegaan dat het gemiddelde aantal threads tussen exemplaar aan 575 valt.
4. Voordat u omlaag schalen, probeert om in te schatten welke de definitieve status voor automatisch schalen worden als opgeschaald. Bijvoorbeeld, 575 x 3 (huidige aantal instanties) = 1,725 / 2 (laatste aantal exemplaren wanneer omlaag geschaald) = 862.5 threads. Dit betekent dat voor automatisch schalen zou moeten onmiddellijk scale-out opnieuw, zelfs nadat deze in, geschaald als het gemiddelde aantal threads hetzelfde blijft, of zelfs slechts een kleine hoeveelheid valt. Echter als deze opnieuw, het hele proces opgeschaald wilt herhalen, waardoor een oneindige lus.
5. Om te voorkomen dat deze situatie ('op en neer' genoemd), schaalt automatisch schalen niet omlaag helemaal. In plaats daarvan wordt overgeslagen en de voorwaarde reevaluates opnieuw de volgende keer dat de taak van de service wordt uitgevoerd. Dit kan veel mensen verwarrend omdat voor automatisch schalen om te werken wanneer het gemiddelde aantal threads 575 is wouldn't weergegeven.

Schatting tijdens een inschalen is bedoeld om te voorkomen dat 'op en neer"situaties, waarbij in- en scale-out acties voortdurend heen en weer gaat. Dit gedrag Houd er rekening mee als u de dezelfde drempelwaarden voor de scale-out kiest en in.

U wordt aangeraden een geschikte marge tussen de scale-out en in de drempelwaarden te kiezen. Houd rekening met de volgende combinatie van betere regel als een voorbeeld.

* Aantal planexemplaren Verhoog met 1 tellen wanneer CPU-percentage > = 80
* Instanties met 1 verlagen tellen wanneer CPU-percentage < = 60

In dit geval  

1. Wordt ervan uitgegaan dat er zijn te beginnen met 2 exemplaren.
2. Als de gemiddelde CPU-percentage voor instanties op 80 gaat, automatisch schalen kan worden geschaald om het toevoegen van een derde exemplaar.
3. Nu wordt ervan uitgegaan dat na verloop van tijd de CPU-percentage en 60 valt.
4. Regel voor inschalen van automatisch schalen maakt een schatting van de definitieve status alsof het op schaal. Bijvoorbeeld, 60 x 3 (huidige aantal instanties) = 180 / 2 (laatste aantal exemplaren wanneer omlaag geschaald) = 90. Dus voor automatisch schalen biedt geen inschalen omdat het scale-out onmiddellijk opnieuw zou moeten. In plaats daarvan overgeslagen omlaag schalen.
5. De volgende tijd voor automatisch schalen wordt gecontroleerd, de CPU blijft tot 50 vallen. Schat opnieuw - 50 x 3, instantie = 150 / 2 exemplaren = 75 is lager dan de drempelwaarde voor scale-out van 80, zodat deze kan worden geschaald is naar 2-instanties.

### <a name="considerations-for-scaling-threshold-values-for-special-metrics"></a>Overwegingen voor het schalen van drempelwaarden voor speciale metrische gegevens
 De drempelwaarde is voor speciale metrische gegevens, zoals opslag- of Service Bus-wachtrij lengte van de metrische gegevens, het gemiddelde aantal berichten die per Huidig aantal instanties beschikbaar zijn. Kies zorgvuldig de drempelwaarde voor deze metrische gegevens.

Illustreren we gaan het met een voorbeeld om te controleren of dat u het probleem beter te begrijpen.

* Aantal planexemplaren Verhoog met 1 wanneer Storage-wachtrij bericht aantal > = 50
* Instanties met 1 aantal verlagen wanneer Storage-wachtrij bericht aantal < = 10

Houd rekening met de volgende volgorde:

1. Er zijn twee exemplaren van de storage-wachtrij.
2. Berichten steeds en bij het controleren van de storage-wachtrij, het totale aantal 50 leest. U kunt ervan uitgaan dat die voor automatisch schalen een actie van de scale-out moet worden gestart. Let echter op dat is het nog steeds 50/2 = 25 berichten per exemplaar. Scale-out wordt dus niet uitgevoerd. Voor de eerste scale-out gebeurt, moet het totale aantal berichten in de storage-wachtrij 100.
3. Vervolgens wordt ervan uitgegaan dat het totale aantal berichten gelijk is aan 100.
4. Een 3e exemplaar van de storage-wachtrij wordt toegevoegd vanwege een scale-out-actie.  De volgende uitschaalactie gebeurt niet totdat het totale aantal berichten in de wachtrij 150 bereikt omdat 150/3 = 50.
5. Nu wordt het aantal berichten in de wachtrij kleiner. Met drie exemplaren, de eerste inschaalactie gebeurt er wanneer het totale aantal berichten in alle wachtrijen maximaal 30 niet toevoegen omdat 30/3 = 10 berichten per instantie, die de drempelwaarde in te schalen.

### <a name="considerations-for-scaling-when-multiple-profiles-are-configured-in-an-autoscale-setting"></a>Overwegingen voor het schalen wanneer er meerdere profielen worden geconfigureerd in een instelling voor automatisch schalen
U kunt een standaard-profiel altijd zonder eventuele afhankelijkheden van planning of tijd toegepast wordt, in een instelling voor automatisch schalen of kunt u een terugkerende profiel of een profiel voor een bepaalde periode met een bereik van de datum en tijd.

Wanneer automatisch schalen service verwerkt, controleert deze altijd in de volgende volgorde:

1. Vaste datum profiel
2. Terugkerende profiel
3. Standaard-profiel ("altijd")

Als u een profiel-voorwaarde wordt voldaan, wordt de volgende voorwaarde profiel eronder niet gecontroleerd door voor automatisch schalen. Een profiel verwerkt voor automatisch schalen alleen op een tijdstip. Dit betekent dat als u ook een verwerkingsvoorwaarde van het profiel voor een lager niveau wilt, moet u opnemen die regels ook in het huidige profiel.

Laten we bekijken dit met behulp van een voorbeeld:

De onderstaande afbeelding ziet u een instelling voor automatisch schalen met een standaardprofiel met minimale exemplaren = 2- en maximumaantal exemplaren = 10. In dit voorbeeld regels zijn geconfigureerd voor het scale-out wanneer het aantal berichten in de wachtrij groter dan 10 is en inschalen als het aantal berichten in de wachtrij minder dan drie is. De resource kan nu worden geschaald tussen twee en tien exemplaren.

Er is bovendien een terugkerende profiel instellen voor maandag. Deze optie is ingesteld voor minimale exemplaren = 3 en maximaal exemplaren = 10. Dit betekent dat op maandag, de eerste keer automatische schaalaanpassing controles voor dit probleem, als het aantal exemplaren twee is, wordt er opgeschaald naar de nieuwe minimale van drie. Als voor automatisch schalen om te vinden van deze voorwaarde profiel blijft vergeleken (maandag), wordt alleen de regels worden verwerkt op basis van CPU scale-out/in voor dit profiel is geconfigureerd. Op dit moment is er niet gecontroleerd voor de lengte van de wachtrij. Echter, als u wilt dat de voorwaarde van de lengte van de wachtrij moet worden gecontroleerd, moet u opnemen deze regels uit het standaardprofiel ook in uw profiel maandag.

Op dezelfde manier als voor automatisch schalen terug naar de standaard-profiel schakelt, wordt eerst gecontroleerd als de minimale en maximale voorwaarden wordt voldaan. Als het aantal exemplaren op het moment van 12 is, wordt er opgeschaald naar 10, de maximaal toegestane aantal voor het standaardprofiel.

![Instellingen voor automatisch schalen](./media/autoscale-best-practices/insights-autoscale-best-practices-2.png)

### <a name="considerations-for-scaling-when-multiple-rules-are-configured-in-a-profile"></a>Overwegingen voor het schalen als meerdere regels zijn geconfigureerd in een profiel
Er zijn gevallen waarin u mogelijk meerdere regels in een profiel instellen. De volgende set regels voor automatisch schalen worden gebruikt door de services gebruiken wanneer u meerdere regels zijn ingesteld.

Op *uitschalen*, automatisch schalen wordt uitgevoerd als een regel wordt voldaan.
Op *schaal*, automatisch schalen moeten alle regels moet worden voldaan.

Ter illustratie, wordt ervan uitgegaan dat u de volgende vier regels voor automatisch schalen hebt:

* Als CPU < 30%, schaal in met 1
* Als geheugen < 50%, schaal in met 1
* Als CPU > 75%, scale-out met 1
* Als geheugen > 75%, scale-out met 1

Vervolgens het volgende gebeurt:

* Als geheugen is 50% CPU is 76% we scale-out.
* Als is 50% van CPU en geheugen % 76 we scale is-out.

Aan de andere kant als is 25% van CPU en geheugen 51% voor automatisch schalen is geen **niet** schaal. Om te schalen, in CPU moet 29% en het geheugen 49%.

### <a name="always-select-a-safe-default-instance-count"></a>Altijd een veilige standaardaantal exemplaar selecteren
Het standaardaantal exemplaren is belangrijk voor automatisch schalen die uw service naar het aantal dat kan worden geschaald wanneer metrische gegevens niet beschikbaar zijn. Selecteer daarom een standaardaantal exemplaren die voor uw workloads veilig is.

### <a name="configure-autoscale-notifications"></a>Meldingen over automatisch schalen configureren
Automatisch schalen wordt op het activiteitenlogboek plaatsen als een van de volgende situaties optreden:

* Problemen met een schaalbewerking voor automatisch schalen
* Service voor automatisch schalen voltooid is een schaalactie
* Service voor automatisch schalen mislukt om een scale-actie te ondernemen.
* Metrische gegevens zijn niet beschikbaar voor de service voor automatisch schalen om een scale-beslissing te nemen.
* Metrische gegevens zijn beschikbaar (herstel) opnieuw om te beslissen schaal.

U kunt ook een waarschuwing voor activiteitenlogboek gebruiken voor het bewaken van de status van de engine voor automatisch schalen. Hier volgen enkele voorbeelden aan [maken van een activiteit Log-waarschuwing voor het bewaken van alle voor automatisch schalen-engine-bewerkingen op uw abonnement](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert) of [maken van een activiteit Log-waarschuwing voor het bewaken van alle mislukte automatisch schaalt in / uitschalen bewerkingen op uw abonnement](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).

Naast het gebruik van waarschuwingen voor activiteitenlogboeken, kunt u ook de e-mailadres of webhook-meldingen om te worden geïnformeerd voor geslaagde schaalacties via op het tabblad meldingen van de instelling voor automatisch schalen configureren.

## <a name="next-steps"></a>Volgende stappen
- [Maak een activiteit Log-waarschuwing voor het bewaken van alle voor automatisch schalen-engine-bewerkingen op uw abonnement.](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert)
- [Een activiteit Log-waarschuwing voor het bewaken van alle mislukte automatisch schaalt in- / uitschalen bewerkingen op uw abonnement maken](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert)

