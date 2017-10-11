---
title: Maken van waarschuwingen in OMS Log Analytics | Microsoft Docs
description: Waarschuwingen in logboekanalyse kunnen belangrijke informatie in de OMS-opslagplaats te identificeren en proactief zullen u informeren over problemen of acties uit om te proberen op te lossen ze aanroepen.  In dit artikel beschrijft het maken van een waarschuwingsregel en details van de verschillende acties die ze kunnen ondernemen.
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/23/2017
ms.author: bwren
ms.openlocfilehash: c34fb7295e8f386f0e7cf2c1db6b26a3e49eae98
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="working-with-alert-rules-in-log-analytics"></a>Werken met regels voor waarschuwingen in Log Analytics
Waarschuwingen worden gemaakt door regels voor waarschuwingen die automatisch logboek zoekopdrachten met regelmatige tussenpozen worden uitgevoerd.  Ze maken een record voor een waarschuwing als de resultaten aan bepaalde criteria voldoen.  De regel kan een of meer acties voor het proactief zullen u informeren over de waarschuwing of een ander proces aanroepen vervolgens automatisch uitgevoerd.   

Dit artikel worden de processen kunt maken en bewerken van regels voor waarschuwingen met de OMS-portal.  Zie voor meer informatie over de verschillende instellingen en het implementeren van vereiste logica [inzicht-waarschuwingen in logboekanalyse](log-analytics-alerts.md).

>[!NOTE]
> U kunt momenteel maken of wijzigen van een waarschuwingsregel met de Azure portal. 

## <a name="create-an-alert-rule"></a>Een waarschuwingsregel maken

Voor het maken van een waarschuwingsregel met de OMS-portal, begint u met het maken van een zoekopdracht logboek voor de records die de waarschuwing moet worden aangeroepen.  De **waarschuwing** knop kan vervolgens worden zodat u kunt maken en configureren van de waarschuwingsregel.

>[!NOTE]
> Maximaal 250 waarschuwingsregels kan momenteel worden gemaakt in een OMS-werkruimte. 

