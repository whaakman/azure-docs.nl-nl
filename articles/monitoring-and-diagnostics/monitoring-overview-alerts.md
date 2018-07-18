---
title: Overzicht van klassieke waarschuwingen in Microsoft Azure en Azure Monitor
description: Waarschuwingen kunnen u metrische gegevens voor Azure-resources, gebeurtenissen en logboeken controleren en een melding wanneer een bepaalde voorwaarde wordt voldaan.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: robb
ms.component: alerts
ms.openlocfilehash: a0abcbdaa7e998413efb717be6e0addc5607ec5c
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114008"
---
# <a name="what-are-classic-alerts-in-microsoft-azure"></a>Wat zijn klassieke waarschuwingen in Microsoft Azure?

> [!NOTE]
> In dit artikel wordt beschreven hoe u oudere, klassieke metrische waarschuwingen maken. Azure Monitor nu ondersteunt [nieuwere, near-real time metrische waarschuwingen en een nieuwe ervaring voor waarschuwingen](monitoring-overview-unified-alerts.md). 
>

Met behulp van waarschuwingen, voorwaarden kunt configureren voor gegevens en ontvang een melding als de voorwaarden die overeenkomen met de meest recente gegevens.


## <a name="alerts-on-azure-monitor-data"></a>Waarschuwingen van Azure Monitor-gegevens
Er zijn twee soorten klassieke waarschuwingen beschikbaar: metrische waarschuwingen en waarschuwingen voor activiteitenlogboeken.

* **Klassieke metrische waarschuwingen**: deze waarschuwing wordt geactiveerd wanneer de waarde van een opgegeven metrische gegevens van een drempel die u toewijst overschrijden. De waarschuwing genereert een melding wanneer de it de 'geactiveerd' (als de drempelwaarde is overschreden en de waarschuwing voorwaarde wordt voldaan). Het genereert een waarschuwing ook wanneer deze "opgelost wordt" (als de drempelwaarde wordt overschreden, opnieuw en niet meer aan de voorwaarde wordt voldaan). 

* **Waarschuwingen voor klassieke activiteitenlogboeken**: deze waarschuwing voor streaming wordt geactiveerd wanneer het activiteitenlogboek wordt gegenereerd dat overeenkomt met de criteria die u hebt toegewezen filteren. Deze waarschuwingen hebben slechts één status, 'geactiveerd', omdat de waarschuwings-engine de filtercriteria wordt gewoon van toepassing op een nieuwe gebeurtenis. Deze waarschuwingen kunnen u gewaarschuwd wanneer er een nieuw Health Service-incident plaatsvindt of wanneer een gebruiker of toepassing een bewerking uitvoert in uw abonnement, zoals ' virtuele machine verwijderen."

Voor het ontvangen van diagnostische logboekgegevens beschikbaar die is beschikbaar via Azure Monitor, de gegevens in Log Analytics (voorheen Operations Management Suite) route en gebruik van de waarschuwing voor een Log Analytics-query. Meld u nu maakt gebruik van Analytics de [nieuwe waarschuwingen methode](monitoring-overview-unified-alerts.md). 

Het volgende diagram geeft een overzicht van bronnen van de gegevens in Azure Monitor en stelt hoe u kunt waarschuwingen uit deze gegevens.

![Waarschuwingen uitgelegd](./media/monitoring-overview-alerts/Alerts_Overview_Resource_v4.png)

## <a name="taxonomy-of-azure-monitor-alerts-classic"></a>Taxonomie van Azure Monitor-waarschuwingen (klassiek)
Azure maakt gebruik van de volgende termen om te beschrijven van klassieke waarschuwingen en de bijbehorende functies:
* **Waarschuwing**: de definitie van de criteria (een of meer regels of voorwaarden) die wordt geactiveerd wanneer voldaan.
* **Actieve**: de status die wordt uitgevoerd wanneer de criteria die is gedefinieerd in een klassieke waarschuwing wordt voldaan.
* **Opgelost**: de status die wordt uitgevoerd wanneer de criteria die is gedefinieerd in een klassieke waarschuwing niet meer na het eerder zijn voldaan wordt voldaan.
* **Melding**: de actie die moet worden uitgevoerd op basis van wanneer een klassieke waarschuwing geactiveerd wordt.
* **Actie**: de specifieke aanroep die wordt verzonden naar de ontvanger van een melding (bijvoorbeeld een e-mailbericht of een bericht naar een webhook-URL). Meldingen kunnen meestal meerdere acties activeren.

## <a name="how-do-i-receive-notifications-from-an-azure-monitor-classic-alert"></a>Hoe ontvang ik meldingen vanuit een klassieke waarschuwing voor Azure Monitor?
Waarschuwingen van andere services van Azure gebruikt in het verleden, hun eigen methoden voor ingebouwde meldingen. 

Nu Azure Monitor biedt herbruikbare melding groepering met de naam *actiegroepen*. Actiegroepen opgeven een set van ontvangers voor een melding. Wanneer een waarschuwing is geactiveerd die verwijst naar de actiegroep, ontvangen alle ontvangers waarmee de melding. Deze functionaliteit kunt u een groepering van ontvangers (bijvoorbeeld in de lijst op de aanroep engineer) gebruiken voor veel waarschuwingen objecten. Actiegroepen ondersteuning voor melding via verschillende methoden. Deze methoden kunnen plaatsen in een webhook-URL, verzenden van e-mailberichten, SMS-berichten en een aantal andere acties opnemen. Zie voor meer informatie, [maken en beheren van actiegroepen in Azure portal](monitoring-action-groups.md). 

Oudere klassieke activiteitenlogboekalarmen actiegroepen gebruiken.

Echter niet de oudere metrische waarschuwingen actiegroepen gebruiken. In plaats daarvan kunt u de volgende acties: 
* E-mailmeldingen verzenden naar de service-beheerder, CO-beheerders, of naar extra e-mailadressen die u opgeeft.
* Een webhook, waarmee u start automation extra acties kunt aanroepen.

Webhooks inschakelen automatisering en herstel, bijvoorbeeld met behulp van de volgende services:
- Azure Automation Runbook
- Azure Functions
- Logische App van Azure
- Een service van derden

## <a name="next-steps"></a>Volgende stappen
Informatie ophalen over de regels voor waarschuwingen en hoe u ze configureren met behulp van de volgende documentatie:

* Meer informatie over [metrische gegevens](monitoring-overview-metrics.md)
* Configureer [klassieke metrische waarschuwingen met behulp van Azure portal](insights-alerts-portal.md)
* Configureer [klassieke metrische waarschuwingen met behulp van PowerShell](insights-alerts-powershell.md)
* Configureer [klassieke metrische waarschuwingen met behulp van de Azure CLI](insights-alerts-command-line-interface.md)
* Configureer [klassieke metrische waarschuwingen met behulp van de Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Meer informatie over [activiteitenlogboeken](monitoring-overview-activity-logs.md)
* Configureer [waarschuwingen voor activiteitenlogboeken met behulp van Azure portal](monitoring-activity-log-alerts.md)
* Configureer [waarschuwingen voor activiteitenlogboeken met behulp van Resource Manager](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Controleer de [activiteit log waarschuwing webhook-schema](monitoring-activity-log-alerts-webhook.md)
* Meer informatie over [actiegroepen](monitoring-action-groups.md)
* Configureer [nieuwere waarschuwingen](monitor-alerts-unified-usage.md)
