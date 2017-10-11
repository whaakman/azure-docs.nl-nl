---
title: De Azure Activity Log naar Event Hubs Stream | Microsoft Docs
description: Informatie over het streamen van de Azure Activity Log naar Event Hubs.
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ec4c2d2c-8907-484f-a910-712403a06829
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 6/06/2017
ms.author: johnkem
ms.openlocfilehash: 88c5701279f370914fac68872d67b02a7571748a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Stream de Azure Activity Log naar Event Hubs
De [ **Azure Activity Log** ](monitoring-overview-activity-logs.md) kan worden gestreamd in bijna realtime voor elke toepassing met behulp van de ingebouwde optie 'Exporteren' in de portal of doordat de Service Bus regel-Id in een logboek profiel via de Azure PowerShell-Cmdlets of Azure CLI.

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Wat u kunt doen met de activiteitenlogboek en Event Hubs
Hier volgen slechts enkele manieren waarop u de streaming-mogelijkheden kunt gebruiken voor het logboek:

* **Stream met de logboekregistratie en telemetrie systemen van derden** – gedurende een periode, streaming van Event Hubs, raken de methode om uw activiteitenlogboek doorsluizen naar siem's van derden en meld u analytics-oplossingen.
* **Maken van een aangepaste Telemetrie en logboekregistratie platform** : als u al hebt een op maat gemaakte telemetrie platform of de zijn alleen nadenkt over het bouwen van een uiterst schaalbare voor publiceren / abonneren aard van Event Hubs kunt u het activiteitenlogboek flexibel opnemen. [Zie de Dan Rosanova-handleiding voor het gebruik van Event Hubs in een hier wereldwijde schaal telemetrie-platform.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-the-activity-log"></a>Streaming van het activiteitenlogboek activeren
U kunt het inschakelen van het activiteitenlogboek streaming via programmacode of via de portal. In beide gevallen moet u kiest een Service Bus-Namespace en een gedeeld toegangsbeleid voor die naamruimte en een Event Hub wordt gemaakt in die naamruimte als de eerste nieuwe activiteitenlogboek gebeurtenis plaatsvindt. Als u een Service Bus-Namespace niet hebt, moet u eerst een maken. Als u hebt eerder gestreamd activiteitenlogboek van gebeurtenissen naar deze Service Bus-Namespace, wordt opnieuw gebruikt de Event Hub die eerder is gemaakt. Het beleid voor gedeelde toegang definieert de machtigingen van het mechanisme voor streaming. Vandaag de dag streaming naar een Event Hubs vereist **beheren**, **verzenden**, en **luisteren** machtigingen. U kunt maken of wijzigen van Service Bus Namespace gedeeld toegangsbeleid in de klassieke portal onder het tabblad 'Configureren' voor uw Service Bus-Namespace. Voor het bijwerken van het profiel van het logboek activiteitenlogboek om op te nemen streaming, moet de gebruiker wijziging aan te brengen de machtiging ListKey hebben op die Service Bus-autorisatieregel.

De service bus of event hub-naamruimte heeft geen zich in hetzelfde abonnement als het abonnement dat Logboeken, zolang de gebruiker die de instelling configureert juiste RBAC toegang tot beide abonnementen heeft.

### <a name="via-azure-portal"></a>Via de Azure-portal
1. Navigeer naar de **activiteitenlogboek** blade via het menu aan de linkerkant van de portal.
   
    ![Navigeer naar activiteitenlogboek in portal](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klik op de **exporteren** knop aan de bovenkant van de blade.
   
    ![Knop exporteren in de portal](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. In de blade die wordt weergegeven, kunt u de regio's waarvoor u wilt stroom gebeurtenissen en de Service Bus-Namespace waarin u een Event Hub wordt gemaakt voor het streamen van deze gebeurtenissen wilt selecteren.
   
    ![Activiteitenlogboek blade exporteren](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klik op **opslaan** deze instellingen op te slaan. De instellingen zijn onmiddellijk worden toegepast op uw abonnement.

### <a name="via-powershell-cmdlets"></a>Via PowerShell-Cmdlets
Als een profiel voor een logboek al bestaat, moet u eerst dat profiel te verwijderen.

1. Gebruik `Get-AzureRmLogProfile` om te bepalen of er een logboek-profiel bestaat
2. Als dit het geval is, gebruik `Remove-AzureRmLogProfile` om deze te verwijderen.
3. Gebruik `Set-AzureRmLogProfile` om een profiel te maken:

```
Add-AzureRmLogProfile -Name my_log_profile -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

De regel-ID van Service Bus is een tekenreeks met deze indeling: {service bus resource ID} /authorizationrules/ {sleutelnaam}, bijvoorbeeld 

### <a name="via-azure-cli"></a>Via Azure CLI
Als een profiel voor een logboek al bestaat, moet u eerst dat profiel te verwijderen.

1. Gebruik `azure insights logprofile list` om te bepalen of er een logboek-profiel bestaat
2. Als dit het geval is, gebruik `azure insights logprofile delete` om deze te verwijderen.
3. Gebruik `azure insights logprofile add` om een profiel te maken:

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

De regel-ID van Service Bus is een tekenreeks met deze indeling: `{service bus resource ID}/authorizationrules/{key name}`.

## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Hoe ik de logboekgegevens van Event Hubs verbruiken?
[Het schema voor het logboek is hier beschikbaar](monitoring-overview-activity-logs.md). Elke gebeurtenis wordt in een matrix met JSON-blobs aangeroepen "records."

## <a name="next-steps"></a>Volgende stappen
* [Het activiteitenlogboek naar een opslagaccount archiveren](monitoring-archive-activity-log.md)
* [Lees het overzicht van de Azure Activity Log](monitoring-overview-activity-logs.md)
* [Een waarschuwing op basis van een gebeurtenis activiteitenlogboek instellen](insights-auditlog-to-webhook-email.md)

