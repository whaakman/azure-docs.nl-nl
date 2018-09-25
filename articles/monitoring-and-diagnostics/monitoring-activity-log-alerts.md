---
title: Waarschuwingen voor activiteitenlogboeken in Azure Monitor
description: Bericht via SMS, webhook, SMS, e-mail en meer, wanneer bepaalde gebeurtenissen plaatsvinden in het activiteitenlogboek.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 5ddf510d50f38ed9aaf742bd06c330e53ffe1391
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46992793"
---
# <a name="alerts-on-activity-log"></a>Waarschuwingen voor activiteitenlogboeken 

## <a name="overview"></a>Overzicht
Waarschuwingen voor activiteitenlogboeken zijn waarschuwingen die activeren wanneer een nieuwe activiteit log-gebeurtenis optreedt die overeenkomt met de voorwaarden die zijn opgegeven in de waarschuwing. Ze zijn Azure-resources, zodat ze kunnen worden gemaakt met behulp van een Azure Resource Manager-sjabloon. Ze kunnen ook worden gemaakt, bijgewerkt of verwijderd in Azure portal. In dit artikel bevat de concepten achter waarschuwingen voor activiteitenlogboeken. Hier vervolgens ziet u hoe u Azure portal gebruiken voor het instellen van een waarschuwing op gebeurtenissen in het activiteitenlogboek. Zie voor meer informatie over het gebruik van [maken en beheren van waarschuwingen voor activiteitenlogboeken](alert-activity-log.md).

> [!NOTE]
> Waarschuwingen **kan geen** voor gebeurtenissen worden gemaakt in de categorie van de waarschuwing van activiteitenlogboek

Normaal gesproken u waarschuwingen voor activiteitenlogboek maken om meldingen te ontvangen wanneer:

* Specifieke bewerkingen die worden uitgevoerd voor resources in uw Azure-abonnement, vaak binnen het bereik van bepaalde resourcegroepen of resources. Bijvoorbeeld, als u wilt worden gewaarschuwd wanneer een virtuele machine in myProductionResourceGroup wordt verwijderd. Of u wilt worden gewaarschuwd als nieuwe rollen zijn toegewezen aan een gebruiker in uw abonnement.
* Er treedt een gebeurtenis van de health service op. Service health-gebeurtenissen opnemen van incidenten en onderhoudsgebeurtenissen die betrekking hebben op de resources in uw abonnement.

Een eenvoudige vergelijking voor informatie over voorwaarden waarop de waarschuwingsregels voor activiteitenlogboeken, kunnen worden gemaakt is om te verkennen of filteren van gebeurtenissen via [activiteitenlogboek in Azure Portal](monitoring-overview-activity-logs.md#query-the-activity-log-in-the-azure-portal). In Azure controleren: activiteitenlogboek, een kunt filteren of nodig gebeurtenis zoeken en vervolgens een waarschuwing te maken met behulp van de **waarschuwing voor activiteitenlogboek toevoegen** knop.

In beide gevallen wordt een waarschuwing voor activiteitenlogboek controleert alleen voor gebeurtenissen in het abonnement waarin de waarschuwing is gemaakt.

U kunt een waarschuwing voor activiteitenlogboek op basis van een eigenschap op het hoogste niveau in de JSON-object voor het activiteitenlogboek kunt configureren. Zie voor meer informatie, [overzicht van de Azure-activiteitenlogboek](./monitoring-overview-activity-logs.md#categories-in-the-activity-log). Zie voor meer informatie over service health-gebeurtenissen, [ontvangen van waarschuwingen voor activiteitenlogboeken voor servicemeldingen](./monitoring-activity-log-alerts-on-service-notifications.md). 

Waarschuwingen voor activiteitenlogboeken zijn een aantal veelgebruikte opties:

- **Categorie**: met beheerdersrechten, Service Health, automatisch schalen, beveiliging, beleid en aanbeveling. 
- **Bereik**: de afzonderlijke resource of set resources waarvoor de waarschuwing voor activiteitenlogboek is gedefinieerd. Bereik voor een waarschuwing voor activiteitenlogboek kan worden gedefinieerd op verschillende niveaus:
    - Niveau van de resource: bijvoorbeeld voor een specifieke virtuele machine
    - Niveau van de resourcegroep: bijvoorbeeld alle virtuele machines in een specifieke resourcegroep
    - Abonnementsniveau: bijvoorbeeld alle virtuele machines in een abonnement (of) alle resources in een abonnement
- **Resourcegroep**: standaard wordt de waarschuwingsregel is opgeslagen in dezelfde resourcegroep bevinden als die van het doel gedefinieerd binnen het bereik. De gebruiker kan ook definiëren voor de resourcegroep waar de waarschuwingsregel moet worden opgeslagen.
- **Resourcetype**: Resource Manager gedefinieerd naamruimte voor het doel van de waarschuwing.

- **De naam van bewerking**: naam van de bewerking The Resource Manager Role-Based Access Control.
- **Niveau**: de ernst van de gebeurtenis (uitgebreid, informatief, waarschuwing, fout of kritiek).
- **Status**: de status van de gebeurtenis, meestal aan de slag, mislukt of geslaagd.
- **Gebeurtenis gestart door**: ook wel bekend als de "oproepende functie." De e-mailadres of Azure Active Directory-id van de gebruiker die de bewerking heeft uitgevoerd.

> [!NOTE]
> In een abonnement maximaal 100 regels voor waarschuwingen voor de activiteit van de scope op een: één resource, alle resources in de resource-groep (of) gehele abonnementsniveau.

Wanneer een waarschuwing voor activiteitenlogboek wordt geactiveerd, wordt een actiegroep die u gebruikt om acties of meldingen te genereren. Een actiegroep is een herbruikbare set van ontvangers van meldingen, zoals e-mailadressen, telefoonnummers webhook-URL's of SMS. De ontvangers kunnen worden verwezen vanuit meerdere waarschuwingen en de meldingskanalen groep centraliseren. Wanneer u de waarschuwing voor activiteitenlogboeken hebt gedefinieerd, hebt u twee opties. U kunt:

* Gebruik een bestaande actiegroep die u in de waarschuwing voor activiteitenlogboeken.
* Een nieuwe actiegroep maken.

Zie voor meer informatie over actiegroepen [maken en beheren van actiegroepen in Azure portal](monitoring-action-groups.md).


## <a name="next-steps"></a>Volgende stappen
- Krijgen een [overzicht van waarschuwingen](monitoring-overview-alerts.md).
- Meer informatie over [maken en wijzigen van waarschuwingen voor activiteitenlogboeken](alert-activity-log.md).
- Controleer de [activiteit log waarschuwing webhook-schema](monitoring-activity-log-alerts-webhook.md).
- Meer informatie over [health servicemeldingen](monitoring-service-notifications.md).

