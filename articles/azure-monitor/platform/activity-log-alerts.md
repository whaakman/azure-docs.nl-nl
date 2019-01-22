---
title: Waarschuwingen voor activiteitenlogboeken in Azure Monitor
description: Bericht via SMS, webhook, SMS, e-mail en meer, wanneer bepaalde gebeurtenissen plaatsvinden in het activiteitenlogboek.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 3459ef4caa99a2147144354165bef7d2852e87cc
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54433343"
---
# <a name="alerts-on-activity-log"></a>Waarschuwingen voor activiteitenlogboeken 

## <a name="overview"></a>Overzicht
Waarschuwingen voor activiteitenlogboeken zijn waarschuwingen die activeren wanneer een nieuwe activiteit log-gebeurtenis optreedt die overeenkomt met de voorwaarden die zijn opgegeven in de waarschuwing. Ze zijn Azure-resources, zodat ze kunnen worden gemaakt met behulp van een Azure Resource Manager-sjabloon. Ze kunnen ook worden gemaakt, bijgewerkt of verwijderd in Azure portal. In dit artikel bevat de concepten achter waarschuwingen voor activiteitenlogboeken. Hier vervolgens ziet u hoe u Azure portal gebruiken voor het instellen van een waarschuwing op gebeurtenissen in het activiteitenlogboek. Zie voor meer informatie over het gebruik van [maken en beheren van waarschuwingen voor activiteitenlogboeken](../../azure-monitor/platform/alerts-activity-log.md).

> [!NOTE]
> Waarschuwingen **kan geen** voor gebeurtenissen worden gemaakt in de categorie van de waarschuwing van activiteitenlogboek

Normaal gesproken u waarschuwingen voor activiteitenlogboek maken om meldingen te ontvangen wanneer:

* Specifieke bewerkingen die worden uitgevoerd voor resources in uw Azure-abonnement, vaak binnen het bereik van bepaalde resourcegroepen of resources. Bijvoorbeeld, als u wilt worden gewaarschuwd wanneer een virtuele machine in myProductionResourceGroup wordt verwijderd. Of u wilt worden gewaarschuwd als nieuwe rollen zijn toegewezen aan een gebruiker in uw abonnement.
* Er treedt een gebeurtenis van de health service op. Service health-gebeurtenissen opnemen van incidenten en onderhoudsgebeurtenissen die betrekking hebben op de resources in uw abonnement.

Een eenvoudige vergelijking voor informatie over voorwaarden waarop de waarschuwingsregels voor activiteitenlogboeken, kunnen worden gemaakt is om te verkennen of filteren van gebeurtenissen via [activiteitenlogboek in Azure Portal](../../azure-monitor/platform/activity-logs-overview.md#query-the-activity-log-in-the-azure-portal). In Azure controleren: activiteitenlogboek, een kunt filteren of nodig gebeurtenis zoeken en vervolgens een waarschuwing te maken met behulp van de **waarschuwing voor activiteitenlogboek toevoegen** knop.

In beide gevallen wordt een waarschuwing voor activiteitenlogboek controleert alleen voor gebeurtenissen in het abonnement waarin de waarschuwing is gemaakt.

U kunt een waarschuwing voor activiteitenlogboek op basis van een eigenschap op het hoogste niveau in de JSON-object voor het activiteitenlogboek kunt configureren. Zie voor meer informatie, [overzicht van de Azure-activiteitenlogboek](./../../azure-monitor/platform/activity-logs-overview.md#categories-in-the-activity-log). Zie voor meer informatie over service health-gebeurtenissen, [ontvangen van waarschuwingen voor activiteitenlogboeken voor servicemeldingen](./../../azure-monitor/platform/alerts-activity-log-service-notifications.md). 

Waarschuwingen voor activiteitenlogboeken zijn een aantal veelgebruikte opties:

- **Categorie**: Met beheerdersrechten, Service-status voor automatisch schalen, beveiliging, beleid en aanbeveling. 
- **Bereik**: De afzonderlijke resource of set resources waarvoor de waarschuwing voor activiteitenlogboek is gedefinieerd. Bereik voor een waarschuwing voor activiteitenlogboek kan worden gedefinieerd op verschillende niveaus:
    - Niveau van de resource: Bijvoorbeeld: voor een specifieke virtuele machine
    - Niveau van de resourcegroep: Bijvoorbeeld, alle virtuele machines in een specifieke resourcegroep
    - Abonnement: Bijvoorbeeld, alle virtuele machines in een abonnement (of) alle resources in een abonnement
- **Resourcegroep**: De waarschuwingsregel wordt standaard opgeslagen in dezelfde resourcegroep bevinden als die van het doel gedefinieerd binnen het bereik. De gebruiker kan ook definiëren voor de resourcegroep waar de waarschuwingsregel moet worden opgeslagen.
- **Resourcetype**: Resource Manager gedefinieerd naamruimte voor het doel van de waarschuwing.

- **De naam van bewerking**: De naam van de Resource Manager Role-Based Access Control-bewerking.
- **Niveau**: De ernst van de gebeurtenis (uitgebreid, informatief, waarschuwing, fout of kritiek).
- **Status**: De status van de gebeurtenis, meestal aan de slag, mislukt of geslaagd.
- **Gebeurtenis gestart door**: Ook wel bekend als de "oproepende functie." De e-mailadres of Azure Active Directory-id van de gebruiker die de bewerking heeft uitgevoerd.

> [!NOTE]
> In een abonnement maximaal 100 regels voor waarschuwingen voor een activiteit van de scope op beide kan worden gemaakt: één resource, alle resources in de resource-groep (of) gehele abonnementsniveau.

Wanneer een waarschuwing voor activiteitenlogboek wordt geactiveerd, wordt een actiegroep die u gebruikt om acties of meldingen te genereren. Een actiegroep is een herbruikbare set van ontvangers van meldingen, zoals e-mailadressen, telefoonnummers webhook-URL's of SMS. De ontvangers kunnen worden verwezen vanuit meerdere waarschuwingen en de meldingskanalen groep centraliseren. Wanneer u de waarschuwing voor activiteitenlogboeken hebt gedefinieerd, hebt u twee opties. U kunt:

* Gebruik een bestaande actiegroep die u in de waarschuwing voor activiteitenlogboeken.
* Een nieuwe actiegroep maken.

Zie voor meer informatie over actiegroepen [maken en beheren van actiegroepen in Azure portal](../../azure-monitor/platform/action-groups.md).


## <a name="next-steps"></a>Volgende stappen
- Krijgen een [overzicht van waarschuwingen](../../azure-monitor/platform/alerts-overview.md).
- Meer informatie over [maken en wijzigen van waarschuwingen voor activiteitenlogboeken](../../azure-monitor/platform/alerts-activity-log.md).
- Controleer de [activiteit log waarschuwing webhook-schema](activity-log-alerts-webhook.md).
- Meer informatie over [health servicemeldingen](../../azure-monitor/platform/service-notifications.md).


