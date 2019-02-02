---
title: Overzicht van klassieke waarschuwingen in Microsoft Azure en Azure Monitor
description: Klassieke waarschuwingen worden afgeschaft. Waarschuwingen kunnen u metrische gegevens voor Azure-resources, gebeurtenissen en logboeken controleren en een melding wanneer een bepaalde voorwaarde wordt voldaan.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/29/2018
ms.author: robb
ms.openlocfilehash: 596b7eb53d82f8eee7da439314994fd0951c7e64
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55661714"
---
# <a name="what-are-classic-alerts-in-microsoft-azure"></a>Wat zijn klassieke waarschuwingen in Microsoft Azure?

> [!NOTE]
> In dit artikel wordt beschreven hoe u oudere klassieke metrische waarschuwingen maken. Azure Monitor nu ondersteunt [nieuwere bijna realtime metrische waarschuwingen en een nieuwe ervaring voor waarschuwingen](../../azure-monitor/platform/alerts-overview.md). Klassieke waarschuwingen zijn [gepland om te worden buiten gebruik gesteld](https://docs.microsoft.com/azure/azure-monitor/platform/monitoring-classic-retirement).  
>

Waarschuwingen kunnen u voorwaarden configureren voor gegevens en een melding te ontvangen als de voorwaarden die overeenkomen met de meest recente gegevens.

## <a name="old-and-new-alerting-capabilities"></a>Oude en nieuwe waarschuwingsfuncties

In het verleden Azure Monitor, Application Insights, Log Analytics en status van de Service afzonderlijke heeft waarschuwingsfuncties. Overwerk, Azure, is verbeterd en combinatie van de gebruikersinterface en de verschillende methoden van waarschuwingen. De samenvoeging is nog steeds in behandeling. Waarschuwingen

Klassieke waarschuwingen ziet u alleen in het scherm van de gebruiker klassieke waarschuwingen in de Azure Portal. Dit scherm wordt weergegeven in de **klassieke waarschuwingen weergeven** knop op het scherm waarschuwingen. 

 ![Waarschuwing keuzes in Azure portal](media/alerts-classic.overview/monitor-alert-screen2.png)

De nieuwe ervaring voor waarschuwingen gebruiker heeft de volgende voordelen via de ervaring voor klassieke waarschuwingen:
-   **Betere notification system** -actiegroepen, die zijn benoemde groepen van meldingen en acties die opnieuw kunnen worden gebruikt in meerdere waarschuwingen voor alle nieuwere waarschuwingen gebruiken. Gebruik geen actiegroepen klassieke metrische waarschuwingen en oudere Log Analytics-waarschuwingen.
-   **Een geïntegreerde ervaring van de ontwerper** : alle waarschuwingen maken voor metrische gegevens, logboeken en activiteit aanmelden via Azure Monitor, Log Analytics, en Application Insights is op één plek.
-   **Weergave geactiveerde waarschuwingen van Log Analytics in Azure portal** -kunt u nu ook Zie Log Analytics-waarschuwingen in uw abonnement wordt gestart. Eerder waren deze in een afzonderlijke portal.
-   **Scheiding van de geactiveerde waarschuwingen en waarschuwingsregels** - waarschuwingsregels (de definitie van de voorwaarde die wordt een waarschuwing geactiveerd) en geactiveerde waarschuwingen (een exemplaar van de waarschuwingsregel starten) van elkaar worden onderscheiden, zodat de operationele en configuratie-weergaven worden gescheiden.
-   **Betere werkstroom** - de nieuwe waarschuwingen ontwerpen gidsen voor gebruikerservaring van de gebruiker het proces van het configureren van een waarschuwingsregel, waardoor het eenvoudiger voor het detecteren van de juiste dingen die u moet u een melding op.
-   **Consolidatie van waarschuwingen voor slimme** en **waarschuwingsstatus instellen** -nieuwere waarschuwingen omvatten functionaliteit waarmee automatische groepering van vergelijkbare waarschuwingen samen om te beperken van overbelasting in de gebruikersinterface. 

De nieuwere metrische waarschuwingen hebben de volgende voordelen via de klassieke metrische waarschuwingen:
-   **Verbeterde latentie**: Nieuwere metrische waarschuwingen kunnen zo vaak als elke minuut wordt uitgevoerd. Oudere metrische waarschuwingen wordt altijd uitgevoerd met een frequentie van vijf minuten. Nieuwere waarschuwingen hebben kleinere vertraging van probleem exemplaar op actie (3 tot 5 minuten) of meldingen vergroten. Oudere waarschuwingen zijn 5 tot 15 minuten, afhankelijk van het type.  Waarschuwingen hebben meestal de vertraging van 10 tot en met 15 - minuut vanwege de tijd die nodig zijn om op te nemen van de logboeken, maar nieuwere verwerkingsmethoden dat moment verlagen. 
-   **Ondersteuning voor multidimensionale metrische gegevens**: U kunt waarschuwingen op eendimensionale metrische gegevens, zodat u kunt een interessante segment van de metrische gegevens controleren.
-   **Meer controle over metrische voorwaarden**: U kunt uitgebreidere waarschuwingsregels definiëren. De nieuwere waarschuwingen bewaken van de maximale, minimale, gemiddelde en totale waarden van metrische gegevens ondersteunen.
-   **Bewaking van meerdere metrische gegevens gecombineerd**: U kunt meerdere metrische gegevens (op dit moment maximaal twee metrische gegevens) met een enkele regel bewaken. Een waarschuwing wordt geactiveerd als beide metrische gegevens in strijd is met hun respectieve drempelwaarden voor de opgegeven periode.
-   **Betere notification system**: Alle nieuwere waarschuwingen gebruiken [actiegroepen](../../azure-monitor/platform/action-groups.md), die zijn benoemde groepen van meldingen en acties die in meerdere waarschuwingen kunnen worden hergebruikt.  Gebruik geen actiegroepen klassieke metrische waarschuwingen en oudere Log Analytics-waarschuwingen. 
-   **Metrische gegevens uit logboeken** (openbare preview): Logboekgegevens die naar Log Analytics kunnen nu worden geëxtraheerd en geconverteerd naar metrische gegevens van Azure Monitor en klikt u vervolgens een melding op net als andere metrische gegevens. Zie [waarschuwingen (klassiek)](alerts-classic.overview.md) voor de belangrijkste termen die specifiek zijn voor klassieke waarschuwingen. 


## <a name="classic-alerts-on-azure-monitor-data"></a>Klassieke waarschuwingen van Azure Monitor-gegevens
Er zijn twee soorten klassieke waarschuwingen beschikbaar - metrische waarschuwingen en waarschuwingen voor activiteitenlogboeken.

* **Klassieke metrische waarschuwingen** -deze waarschuwing wordt geactiveerd wanneer de waarde van een opgegeven metrische gegevens van een drempel die u toewijst overschrijden. De waarschuwing genereert een melding wanneer deze drempelwaarde wordt overschreden en de waarschuwing voorwaarde wordt voldaan. Op dat moment wordt de waarschuwing beschouwd als 'Geactiveerd'. Er wordt nog een melding gegenereerd wanneer deze '' - dat wil zeggen, opgelost is wanneer de drempel voor het opnieuw wordt gepasseerd en niet meer aan de voorwaarde wordt voldaan.

* **Waarschuwingen voor klassieke activiteitenlogboeken** -de waarschuwing voor een streaming-logboek die wordt geactiveerd op de vermelding van een activiteitenlogboek-gebeurtenis die overeenkomt met uw filtercriteria. Deze waarschuwingen hebben slechts één status 'Geactiveerd'. De waarschuwings-engine de filtercriteria wordt gewoon van toepassing op een nieuwe gebeurtenis. Het is niet zoeken naar oudere vermeldingen. Deze waarschuwingen kunnen u een melding wanneer een nieuw Health Service-incident plaatsvindt of wanneer een gebruiker of toepassing een bewerking uitvoert in uw abonnement, bijvoorbeeld 'Delete virtuele machine.'

Voor diagnostische logboekgegevens die beschikbaar zijn via Azure Monitor, de gegevens in Log Analytics (voorheen OMS) route en de waarschuwing voor een Log Analytics-query gebruiken. Meld u nu maakt gebruik van Analytics de [methode nieuwe waarschuwingen](../../azure-monitor/platform/alerts-overview.md) 

Het volgende diagram geeft een overzicht van bronnen van de gegevens in Azure Monitor en, conceptueel gezien hoe u waarschuwt dat gegevens.

![Waarschuwingen uitgelegd](media/alerts-classic.overview/Alerts_Overview_Resource_v5.png)

## <a name="taxonomy-of-alerts-classic"></a>Taxonomie van waarschuwingen (klassiek)
Azure maakt gebruik van de volgende termen om te beschrijven van klassieke waarschuwingen en de bijbehorende functies:
* **Waarschuwing** -een definitie van de criteria (een of meer regels of voorwaarden) die wordt geactiveerd wanneer voldaan.
* **Actieve** -de status als de criteria die zijn gedefinieerd door een klassieke waarschuwing wordt voldaan.
* **Opgelost** -de status wanneer aan de criteria die zijn gedefinieerd door een klassieke waarschuwing niet meer na het eerder zijn voldaan wordt voldaan.
* **Melding** : de actie op die op basis van een klassieke waarschuwing actief.
* **Actie** -specifieke teruggebeld te worden verzonden naar een ontvanger van een melding (bijvoorbeeld een adres e-mailen of plaatsen in een webhook-URL). Meldingen kunnen meestal meerdere acties activeren.

## <a name="how-do-i-receive-a-notification-from-an-azure-monitor-classic-alert"></a>Hoe ontvang ik een melding van een klassieke waarschuwing voor Azure Monitor?
Waarschuwingen van andere services van Azure gebruikt in het verleden, hun eigen methoden voor ingebouwde meldingen. 

Azure Monitor gemaakt een herbruikbare melding groeperen met de naam *actiegroepen*. Actiegroepen opgeven een set van ontvangers voor een melding. Telkens wanneer een waarschuwing wordt geactiveerd die verwijst naar de actiegroep ontvangt alle ontvangers deze melding. Actiegroepen kunnen u een groepering van ontvangers (bijvoorbeeld in de lijst op de aanroep engineer) gebruiken voor veel waarschuwingen objecten. Actiegroepen ondersteuning voor aanmelding door plaatsen in een webhook-URL naast het e-mailadressen, SMS-nummers en een aantal andere acties.  Zie voor meer informatie, [actiegroepen](../../azure-monitor/platform/action-groups.md). 

Oudere klassieke waarschuwingen voor activiteitenlogboek actiegroepen gebruiken.

De oudere metrische waarschuwingen wordt actiegroepen echter niet gebruiken. In plaats daarvan kunt u de volgende acties: 
- E-mailmeldingen verzenden naar de servicebeheerder, medebeheerders of naar extra e-mailadressen die u opgeeft.
- Een webhook, waarmee u start automation extra acties kunt aanroepen.

Webhooks maakt het mogelijk om automatisering en herstel, bijvoorbeeld:
    - Azure Automation Runbook
    - Azure Functions
    - Azure Logic App
    - Een service van derden

## <a name="next-steps"></a>Volgende stappen
Informatie ophalen over de regels voor waarschuwingen en ze te configureren met behulp van:

* Meer informatie over [metrische gegevens](../../azure-monitor/platform/data-collection.md)
* Configureer [klassieke metrische waarschuwingen via Azure portal](alerts-classic-portal.md)
* Configureer [klassieke metrische waarschuwingen PowerShell](alerts-classic-portal.md)
* Configureer [klassieke metrische waarschuwingen opdrachtregelinterface (CLI)](alerts-classic-portal.md)
* Configureer [klassieke metrische waarschuwingen Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Meer informatie over [activiteitenlogboek](../../azure-monitor/platform/activity-logs-overview.md)
* Configureer [waarschuwingen voor activiteitenlogboeken via Azure portal](../../azure-monitor/platform/activity-log-alerts.md)
* Configureer [waarschuwingen voor activiteitenlogboeken via Resource Manager](alerts-activity-log.md)
* Controleer de [activiteit log waarschuwing webhook-schema](../../azure-monitor/platform/activity-log-alerts-webhook.md)
* Meer informatie over [actiegroepen](../../azure-monitor/platform/action-groups.md)
* Configureer [nieuwere waarschuwingen](../../azure-monitor/platform/alerts-metric.md)
