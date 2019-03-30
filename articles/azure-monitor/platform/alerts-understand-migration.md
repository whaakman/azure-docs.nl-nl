---
title: Hulpprogramma voor migratie van hoe vrijwillig waarschuwingen in Azure Monitor werkt begrijpen
description: Informatie over de werking van het hulpprogramma voor migratie van waarschuwingen en problemen oplossen als u problemen ondervindt.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: a45a0cff606bc854924d5da0841b26e1cb9031bb
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58632127"
---
# <a name="understand-how-the-migration-tool-works"></a>Informatie over de werking van het hulpprogramma voor migratie

Als [eerder aangekondigd](monitoring-classic-retirement.md), klassieke waarschuwingen in Azure Monitor worden stopgezet in juli 2019. Het hulpprogramma voor migratie voor het activeren van migratie vrijwillig is beschikbaar in Azure portal en is beschikbaar voor klanten die gebruikmaken van klassieke waarschuwingsregels.

In dit artikel begeleidt u door de werking van het hulpprogramma voor migratie op vrijwillige basis. Er wordt ook beschreven herstel voor enkele veelvoorkomende problemen.

## <a name="which-classic-alert-rules-can-be-migrated"></a>Welke regels voor klassieke waarschuwingen kunnen worden gemigreerd?

Bijna alle regels voor klassieke waarschuwingen kunnen worden gemigreerd met behulp van het hulpprogramma, maar er zijn enkele uitzonderingen. De volgende regels voor waarschuwingen worden niet gemigreerd met behulp van het hulpprogramma (of tijdens de automatische migratie in juli 2019)

