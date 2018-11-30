---
title: Verzamelen en analyseren van Azure-activiteitenlogboeken in Log Analytics | Microsoft Docs
description: U kunt de oplossing Azure-activiteitenlogboeken gebruiken om te analyseren en te zoeken naar de Azure-activiteitenlogboek in al uw Azure abonnementen.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: dbac4c73-0058-4191-a906-e59aca8e2ee0
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 6e711b99fa197a2092a673c87124e49fcf3c293e
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52635523"
---
# <a name="collect-and-analyze-azure-activity-logs-in-log-analytics"></a>Verzamelen en analyseren van Azure-activiteitenlogboeken in Log Analytics

![Azure-activiteitenlogboeken symbool](./media/log-analytics-activity/activity-log-analytics.png)

De oplossing Activity Log Analytics helpt u bij het analyseren en te zoeken naar de [Azure-activiteitenlogboek](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) in al uw Azure abonnementen. De Azure-activiteitenlogboek wordt een logboek dat inzicht in de bewerkingen die worden uitgevoerd voor resources in uw abonnementen biedt. Het activiteitenlogboek was voorheen bekend als *auditlogboeken* of *operationele logboeken* omdat deze dat deze gebeurtenissen voor uw abonnementen meldt.

Met het activiteitenlogboek, kunt u bepalen de *wat*, *die*, en *wanneer* voor (PUT, POST, DELETE schrijfbewerkingen) die voor de resources in uw abonnement. U kunt ook de status van de bewerkingen en andere relevante eigenschappen begrijpen. Het activiteitenlogboek bevat geen lees (GET)-bewerkingen of bewerkingen voor resources die gebruikmaken van het klassieke implementatiemodel.

Wanneer u uw Azure-activiteitenlogboeken verbinden met Log Analytics, kunt u het volgende doen:

- De activiteitenlogboeken met vooraf gedefinieerde weergaven analyseren
- Analyseren en zoek- en logboeken van meerdere Azure-abonnementen
- Activiteitenlogboeken langer dan 90 dagen behouden<sup>1</sup>
- Activiteitenlogboeken correleren met andere Azure-platform-en toepassingsgegevens
- Operationele activiteiten worden geaggregeerd op status
- Weergave van trends wat van activiteiten die plaatsvinden op elk van uw Azure-services
- Een rapport over autorisatie wijzigingen op al uw Azure-resources
- Om onderbreking of service problemen die invloed hebben op uw resources te identificeren
- Zoeken in Logboeken correleren gebruikersactiviteiten, bewerkingen voor automatisch schalen, autorisatie wijzigingen en status van de service met andere logboeken of de metrische gegevens uit uw omgeving gebruiken

<sup>1</sup>standaard Log Analytics blijft uw Azure-activiteitenlogboeken gedurende 90 dagen, zelfs als u op de gratis laag. Of, als u een instelling van de werkruimte behoud van minder dan 90 dagen. Als uw werkruimte behoud die langer is dan 90 dagen bevat, worden de activiteitenlogboeken bewaard op basis van de bewaarperiode van uw werkruimte.

Log Analytics verzamelt gratis activiteitenlogboeken en slaat de logboeken voor 90 dagen gratis. Als u Logboeken voor meer dan 90 dagen opslaat, worden er kosten voor het bewaren van gegevens voor de gegevens langer dan 90 dagen opgeslagen.

Wanneer u zich in de prijscategorie gratis, activiteitenlogboeken niet van toepassing op uw dagelijkse gegevensverbruik.

## <a name="connected-sources"></a>Verbonden bronnen

In tegenstelling tot de meeste andere Log Analytics-oplossingen, is niet-gegevens voor activiteitenlogboeken verzameld door agents. Alle gegevens die worden gebruikt door de oplossing wordt geleverd rechtstreeks vanuit Azure.

| Verbonden bron | Ondersteund | Beschrijving |
| --- | --- | --- |
| [Windows-agents](../azure-monitor/platform/agent-windows.md) | Nee | De oplossing verzamelt geen informatie van Windows-agents. |
| [Linux-agents](log-analytics-quick-collect-linux-computer.md) | Nee | De oplossing worden geen gegevens verzameld van Linux-agents. |
| [SCOM-beheergroep](log-analytics-om-agents.md) | Nee | De oplossing worden geen gegevens verzameld van agents in een verbonden SCOM-beheergroep. |
| [Azure Storage-account](log-analytics-azure-storage.md) | Nee | De oplossing verzamelt geen gegevens uit Azure storage. |

## <a name="prerequisites"></a>Vereisten

- Voor toegang tot Azure-activiteitenlogboekinformatie, moet u een Azure-abonnement hebben.

## <a name="configuration"></a>Configuratie

Voer de volgende stappen uit voor het configureren van de oplossing Activity Log Analytics voor uw werkruimten.

