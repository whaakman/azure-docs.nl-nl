---
title: Overzicht van klassieke waarschuwingen in Microsoft Azure en Azure-Monitor
description: Waarschuwingen kunnen u Azure-resource metrische gegevens, gebeurtenissen of Logboeken bewaken en een melding krijgen wanneer een opgegeven voorwaarde wordt voldaan.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: robb
ms.component: alerts
ms.openlocfilehash: 89c3975a1212aa991e42e0cce4fe5521b53bd373
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263667"
---
# <a name="what-are-classic-alerts-in-microsoft-azure"></a>Wat zijn klassieke waarschuwingen in Microsoft Azure?

> [!NOTE]
> In dit artikel wordt beschreven hoe oudere klassieke metrische waarschuwingen maken. Azure ondersteunt van de Monitor nu [nieuwere metrische vrijwel in realtime-waarschuwingen en een nieuwe waarschuwingen ervaring](monitoring-overview-unified-alerts.md). 
>

Waarschuwingen kunnen u voorwaarden configureren over gegevens en worden gewaarschuwd wanneer de voorwaarden overeen met de meest recente bewakingsgegevens.


## <a name="alerts-on-azure-monitor-data"></a>Waarschuwingen op gegevens van de Azure-Monitor
Er zijn twee soorten klassieke waarschuwingen beschikbaar - metrische waarschuwingen en activiteit logboek waarschuwingen.

* **Klassieke metrische waarschuwingen** -deze waarschuwing wordt geactiveerd wanneer de waarde van een opgegeven waarde overschrijdt de drempelwaarde die u toewijst. De waarschuwing genereert een melding wanneer de waarschuwing is 'geactiveerd' (als de drempelwaarde is overschreden en de waarschuwing voorwaarde wordt voldaan), evenals wanneer deze 'opgelost is' (wanneer de drempel opnieuw wordt gepasseerd en niet langer aan de voorwaarde wordt voldaan). Zie hieronder voor nieuwere metrische waarschuwingen.

* **Klassieke activiteit logboek waarschuwingen** -een streaming logboek waarschuwing die wordt geactiveerd wanneer een gebeurtenis activiteitenlogboek wordt gegenereerd dat overeenkomt met filtercriteria die u hebt toegewezen. Deze waarschuwingen hebben slechts één status 'Geactiveerd', omdat de waarschuwings-engine gewoon de filtercriteria die van toepassing op een nieuwe gebeurtenis. Deze waarschuwingen kunnen worden gebruikt om te worden geïnformeerd wanneer er een nieuw incident in de servicestatus optreedt of als een gebruiker of toepassing een bewerking uitvoert in uw abonnement, bijvoorbeeld 'Virtuele machine verwijderen'.

We raden voor diagnostische logboekgegevens die beschikbaar is via Azure Monitor, routering van de gegevens in logboekanalyse (voorheen OMS) en het gebruik van een waarschuwing Log Analytics-query. Meld u Analytics nu gebruikt de [methode nieuwe waarschuwingen](monitoring-overview-unified-alerts.md) 

Het volgende diagram geeft een overzicht van bronnen van de gegevens in Azure Monitor en, conceptueel gezien hoe u waarschuwt af die gegevens.

![Waarschuwingen uitgelegd](./media/monitoring-overview-alerts/Alerts_Overview_Resource_v4.png)

## <a name="taxonomy-of-azure-monitor-alerts-classic"></a>Taxonomie van waarschuwingen van Azure controleren (klassiek)
Azure maakt gebruik van de volgende termen om klassieke waarschuwingen en hun functies te beschrijven:
* **Waarschuwing** -definitie van een van de criteria (een of meer regels of -voorwaarden) die wordt geactiveerd wanneer voldaan.
* **Actieve** -de status wanneer aan de criteria die zijn gedefinieerd door een klassieke waarschuwing wordt voldaan.
* **Opgelost** -de status wanneer aan de criteria die zijn gedefinieerd door een klassieke waarschuwing niet meer na een eerder zijn voldaan wordt voldaan.
* **Melding** : de actie op die op basis van een klassieke waarschuwing actief.
* **Actie** -specifieke gebeld verzonden naar een ontvanger van een melding (bijvoorbeeld e-mailen een adres of ze naar een webhook-URL). Meldingen kunnen meestal meerdere acties activeren.

## <a name="how-do-i-receive-a-notification-from-an-azure-monitor-classic-alert"></a>Hoe ontvang ik een melding van een klassieke Azure-monitorwaarschuwing
Waarschuwingen van andere services van Azure gebruikt in het verleden hebben hun eigen ingebouwde notification-methoden. 

Nu u Azure Monitor biedt een herbruikbare melding groeperen aangeroepen *actiegroepen*. Actiegroepen Geef een set van ontvangers voor een melding en elk gewenst moment een waarschuwing is geactiveerd die verwijst naar de groep actie alle ontvangers die meldingen ontvangen. Hiermee kunt u een groepering van ontvangers (bijvoorbeeld uw op aanroep engineer lijst) over veel waarschuwingen objecten hergebruiken. Actie beheergroepen ondersteunen melding door ze naar een webhook-URL naast het e-mailadressen, SMS-nummers en een aantal andere acties.  Zie voor meer informatie [actiegroepen](monitoring-action-groups.md). 

Oudere klassieke activiteitenlogboek waarschuwingen actiegroepen gebruiken.

De oudere metrische waarschuwingen Gebruik echter geen actiegroepen. In plaats daarvan kunt u de volgende acties: 
* E-mailmeldingen verzenden naar de service-beheerder, CO-beheerders of extra e-mailadressen die u opgeeft.
* Aanroepen van een webhook waarmee u kunt aanvullende automation acties te starten.

Webhooks kunt automation en het doorvoeren, bijvoorbeeld met behulp van:
    - Azure Automation Runbook
    - Azure Functions
    - Azure Logic App
    - een service van derden

## <a name="next-steps"></a>Volgende stappen
Informatie ophalen over de regels voor waarschuwingen en ze te configureren met behulp van:

* Meer informatie over [metrische gegevens](monitoring-overview-metrics.md)
* Configureer [klassieke metriek waarschuwingen per Azure-portal](insights-alerts-portal.md)
* Configureer [klassieke metriek waarschuwingen PowerShell](insights-alerts-powershell.md)
* Configureer [klassieke metriek waarschuwingen opdrachtregelinterface (CLI)](insights-alerts-command-line-interface.md)
* Configureer [klassieke metriek waarschuwingen Monitor REST API van Azure](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Meer informatie over [Activity Log](monitoring-overview-activity-logs.md)
* Configureer [activiteit logboek waarschuwingen per Azure-portal](monitoring-activity-log-alerts.md)
* Configureer [activiteit logboek waarschuwingen via Resource Manager](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Controleer de [activiteit logboek waarschuwing webhook schema](monitoring-activity-log-alerts-webhook.md)
* Meer informatie over [actiegroepen](monitoring-action-groups.md)
* Configureer [nieuwere waarschuwingen](monitor-alerts-unified-usage.md)