1. Klik op de pagina overzicht van OMS **logboek zoeken**.
2. Maak een nieuw logboekbestand zoekquery of Selecteer een zoekbewerking opgeslagen logboek. 
3. Klik op **waarschuwing** boven aan de pagina opent de **waarschuwingsregel toevoegen** scherm.
4. Configureren van de waarschuwingsregel met informatie in [Details van waarschuwingsregels](#details-of-alert-rules) hieronder.
6. Klik op **opslaan** voltooien van de waarschuwingsregel.  Begint onmiddellijk uitgevoerd.


## <a name="edit-an-alert-rule"></a>Een waarschuwingsregel bewerken
U krijgt een lijst met alle regels voor waarschuwingen in de **waarschuwingen** menu van logboekanalyse **instellingen**.  

![Waarschuwingen beheren](./media/log-analytics-alerts/configure.png)

1. In de OMS-console selecteert de **instellingen** tegel.
2. Selecteer **waarschuwingen**.

U kunt meerdere acties uitvoeren vanuit deze weergave.

* Een regel uitschakelen door het selecteren van **uit** ernaast.
* Een waarschuwingsregel bewerken door te klikken op het potloodpictogram ernaast.
* Verwijderen van een waarschuwingsregel door te klikken op de **X** ernaast. 

## <a name="details-of-alert-rules"></a>Details van regels voor waarschuwingen
Wanneer u maakt of een waarschuwingsregel in de OMS-portal bewerkt, werkt u met de **waarschuwingsregel toevoegen** of **waarschuwingsregel bewerken** pagina.  De volgende tabellen beschrijven de velden in dit scherm.

![Waarschuwingsregel](media/log-analytics-alerts/add-alert-rule.png)

### <a name="alert-information"></a>Informatie over waarschuwingen
Dit zijn algemene instellingen voor de waarschuwingsregel en de waarschuwingen die wordt gemaakt.

| Eigenschap | Beschrijving |
|:--- |:---|
| Naam | Unieke naam voor de waarschuwingsregel. Deze naam is opgenomen in alle waarschuwingen die zijn gemaakt door de regel.  |
| Beschrijving | Optionele beschrijving van de waarschuwingsregel. |
| Ernst |Ernst van eventuele waarschuwingen die door deze regel wordt gemaakt. |

### <a name="search-query-and-time-window"></a>De query en het tijdstip zoekvenster
De query en het tijdstip zoekvenster die de records retourneren die worden geëvalueerd om te bepalen of er waarschuwingen moeten worden gemaakt.

| Eigenschap | Beschrijving |
|:--- |:---|
| Zoekopdracht | Dit is de query die wordt uitgevoerd.  De records geretourneerd door deze query wordt gebruikt om te bepalen of een waarschuwing wordt gemaakt.<br><br>Selecteer **huidige zoekquery gebruiken** voor het gebruik van de huidige query of Selecteer een bestaande opgeslagen zoekopdracht uit de lijst.  De querysyntaxis van de is opgegeven in het tekstvak waar u kunt deze desgewenst wijzigen. |
| Tijdvenster |Hiermee geeft u het tijdsbereik voor de query.  De query retourneert alleen de records die zijn gemaakt binnen dit bereik van de huidige tijd.  Dit kan een waarde tussen 5 minuten en 24 uur zijn.  Deze moet groter zijn dan of gelijk zijn aan de frequentie van waarschuwingen.  <br><br> Bijvoorbeeld, als de periode is ingesteld op 60 minuten en de query wordt uitgevoerd om 1:15 uur, wordt alleen de records die zijn gemaakt tussen 12:15 uur en 1:15 uur geretourneerd. |

Wanneer u de periode voor de waarschuwingsregel opgeeft, wordt het aantal bestaande records die overeenkomen met de zoekcriteria voor dat tijdvenster weergegeven.  Hiermee kunt u bepalen van de frequentie waarmee u het aantal resultaten die u verwacht.

### <a name="schedule"></a>Planning
Hiermee definieert u hoe vaak de zoekopdracht wordt uitgevoerd.

| Eigenschap | Beschrijving |
|:--- |:---|
| Frequentie van waarschuwingen | Hiermee geeft u op hoe vaak de query moet worden uitgevoerd. Is een waarde tussen 5 minuten en 24 uur. Moet gelijk zijn aan of kleiner is dan de periode.  Als de waarde groter dan de periode is, risico u records wordt overgeslagen.<br><br>Neem bijvoorbeeld een tijdvenster van 30 minuten en een frequentie van 60 minuten.  Als de query wordt uitgevoerd om 1:00 uur, wordt er records tussen 12:30 en 13:00 uur.  De volgende keer dat de query werd uitgevoerd is 2:00 wanneer er records tussen 1:30 en 2:00 zou geretourneerd.  Alle records gemaakt tussen de 1:00 en 1:30 zou nooit worden geëvalueerd. |


### <a name="generate-alert-based-on"></a>Op basis van de waarschuwing wordt gegenereerd
Hiermee definieert u de criteria die wordt geëvalueerd op basis van de resultaten van de zoekopdracht om te bepalen of een waarschuwing moet worden gemaakt.  Deze gegevens zullen afwijken, afhankelijk van het type van de waarschuwingsregel die u selecteert.  U kunt details ophalen voor de verschillende waarschuwingsregel typen uit [inzicht-waarschuwingen in logboekanalyse](log-analytics-alerts.md).

| Eigenschap | Beschrijving |
|:--- |:---|
| Waarschuwingen onderdrukken | Wanneer u de onderdrukking voor de waarschuwingsregel inschakelt, worden de acties voor de regel uitgeschakeld gedurende een gedefinieerde periode na het maken van een nieuwe waarschuwing. De regel nog steeds actief is en waarschuwing records maakt als de criteria wordt voldaan. Dit is dat u het probleem te verhelpen zonder dubbele acties worden uitgevoerd, tijd. |

#### <a name="number-of-results-alert-rules"></a>Aantal resultaten waarschuwingsregels

| Eigenschap | Beschrijving |
|:--- |:---|
| Aantal resultaten |Een waarschuwing wordt gemaakt als het aantal records dat wordt geretourneerd door de query **groter is dan** of **minder dan** de waarde die u opgeeft.  |

#### <a name="metric-measurement-alert-rules"></a>Waarschuwingsregels metrische meting

| Eigenschap | Beschrijving |
|:--- |:---|
| Cumulatieve waarde | Elke cumulatieve waarde in de resultaten moet worden overschreden om te worden beschouwd als een schending van de drempelwaarde. |
| Op basis van de trigger-waarschuwing | Het aantal schendingen van een waarschuwing moet worden gemaakt.  U kunt opgeven **totaal aantal schendingen** instellen voor elke combinatie van inbreuk op de resultaten of **opeenvolgende schendingen** om te vereisen dat de schendingen in opeenvolgende steekproeven plaatsvinden moeten. |

### <a name="actions"></a>Acties
Waarschuwingsregels maakt altijd een [waarschuwing record](#alert-records) wanneer de drempelwaarde wordt voldaan.  U kunt ook een of meer antwoorden worden uitgevoerd zoals een e-mailbericht verzenden of een runbook starten definiëren.



#### <a name="email-actions"></a>E-acties
E-acties verzend een e-mailbericht met de details van de waarschuwing naar een of meer geadresseerden.

| Eigenschap | Beschrijving |
|:--- |:---|
| E-mailmelding |Geef **Ja** als u wilt dat een e-mailbericht moet worden verzonden wanneer de waarschuwing wordt geactiveerd. |
| Onderwerp |Onderwerpen in het e-mailbericht.  U kunt de hoofdtekst van het e-mailbericht niet wijzigen. |
| ontvangers |De adressen van alle e-mailgeadresseerden.  Als u meer dan één adres opgeeft, scheidt u de adressen met een puntkomma (;). |

#### <a name="webhook-actions"></a>Webhookacties
Webhookacties kunnen u een extern proces via één HTTP POST-aanvraag worden aangeroepen.

| Eigenschap | Beschrijving |
|:--- |:---|
| Webhook |Geef **Ja** als u een webhook aanroepen wilt wanneer de waarschuwing wordt geactiveerd. |
| Webhook-URL |De URL van de webhook. |
| Aangepaste JSON-nettolading opnemen |Selecteer deze optie als u wilt de nettolading van de standaard vervangen door een aangepaste nettolading. |
| Voer de aangepaste JSON-nettolading |De aangepaste nettolading voor de webhook.  Zie de vorige sectie voor meer informatie. |

#### <a name="runbook-actions"></a>Runbook-acties
Runbook-acties starten van een runbook in Azure Automation. 

>[!NOTE]
> U hebt de Automation-oplossing geïnstalleerd in de werkruimte voor deze actie kan alleen worden ingeschakeld. 


| Eigenschap | Beschrijving |
|:--- |:---|
| Runbook | Geef **Ja** als u wilt een Azure Automation-runbook wordt gestart wanneer de waarschuwing wordt geactiveerd.  |
| Automation-account | Hiermee geeft u het Automation-account dat runbooks zijn geselecteerd uit.  Dit is het actie-account dat gekoppeld aan de werkruimte. |
| Selecteer een runbook | Selecteer het runbook dat u starten wilt wanneer een waarschuwing wordt gemaakt. |
| Voer op | Selecteer **Azure** voor het uitvoeren van het runbook in de cloud.  Selecteer **hybride worker** voor het uitvoeren van het runbook op een agent met [Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md ) geïnstalleerd.  |




## <a name="next-steps"></a>Volgende stappen
* Installeer de [waarschuwing beheeroplossing](log-analytics-solution-alert-management.md) voor het analyseren van waarschuwingen die zijn gemaakt in logboekanalyse samen met waarschuwingen van System Center Operations Manager (SCOM) verzameld.
* Lees meer over [Meld zoekopdrachten](log-analytics-log-searches.md) kunnen die waarschuwingen genereren.
* Een stapsgewijze Kennismaking voltooien [configureren van een webook](log-analytics-alerts-webhooks.md) met een waarschuwingsregel.  
* Meer informatie over het schrijven van [runbooks in Azure Automation](https://azure.microsoft.com/documentation/services/automation) oplossen van problemen die worden aangeduid met waarschuwingen.