1. Schakel de Activity Log Analytics-oplossing in vanuit de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureActivityOMS?tab=Overview) of met behulp van de procedure beschreven in [Log Analytics-oplossingen toevoegen vanuit de Oplossingengalerie](../azure-monitor/insights/solutions.md).
2. Configureer de activiteitenlogboeken naar uw Log Analytics-werkruimte.
    1. In de Azure-portal, selecteert u uw werkruimte en klik vervolgens op **Azure-activiteitenlogboek**.
    2. Klik op de naam van het abonnement voor elk abonnement.  
        ![Abonnement toevoegen](./media/log-analytics-activity/add-subscription.png)
    3. In de *SubscriptionName* blade, klikt u op **Connect**.  
        ![Verbinding maken met abonnement](./media/log-analytics-activity/subscription-connect.png)

Meld u bij Azure portal een Azure-abonnement verbinden met uw werkruimte.  

## <a name="using-the-solution"></a>De oplossing gebruiken

Wanneer u de oplossing Activity Log Analytics aan uw werkruimte toevoegt, de **Azure-activiteitenlogboeken** tegel wordt toegevoegd aan uw dashboard overzicht. Deze tegel toont een telling van het aantal records voor de Azure-abonnementen die de oplossing toegang tot heeft Azure-activiteit.

![Azure-activiteitenlogboeken tegel](./media/log-analytics-activity/azure-activity-logs-tile.png)

### <a name="view-azure-activity-logs"></a>Weergave Azure-activiteitenlogboeken

Klik op de **Azure-activiteitenlogboeken** tegel om te openen de **Azure-activiteitenlogboeken** dashboard. Het dashboard bevat de blades in de volgende tabel. Elke blade bevat maximaal tien items die overeenkomen met de criteria voor het opgegeven bereik en de duur van die blade. U kunt zoeken in logboeken waarmee alle records worden geretourneerd door onderaan in de blade te klikken op **Alles bekijken** of door te klikken op de koptekst van de blade.

Gegevens van een activiteitenlogboek wordt alleen weergegeven *nadat* u uw activiteitenlogboeken naar de oplossing, zodat u kunt geen gegevens voordat u vervolgens bekijken hebt geconfigureerd.

| Blade | Beschrijving |
| --- | --- |
| Vermeldingen in het Azure-activiteit | Ziet u een staafdiagram van de bovenkant Azure activiteit logboekvermelding record totalen voor het datumbereik dat u hebt geselecteerd en bevat een overzicht van de bovenste 10 activiteit aanroepers. Klik op het staafdiagram voor het uitvoeren van een logboekzoekopdracht voor <code>AzureActivity</code>. Klik op een item oproepende functie voor het uitvoeren van een logboekzoekopdracht die alle vermeldingen in activiteit voor dat item geretourneerd. |
| Activiteitenlogboeken op Status | Geeft een ringdiagram voor de status van de Azure-activiteit-logboek voor het datumbereik dat u hebt geselecteerd. Ook geeft een lijst weer van een lijst van de bovenste tien status records. Klik op de grafiek voor het uitvoeren van een logboekzoekopdracht voor <code>AzureActivity &#124; summarize AggregatedValue = count() by ActivityStatus</code>. Klik op een statusitem voor het uitvoeren van een logboekzoekopdracht die alle vermeldingen in activiteit voor die statusrecord retourneren. |
| Activiteitenlogboeken per Resource | Geeft het totale aantal resources met activiteitenlogboeken en geeft een lijst van de top tien resources met record aantallen voor elke resource. Klik op het totale aantal gebied voor het uitvoeren van een logboekzoekopdracht voor <code>AzureActivity &#124; summarize AggregatedValue = count() by Resource</code>, waarin alle Azure-resources beschikbaar zijn voor de oplossing wordt getoond. Klik op een resource voor het uitvoeren van een logboekzoekopdracht die alle records van de activiteit voor die bron. |
| Activiteitenlogboeken door de Resourceprovider | Geeft het totale aantal resourceproviders die activiteit produceren registreert en geeft een lijst van de tien. Klik op het totale aantal gebied voor het uitvoeren van een logboekzoekopdracht voor <code>AzureActivity &#124; summarize AggregatedValue = count() by ResourceProvider</code>, waarin alle Azure-resource-providers. Klik op een resourceprovider voor het uitvoeren van een logboekzoekopdracht die alle activiteitsrecords retourneert voor de provider. |

![Azure-activiteitenlogboeken-dashboard](./media/log-analytics-activity/activity-log-dash.png)

## <a name="next-steps"></a>Volgende stappen

- Maak een [waarschuwing](../monitoring-and-diagnostics/alert-metric.md) wanneer een bepaalde activiteit gebeurt.
- Gebruik [zoeken in logboeken](log-analytics-queries.md) om gedetailleerde gegevens uit uw activiteitenlogboeken weer te geven.
