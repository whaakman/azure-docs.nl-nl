---
title: Verzamelen en analyseren van Azure activiteitenlogboeken in Log Analytics | Microsoft Docs
description: Om te analyseren en het activiteitenlogboek Azure zoeken in alle Azure-abonnement kunt u de Azure-activiteitenlogboeken-oplossing.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: dbac4c73-0058-4191-a906-e59aca8e2ee0
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/26/2018
ms.author: magoedte
ms.openlocfilehash: b6e823d9338d76a350569091d6794e3ac4a2eae9
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2018
ms.locfileid: "30283381"
---
# <a name="collect-and-analyze-azure-activity-logs-in-log-analytics"></a>Verzamelen en analyseren van Azure activiteitenlogboeken in Log Analytics

![Azure activiteitenlogboeken symbool](./media/log-analytics-activity/activity-log-analytics.png)

De activiteit Log Analytics-oplossing helpt u bij het analyseren en zoek de [Azure activiteitenlogboek](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) voor uw Azure abonnementen. De Azure Activity Log is een logboek biedt inzicht in de bewerkingen die worden uitgevoerd op resources in uw abonnementen. Het activiteitenlogboek heette vroeger *controlelogboeken* of *operationele logboeken* omdat gebeurtenissen voor uw abonnementen rapporteert.

Met het activiteitenlogboek, kunt u bepalen de *wat*, *die*, en *wanneer* voor een (PUT, POST, verwijderen schrijfbewerkingen) gemaakt voor de resources in uw abonnement. U kunt ook de status van de bewerkingen en andere relevante eigenschappen begrijpen. Het activiteitenlogboek omvat geen leesbewerkingen (GET) of -bewerkingen voor bronnen die gebruikmaken van het klassieke implementatiemodel.

Wanneer u uw Azure activiteitenlogboeken met Log Analytics verbindt, kunt u het volgende doen:

- De activiteitenlogboeken met vooraf gedefinieerde weergaven analyseren
- Analyseren en zoek- en logboeken van meerdere Azure-abonnementen
- Activiteitenlogboeken voor meer dan 90 dagen bewaren<sup>1</sup>
- Activiteitenlogboeken correleren met andere Azure-platform-en toepassingsgegevens
- Zie operationele activiteiten samenvoegen met status
- Weergave van trends van activiteiten plaatsvinden op elk van uw Azure-services
- Rapport over autorisatie wijzigingen op alle Azure-resources
- Onderbreking of service problemen die invloed hebben op uw resources bepalen
- Logboek zoeken met elkaar correleren activiteiten van de gebruiker, automatisch schalen operations autorisatie wijzigingen en status van de service voor andere logboeken of de metrische gegevens uit uw omgeving gebruiken

<sup>1</sup>standaard logboekanalyse houdt uw Azure activiteitenlogboeken negentig dagen weergegeven, zelfs als u op de laag gratis. Of, als u een instelling voor het bewaren van werkruimte van minder dan 90 dagen hebben. Als uw werkruimte bewaren die langer is dan 90 dagen heeft, worden de activiteitenlogboeken van de bewaard op basis van de bewaartermijn van uw werkruimte.

Log Analytics verzamelt activiteitenlogboeken kosteloos en slaat de logboeken voor 90 dagen gratis. Als u Logboeken voor meer dan 90 dagen opslaat, wordt u kosten gegevens bewaren voor de gegevens die langer is dan 90 dagen opgeslagen.

Wanneer u op de prijscategorie gratis bent, activiteitenlogboeken niet van toepassing op uw dagelijkse gegevens verbruik.

## <a name="connected-sources"></a>Verbonden bronnen

In tegenstelling tot de meeste andere Log Analytics-oplossingen, is niet gegevens voor activiteitenlogboeken verzameld door agents. Alle gegevens die worden gebruikt door de oplossing wordt geleverd rechtstreeks uit Azure.

| Verbonden bron | Ondersteund | Beschrijving |
| --- | --- | --- |
| [Windows-agents](log-analytics-windows-agent.md) | Nee | De oplossing verzamelt geen informatie van Windows-agents. |
| [Linux-agents](log-analytics-linux-agents.md) | Nee | De oplossing worden geen gegevens verzameld van Linux-agents. |
| [SCOM-beheergroep](log-analytics-om-agents.md) | Nee | De oplossing worden geen gegevens verzameld van agents in een verbonden SCOM-beheergroep. |
| [Azure Storage-account](log-analytics-azure-storage.md) | Nee | De oplossing verzamelt geen gegevens naar Azure storage. |

## <a name="prerequisites"></a>Vereisten

- Voor toegang tot Azure activiteit logboekgegevens, moet u een Azure-abonnement hebben.

## <a name="configuration"></a>Configuratie

Voer de volgende stappen uit voor het configureren van de activiteit Log Analytics-oplossing voor uw werkruimten.