- Regels voor klassieke waarschuwingen op de virtuele machine metrische gegevens voor gasten (Windows en Linux). [Meer informatie over hoe u opnieuw maken van dergelijke regels voor waarschuwingen in nieuwe metrische waarschuwingen](#guest-metrics-on-virtual-machines)
- Regels voor klassieke waarschuwingen over metrische gegevens van klassieke opslag. [Meer informatie over het bewaken van uw klassieke opslagaccounts](https://azure.microsoft.com/blog/modernize-alerting-using-arm-storage-accounts/)
- Klassieke waarschuwingsregels van bepaalde metrische gegevens voor opslagaccount. [Hieronder voor meer informatie](#storage-account-metrics)

Als uw abonnement een dergelijke klassieke regels heeft, de rest van de regels worden gemigreerd, maar deze regels moet handmatig worden gemigreerd. Als we een automatische migratie niet kan bieden, blijft deze bestaande klassieke metrische waarschuwingen geprobeerd van juni 2020 zodat u tijd voor het overzetten naar nieuwe waarschuwingen. Echter kunnen geen nieuwe klassieke waarschuwingen worden gemaakt na juni 2019.

### <a name="guest-metrics-on-virtual-machines"></a>Metrische gegevens voor gasten op virtuele machines

Om te kunnen maken van nieuwe metrische waarschuwingen over metrische gegevens voor gasten, worden de metrische gegevens voor gasten moet worden verzonden naar het archief met Azure Monitor aangepaste metrische gegevens. Volg de onderstaande instructies om in te schakelen van de Azure Monitor-sink in diagnostische instellingen.

- [Metrische gegevens voor gasten voor Windows-VM's inschakelen](collect-custom-metrics-guestos-resource-manager-vm.md)
- [Inschakelen van metrische gegevens voor gasten voor virtuele Linux-machines](https://docs.microsoft.com/azure/azure-monitor/platform/collect-custom-metrics-linux-telegraf)

Zodra de bovenstaande stappen worden uitgevoerd, kunnen nieuwe metrische waarschuwingen worden gemaakt op de metrische gegevens voor gasten. Wanneer u nieuwe metrische waarschuwingen hebt gemaakt, kan de klassieke waarschuwingen kunnen worden verwijderd.

### <a name="storage-account-metrics"></a>Metrische gegevens voor opslagaccount

Alle klassieke waarschuwingen op storage-accounts kunnen worden gemigreerd, met uitzondering van die waarschuwingen op de volgende metrische gegevens:

- PercentAuthorizationError
- PercentClientOtherError
- PercentNetworkError
- PercentServerOtherError
- PercentSuccess
- PercentThrottlingError
- PercentTimeoutError
- AnonymousThrottlingError
- SASThrottlingError

Klassieke waarschuwingsregels op percentage metrische gegevens moeten worden gemigreerd op basis van [de toewijzing tussen metrische gegevens van oude en nieuwe storage](https://docs.microsoft.com/azure/storage/common/storage-metrics-migration#metrics-mapping-between-old-metrics-and-new-metrics). Drempelwaarden moet op de juiste wijze worden gewijzigd omdat de nieuwe metrische gegevens die beschikbaar een absolute is.

Klassieke waarschuwingsregels AnonymousThrottlingError en SASThrottlingError moet worden gesplitst in twee nieuwe waarschuwingen als er is geen gecombineerde metric die biedt dezelfde functionaliteit. Drempelwaarden moet op de juiste wijze worden aangepast.

## <a name="roll-out-phases"></a>Uitrol fasen

Het Migratiehulpmiddel is beschikbaar in fasen voor klanten die gebruikmaken van klassieke waarschuwingsregels. **Eigenaars van abonnementen** ontvangt een e-mail wanneer het abonnement is gereed om te worden gemigreerd met behulp van het hulpprogramma.

> [!NOTE]
> Als het hulpprogramma is geïmplementeerd in fasen, in de eerste fasen, ziet u mogelijk dat de meeste van uw abonnementen zijn nog niet klaar om te worden gemigreerd.

Op dit moment een **subset** van abonnementen, die **alleen** klassieke waarschuwingsregels hebben voor de volgende resource gegevenstypen zijn gemarkeerd als gereed voor migratie. Ondersteuning voor meer resourcetypen wordt in fasen van de toekomstige toegevoegd.

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

Elke gebruiker die de ingebouwde rol van **bewaking Inzender** op het abonnement niveau is het mogelijk voor het activeren van de migratie. De migratie kunnen ook worden geactiveerd door gebruikers met een aangepaste rol met de volgende machtigingen:

- * / lezen
- Microsoft.Insights/actiongroups/*
- Microsoft.Insights/AlertRules/*
- Microsoft.Insights/metricAlerts/*

## <a name="common-issues-and-remediations"></a>Veelvoorkomende problemen en herstelbewerkingen

Nadat u [activeren van de migratie](alerts-using-migration-tool.md), zullen worden gebruikt voor het e-mailadressen die u waarschuwen over voltooiing van de migratie of als er een actie te ondernemen is. Het volgende gedeelte bevat enkele veelvoorkomende problemen en hoe ze te herstellen

### <a name="validation-failed"></a>Validatie is mislukt

Door een recente wijzigingen in de klassieke waarschuwingsregels in uw abonnement, kan niet het abonnement worden gemigreerd. Dit is een tijdelijk probleem. U kunt de migratie opnieuw starten wanneer de migratiestatus van de wordt teruggeplaatst **gereed voor migratie** in een paar dagen.

### <a name="policyscope-lock-preventing-us-from-migrating-your-rules"></a>Vergrendeling van beleid of het bereik waardoor uw regels migreren

Nieuwe metrische waarschuwingen en nieuwe actiegroepen worden gemaakt als onderdeel van de migratie en klassieke waarschuwingsregels worden verwijderd (zodra er nieuwe regels worden gemaakt). Er is echter een beleid of een bereik vergrendelen, waardoor het maken van resources. Sommige of alle regels kunnen niet worden gemigreerd, afhankelijk van de vergrendeling beleid of bereik. U kunt dit probleem oplossen door het verwijderen van het bereik van de vergrendeling/beleid tijdelijk en activeren van de migratie opnieuw uit.

## <a name="next-steps"></a>Volgende stappen

- [Het gebruik van het hulpprogramma voor migratie](alerts-using-migration-tool.md)
- [Voorbereiden voor migratie](alerts-prepare-migration.md)
