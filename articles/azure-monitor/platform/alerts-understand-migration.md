---
title: Informatie over de werking van het hulpprogramma voor migratie op vrijwillige basis voor Azure Monitor-waarschuwingen
description: Informatie over de werking van het hulpprogramma voor migratie van waarschuwingen en oplossen van problemen.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: b5a13254fc9dfd58db83a1bc8b9dd071cfbbdab2
ms.sourcegitcommit: db3fe303b251c92e94072b160e546cec15361c2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/22/2019
ms.locfileid: "66015596"
---
# <a name="understand-how-the-migration-tool-works"></a>Informatie over de werking van het hulpprogramma voor migratie

Als [eerder aangekondigd](monitoring-classic-retirement.md), klassieke waarschuwingen in Azure Monitor worden stopgezet in September 2019 (oorspronkelijk juli 2019 is). Een hulpprogramma voor migratie is beschikbaar in de Azure-portal voor klanten die gebruik van klassieke waarschuwingsregels en willen voor het activeren van de migratie zelf.

In dit artikel wordt uitgelegd hoe het hulpprogramma voor migratie op vrijwillige basis gebruikt. Hierin wordt ook oplossingen voor enkele veelvoorkomende problemen.

> [!NOTE]
> Vertraging in de uitrol van hulpprogramma voor migratie van de vervaldatum voor de migratie van de klassieke waarschuwingen is vanwege [uitgebreid tot en met 31 augustus 2019](https://azure.microsoft.com/updates/azure-monitor-classic-alerts-retirement-date-extended-to-august-31st-2019/) vanaf de oorspronkelijk aangekondigde datum van 30 juni 2019.

## <a name="which-classic-alert-rules-can-be-migrated"></a>Welke regels voor klassieke waarschuwingen kunnen worden gemigreerd?

Hoewel het hulpprogramma vrijwel alle klassieke waarschuwingsregels migreren kunt, zijn er enkele uitzonderingen. De volgende regels voor waarschuwingen worden niet gemigreerd met behulp van het hulpprogramma (of tijdens de automatische migratie in September 2019):

- Klassieke waarschuwingsregels voor metrische gegevens voor gasten voor virtuele machines (Windows en Linux). Zie de [richtlijnen voor het opnieuw maken van dergelijke regels voor waarschuwingen in nieuwe metrische waarschuwingen](#guest-metrics-on-virtual-machines) verderop in dit artikel.
- Regels voor klassieke waarschuwingen over metrische gegevens van klassieke opslag. Zie de [richtlijnen voor het bewaken van uw klassieke opslagaccounts](https://azure.microsoft.com/blog/modernize-alerting-using-arm-storage-accounts/).
- Klassieke waarschuwingsregels van bepaalde metrische gegevens voor opslagaccount. Zie [details](#storage-account-metrics) verderop in dit artikel.

Als uw abonnement een dergelijke klassieke regels heeft, moet u ze handmatig migreren. Omdat we een automatische migratie niet kan bieden, blijven de bestaande, klassieke metrische waarschuwingen van de volgende typen werken tot en met juni 2020. Deze extensie kunt u tijd voor het overzetten naar nieuwe waarschuwingen. Er zijn geen nieuwe klassieke waarschuwingen kunnen echter worden gemaakt na augustus 2019.

### <a name="guest-metrics-on-virtual-machines"></a>Metrische gegevens voor gasten op virtuele machines

Voordat u nieuwe metrische waarschuwingen op de metrische gegevens voor gasten maken kunt, kan de metrische gegevens voor gasten moeten worden verzonden naar het archief met Azure Monitor aangepaste metrische gegevens. Volg deze instructies voor de Azure Monitor-sink in diagnostische instellingen inschakelen:

- [Metrische gegevens voor gasten voor Windows-VM's inschakelen](collect-custom-metrics-guestos-resource-manager-vm.md)
- [Inschakelen van metrische gegevens voor gasten voor virtuele Linux-machines](collect-custom-metrics-linux-telegraf.md)

Nadat u deze stappen worden uitgevoerd, kunt u nieuwe metrische waarschuwingen maken op de metrische gegevens voor gasten. En nadat u de nieuwe metrische waarschuwingen hebt gemaakt, kunt u klassieke waarschuwingen verwijderen.

### <a name="storage-account-metrics"></a>Metrische gegevens voor opslagaccount

Alle klassieke waarschuwingen op storage-accounts kunnen worden gemigreerd, met uitzondering van waarschuwingen voor deze metrische gegevens:

- PercentAuthorizationError
- PercentClientOtherError
- PercentNetworkError
- PercentServerOtherError
- PercentSuccess
- PercentThrottlingError
- PercentTimeoutError
- AnonymousThrottlingError
- SASThrottlingError

Klassieke waarschuwing in de regels voor percentage metrische gegevens moeten worden gemigreerd op basis van [de toewijzing tussen metrische gegevens van oude en nieuwe storage](https://docs.microsoft.com/azure/storage/common/storage-metrics-migration#metrics-mapping-between-old-metrics-and-new-metrics). Drempelwaarden moet op de juiste wijze worden gewijzigd omdat de nieuwe metrische gegevens die beschikbaar een absolute is.

Klassieke waarschuwingsregels op AnonymousThrottlingError en SASThrottlingError moeten worden gesplitst in twee nieuwe waarschuwingen, omdat er geen gecombineerde metric die dezelfde functionaliteit biedt. Drempelwaarden moet op de juiste wijze worden aangepast.

## <a name="rollout-phases"></a>Implementatiefasen

Het Migratiehulpmiddel is beschikbaar in fasen voor klanten die gebruikmaken van klassieke waarschuwingsregels. Eigenaars van abonnementen ontvangt een e-mailbericht wanneer het abonnement gereed om te worden gemigreerd is met behulp van het hulpprogramma.

> [!NOTE]
> Omdat het hulpprogramma wordt in fasen wordt geïmplementeerd, ziet u mogelijk dat de meeste van uw abonnementen niet nog klaar bent om tijdens de eerste fasen worden gemigreerd.

Een subset van abonnementen is momenteel gemarkeerd als gereed voor migratie. De subset, bevat deze abonnementen waarvoor de regels voor klassieke waarschuwingen alleen op de volgende resourcetypen. Ondersteuning voor meer resourcetypen wordt in fasen van de toekomstige toegevoegd.

- Microsoft.apimanagement/service
- Microsoft.batch/batchaccounts
- Microsoft.cache/redis
- Microsoft.datafactory/datafactories
- Microsoft.dbformysql/servers
- Microsoft.dbforpostgresql/servers
- Microsoft.eventhub/namespaces
- Microsoft.Logic/workflows
- Microsoft.network/applicationgateways
- Microsoft.network/dnszones
- Microsoft.network/expressroutecircuits
- Microsoft.network/loadbalancers
- Microsoft.network/networkwatchers/connectionmonitors
- Microsoft.network/publicipaddresses
- Microsoft.network/trafficmanagerprofiles
- Microsoft.network/virtualnetworkgateways
- Microsoft.search/searchservices
- Microsoft.servicebus/namespaces
- Microsoft.streamanalytics/streamingjobs
- Microsoft.timeseriesinsights/environments
- Microsoft.web/hostingenvironments/workerpools
- Microsoft.web/serverfarms
- Microsoft.web/sites

## <a name="who-can-trigger-the-migration"></a>Die de migratie kunt activeren?

Elke gebruiker die de ingebouwde rol van Inzender controleren op het abonnementsniveau van het kan de migratie te activeren. De migratie kunnen ook worden geactiveerd door gebruikers die een aangepaste rol met de volgende machtigingen hebben:

- * / lezen
- Microsoft.Insights/actiongroups/*
- Microsoft.Insights/AlertRules/*
- Microsoft.Insights/metricAlerts/*

## <a name="common-problems-and-remedies"></a>Veelvoorkomende problemen en oplossingen

Nadat u [activeren van de migratie](alerts-using-migration-tool.md), ontvangt u e-mailadres op de adressen die u hebt opgegeven om u te waarschuwen dat de migratie voltooid is of als geen actie te ondernemen. Deze sectie beschrijft enkele veelvoorkomende problemen en hoe u omgaat met hen.

### <a name="validation-failed"></a>Validatie is mislukt

Door een recente wijzigingen in de klassieke waarschuwingsregels in uw abonnement, kan niet het abonnement worden gemigreerd. Dit probleem is tijdelijk. U kunt de migratie opnieuw opstarten nadat de migratiestatus van de wordt teruggeplaatst **gereed voor migratie** in een paar dagen.

### <a name="policy-or-scope-lock-preventing-us-from-migrating-your-rules"></a>Beleid of bereik vergrendelen, waardoor uw regels migreren

Als onderdeel van de migratie, nieuwe metrische waarschuwingen en nieuwe actiegroepen worden gemaakt en vervolgens regels voor klassieke waarschuwingen worden verwijderd. Er is echter een beleid of een bereik vergrendelen, waardoor het maken van resources. Sommige of alle regels kunnen niet worden gemigreerd, afhankelijk van de vergrendeling beleid of bereik. U kunt dit probleem oplossen door tijdelijk verwijderen van de scope vergrendelen of het beleid en het activeren van de migratie opnieuw uit.

## <a name="next-steps"></a>Volgende stappen

- [Het gebruik van het hulpprogramma voor migratie](alerts-using-migration-tool.md)
- [Voorbereiden voor de migratie](alerts-prepare-migration.md)