1. Inschakelen van de activiteit Log Analytics-oplossing van de [Azure marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureActivityOMS?tab=Overview) of met behulp van de procedure beschreven in [toevoegen Log Analytics-oplossingen van de galerie met oplossingen](log-analytics-add-solutions.md).
2. Configureer activiteitenlogboeken naar uw werkruimte voor logboekanalyse.
    1. Selecteer uw werkruimte in de Azure portal en klik vervolgens op **Azure Activity log**.
    2. Klik op de naam van het abonnement voor elk abonnement.  
        ![Abonnement toevoegen](./media/log-analytics-activity/add-subscription.png)
    3. In de *SubscriptionName* blade, klikt u op **Connect**.  
        ![Verbinding maken met abonnement](./media/log-analytics-activity/subscription-connect.png)

Als u de oplossing met behulp van de OMS-portal toevoegt, ziet u de volgende tegel. Meld u aan bij de Azure portal verbinding maken met een Azure-abonnement van uw werkruimte.  
![beoordeling uitvoeren](./media/log-analytics-activity/tile-performing-assessment.png)

## <a name="using-the-solution"></a>De oplossing gebruiken

Wanneer u de activiteit Log Analytics-oplossing aan uw werkruimte toevoegt de **Azure activiteitenlogboeken** tegel is toegevoegd aan uw dashboard overzicht. Deze tegel wordt een telling van het aantal activiteitenrecords voor de Azure-abonnementen die de oplossing heeft voor toegang tot Azure weergegeven.

![Azure activiteitenlogboeken tegel](./media/log-analytics-activity/azure-activity-logs-tile.png)

### <a name="view-azure-activity-logs"></a>Registreert de activiteit Azure weergeven

Klik op de **Azure activiteitenlogboeken** tegel openen de **Azure activiteitenlogboeken** dashboard. Het dashboard bevat de blades in de volgende tabel. Elke blade bevat maximaal 10 items die overeenkomen met de criteria die blade voor het opgegeven bereik en tijdsbereik. U kunt een logboek-zoekquery waarmee alle records door te klikken op uitvoeren **alle** aan de onderkant van de blade of door te klikken op de blade-header.

Logboekgegevens van de activiteit wordt alleen weergegeven *nadat* u uw activiteitenlogboeken te gaan met de oplossing zodat u gegevens niet voor die tijd weergeven hebt geconfigureerd.

| Blade | Beschrijving |
| --- | --- |
| Logboekvermeldingen Azure activiteit | Toont een staafdiagram van de top Azure activiteit logboekvermelding record totalen voor het datumbereik dat u hebt geselecteerd en bevat een overzicht van de top 10 activiteit aanroepfuncties. Klik op het staafdiagram om uit te voeren een zoekopdracht logboek voor <code>AzureActivity</code>. Klik op een item aanroeper om te retourneren van alle logboekvermeldingen voor activiteit voor het artikel log zoekopdracht uitvoert. |
| Activiteitenlogboeken met Status | Toont een ringdiagram voor Azure log de activiteitsstatus voor het datumbereik dat u hebt geselecteerd. Ook wordt een lijst van een lijst met de top tien status records. Klik op de grafiek om uit te voeren een zoekopdracht logboek voor <code>AzureActivity &#124; summarize AggregatedValue = count() by ActivityStatus</code>. Klik op een statusitem om te retourneren van alle activiteit logboekvermeldingen voor deze record status logboek zoekopdracht uitvoert. |
| Activiteitenlogboeken door Resource | Geeft het totale aantal resources met activiteitenlogboeken en geeft een lijst van de top tien resources met record telt voor elke resource. Klik op het totale aantal gebied om uit te voeren een zoekopdracht logboek voor <code>AzureActivity &#124; summarize AggregatedValue = count() by Resource</code>, waarin alle Azure-resources beschikbaar zijn voor de oplossing. Klik op een bron uit voor het retourneren van alle activiteitenrecords voor die bron logboek zoekopdracht uitvoert. |
| Activiteitenlogboeken door Resourceprovider | Geeft het totale aantal resourceproviders die activiteit produceren registreert en geeft een lijst van de tien. Klik op het totale aantal gebied om uit te voeren een zoekopdracht logboek voor <code>AzureActivity &#124; summarize AggregatedValue = count() by ResourceProvider</code>, waarin alle Azure-resourceproviders. Klik op een resourceprovider als u wilt uitvoeren van een logboek zoekopdracht retourneren van alle activiteitenrecords voor de provider. |

![Azure activiteitenlogboeken-dashboard](./media/log-analytics-activity/activity-log-dash.png)

## <a name="next-steps"></a>Volgende stappen

- Maak een [waarschuwing](log-analytics-alerts-creating.md) wanneer een bepaalde activiteit gebeurt.
- Gebruik [logboek zoeken](log-analytics-log-searches.md) om gedetailleerde gegevens uit uw activiteitenlogboeken weer te geven.
