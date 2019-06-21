---
title: Informatie over de werking van het hulpprogramma voor migratie op vrijwillige basis voor Azure Monitor-waarschuwingen
description: Informatie over de werking van het hulpprogramma voor migratie van waarschuwingen en oplossen van problemen.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 015000388c5629dbd8ed8833931a809ebd738bd6
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295528"
---
# <a name="understand-how-the-migration-tool-works"></a>Informatie over de werking van het hulpprogramma voor migratie

Als [eerder aangekondigd](monitoring-classic-retirement.md), klassieke waarschuwingen in Azure Monitor worden stopgezet op 31 augustus 2019 (oorspronkelijk 30 juni 2019 is). Een hulpprogramma voor migratie is beschikbaar in de Azure-portal voor klanten die gebruik van klassieke waarschuwingsregels en willen voor het activeren van de migratie zelf.

In dit artikel wordt uitgelegd hoe het hulpprogramma voor migratie op vrijwillige basis gebruikt. Hierin wordt ook oplossingen voor enkele veelvoorkomende problemen.

> [!NOTE]
> Vertraging in de uitrol van hulpprogramma voor migratie van de vervaldatum voor de migratie van de klassieke waarschuwingen is vanwege [uitgebreid tot en met 31 augustus 2019](https://azure.microsoft.com/updates/azure-monitor-classic-alerts-retirement-date-extended-to-august-31st-2019/) vanaf de oorspronkelijk aangekondigde datum van 30 juni 2019.

## <a name="classic-alert-rules-that-will-not-be-migrated"></a>Klassieke waarschuwingsregels die niet worden gemigreerd

> [!IMPORTANT]
> Waarschuwingen voor activiteitenlogboeken (inclusief Service health waarschuwingen) en waarschuwingen worden niet beïnvloed door de migratie. De migratie is alleen van toepassing op klassieke waarschuwingsregels beschreven [hier](monitoring-classic-retirement.md#retirement-of-classic-monitoring-and-alerting-platform).

Hoewel het hulpprogramma bijna alle migreren kunt [klassieke waarschuwingsregels](monitoring-classic-retirement.md#retirement-of-classic-monitoring-and-alerting-platform), er zijn enkele uitzonderingen. De volgende regels voor waarschuwingen worden niet gemigreerd met behulp van het hulpprogramma (of tijdens de automatische migratie vanaf September 2019):

- Klassieke waarschuwingsregels voor metrische gegevens voor gasten voor virtuele machines (Windows en Linux). Zie de [richtlijnen voor het opnieuw maken van dergelijke regels voor waarschuwingen in nieuwe metrische waarschuwingen](#guest-metrics-on-virtual-machines) verderop in dit artikel.
- Regels voor klassieke waarschuwingen over metrische gegevens van klassieke opslag. Zie de [richtlijnen voor het bewaken van uw klassieke opslagaccounts](https://azure.microsoft.com/blog/modernize-alerting-using-arm-storage-accounts/).
- Klassieke waarschuwingsregels van bepaalde metrische gegevens voor opslagaccount. Zie [details](#storage-account-metrics) verderop in dit artikel.
- Regels voor klassieke waarschuwingen op sommige metrische gegevens van Cosmos DB. Details worden toegevoegd in een toekomstige update.

Als uw abonnement een dergelijke klassieke regels heeft, moet u ze handmatig migreren. Omdat we een automatische migratie niet kan bieden, blijven de bestaande, klassieke metrische waarschuwingen van de volgende typen werken tot en met juni 2020. Deze extensie kunt u tijd voor het overzetten naar nieuwe waarschuwingen. Er zijn geen nieuwe klassieke waarschuwingen kunnen echter worden gemaakt na augustus 2019.

> [!NOTE]
> Naast de hierboven vermelde uitzonderingen, als uw klassieke waarschuwingsregels ongeldig zijn dat wil zeggen ze zijn verbonden [afgeschafte metrische gegevens](#classic-alert-rules-on-deprecated-metrics) of resources die zijn verwijderd, worden ze niet gemigreerd tijdens de migratie op vrijwillige basis. Een dergelijke ongeldig klassieke waarschuwingsregels worden verwijderd als automatische migratie plaatsvindt.

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
- ThrottlingError

Klassieke waarschuwing in de regels voor percentage metrische gegevens moeten worden gemigreerd op basis van [de toewijzing tussen metrische gegevens van oude en nieuwe storage](https://docs.microsoft.com/azure/storage/common/storage-metrics-migration#metrics-mapping-between-old-metrics-and-new-metrics). Drempelwaarden moet op de juiste wijze worden gewijzigd omdat de nieuwe metrische gegevens die beschikbaar een absolute is.

Klassieke waarschuwingsregels op AnonymousThrottlingError, SASThrottlingError en ThrottlingError moeten worden gesplitst in twee nieuwe waarschuwingen, omdat er geen gecombineerde metric die dezelfde functionaliteit biedt. Drempelwaarden moet op de juiste wijze worden aangepast.

### <a name="classic-alert-rules-on-deprecated-metrics"></a>Regels voor klassieke waarschuwingen over afgeschafte metrische gegevens

Dit zijn de regels voor klassieke waarschuwingen over metrische gegevens die eerder werden ondersteund, maar uiteindelijk zijn afgeschaft. Een klein percentage van de klant mogelijk ongeldige klassieke waarschuwingsregels op deze metrische gegevens. Omdat deze waarschuwingsregels ongeldig zijn, ook niet worden ze gemigreerd.

| Resourcetype| Afgeschafte metrische waarde(n) |
|-------------|----------------- |
| Microsoft.DBforMySQL/servers | compute_consumption_percent, compute_limit |
| Microsoft.DBforPostgreSQL/servers | compute_consumption_percent, compute_limit |
| Microsoft.Network/publicIPAddresses | defaultddostriggerrate |
| Microsoft.SQL/servers/databases | service_level_objective, storage_limit, storage_used, beperking, dtu_consumption_percent, storage_used |
| Microsoft.Web/hostingEnvironments/multirolepools | averagememoryworkingset |
| Microsoft.Web/hostingEnvironments/workerpools | bytesreceived, httpqueuelength |

## <a name="how-equivalent-new-alert-rules-and-action-groups-are-created"></a>Hoe gelijkwaardige nieuwe regels voor waarschuwingen en actiegroepen worden gemaakt

Het hulpprogramma voor migratie omgezet in de klassieke waarschuwingsregels gelijkwaardige nieuwe regels voor waarschuwingen en Actiegroepen. Voor de meeste klassieke waarschuwingsregels gelijkwaardige regels voor nieuwe waarschuwingen zijn op de dezelfde metrische gegevens met dezelfde eigenschappen zoals `windowSize` en `aggregationType`. Er zijn echter enkele klassieke waarschuwing regels zijn op de metrische gegevens die een andere, gelijkwaardige metrische gegevens in het nieuwe systeem hebben. De volgende principes gelden voor de migratie van klassieke waarschuwingen, tenzij opgegeven in de onderstaande sectie:

- **Frequentie**: Bepaalt hoe vaak een klassieke of nieuwe waarschuwingsregel voor de voorwaarde wordt gecontroleerd. De `frequency` in klassieke waarschuwingsregels is niet kunnen worden geconfigureerd door de gebruiker en het is altijd 5 minuten om alle brontypen behalve Application Insights-onderdelen waarvan er 1 minuut. Dus is de frequentie van gelijkwaardige regels ook ingesteld op 5 minuten en 1 minuut respectievelijk.
- **Aggregatietype**: Hiermee definieert u hoe de metrische gegevens via het venster van belang zijn worden samengevoegd. De `aggregationType` is ook hetzelfde tussen klassieke waarschuwingen en nieuwe waarschuwingen voor de meeste metrische gegevens. In sommige gevallen, omdat de metrische gegevens tussen het klassieke waarschuwingen en nieuwe waarschuwingen, gelijkwaardige is `aggregationType` of de `primary Aggregation Type` gedefinieerd voor de metriek wordt gebruikt.
- **Eenheden**: Eigenschap van de metrische waarde waarop de waarschuwing wordt gemaakt. Sommige gelijkwaardige metrische gegevens hebben verschillende eenheden. De drempelwaarde wordt op de juiste wijze indien nodig worden aangepast. Bijvoorbeeld, als de oorspronkelijke metriek seconden als eenheid heeft maar gelijkwaardige nieuwe metrische gegevens milliseconden als eenheden, wordt de oorspronkelijke drempelwaarde vermenigvuldigd met 1000 om te controleren of hetzelfde gedrag.
- **Venstergrootte**: Hiermee definieert u het venster via welke metrische gegevens worden samengevoegd als u wilt vergelijken met de drempelwaarde. Voor standard `windowSize` waarden, zoals 5mins, 15mins, 30 minuten voordat, 1 uur, 3 uur, 6 uur, 12 uur, dag, er is geen wijziging aangebracht voor equivalente nieuwe waarschuwingsregel. Voor andere waarden de dichtstbijzijnde `windowSize` is gekozen om te worden gebruikt. Er zijn geen gevolgen deze wijziging voor de meeste klanten. Voor een klein percentage van klanten, is het mogelijk dat er een nodig de drempelwaarde voor exact hetzelfde gedrag aanpassen.

In de volgende secties gedetailleerd we de metrische gegevens die een andere, gelijkwaardige metrische gegevens in het nieuwe systeem hebben beschreven. Elke meetwaarde die hetzelfde voor klassieke en nieuwe regels voor waarschuwingen blijft niet wordt vermeld. U vindt een lijst met metrische gegevens die worden ondersteund in het nieuwe systeem [hier](metrics-supported.md).

### <a name="microsoftstorageaccountsservices"></a>Microsoft.StorageAccounts/services

De volgende metrische gegevens zijn toegewezen aan gelijkwaardige metrische gegevens voor Storage-account-services, zoals de blob-, tabel-, bestands- en wachtrij, zoals hieronder wordt weergegeven:

| Metrische gegevens in de klassieke waarschuwingen | Gelijkwaardige metrische factor bij het nieuwe waarschuwingen | Opmerkingen|
|--------------------------|---------------------------------|---------|
| AnonymousAuthorizationError| Metrische gegevens van transacties met dimensies 'ResponseType' = 'AuthorizationError' en 'Authentication' = 'Anoniem'| |
| AnonymousClientOtherError | Metrische gegevens van transacties met dimensies 'ResponseType' = 'ClientOtherError' en 'Authentication' = 'Anoniem' | |
| AnonymousClientTimeOutError| Metrische gegevens van transacties met dimensies 'ResponseType' = 'ClientTimeOutError' en 'Authentication' = 'Anoniem' | |
| AnonymousNetworkError | Metrische gegevens van transacties met dimensies 'ResponseType' = 'NetworkError' en 'Authentication' = 'Anoniem' | |
| AnonymousServerOtherError | Metrische gegevens van transacties met dimensies 'ResponseType' = 'ServerOtherError' en 'Authentication' = 'Anoniem' | |
| AnonymousServerTimeOutError | Metrische gegevens van transacties met dimensies 'ResponseType' = 'ServerTimeOutError' en 'Authentication' = 'Anoniem' | |
| AnonymousSuccess | Metrische gegevens van transacties met dimensies "ResponseType" = "Geslaagd" en 'Verificatie' = 'Anoniem' | |
| AuthorizationError | Metrische gegevens van transacties met dimensies "ResponseType" = "AuthorizationError" | |
| AverageE2ELatency | SuccessE2ELatency | |
| AverageServerLatency | SuccessServerLatency | |
| Capaciteit | BlobCapacity | Gebruik `aggregationType` gemiddelde in plaats van 'laatste'. Metrische gegevens is alleen van toepassing op Blob-services |
| ClientOtherError | Metrische gegevens van transacties met dimensies "ResponseType" = "ClientOtherError"  | |
| ClientTimeoutError | Metrische gegevens van transacties met dimensies "ResponseType" = "ClientTimeOutError" | |
| ContainerCount | ContainerCount | Gebruik `aggregationType` gemiddelde in plaats van 'laatste'. Metrische gegevens is alleen van toepassing op Blob-services |
| NetworkError | Metrische gegevens van transacties met dimensies "ResponseType" = "NetworkError" | |
| ObjectCount | BlobCount| Gebruik `aggregationType` gemiddelde in plaats van 'laatste'. Metrische gegevens is alleen van toepassing op Blob-services |
| SASAuthorizationError | Metrische gegevens van transacties met dimensies "ResponseType" = "AuthorizationError" en "Authentication" = "SAS" | |
| SASClientOtherError | Metrische gegevens van transacties met dimensies "ResponseType" = "ClientOtherError" en "Authentication" = "SAS" | |
| SASClientTimeOutError | Metrische gegevens van transacties met dimensies "ResponseType" = "ClientTimeOutError" en "Authentication" = "SAS" | |
| SASNetworkError | Metrische gegevens van transacties met dimensies "ResponseType" = "NetworkError" en "Authentication" = "SAS" | |
| SASServerOtherError | Metrische gegevens van transacties met dimensies "ResponseType" = "ServerOtherError" en "Authentication" = "SAS" | |
| SASServerTimeOutError | Metrische gegevens van transacties met dimensies "ResponseType" = "ServerTimeOutError" en "Authentication" = "SAS" | |
| SASSuccess | Metrische gegevens van transacties met dimensies "ResponseType" = "Geslaagd" en "Authentication" = "SAS" | |
| ServerOtherError | Metrische gegevens van transacties met dimensies "ResponseType" = "ServerOtherError" | |
| ServerTimeOutError | Metrische gegevens van transacties met dimensies "ResponseType" = "ServerTimeOutError"  | |
| Geslaagd | Metrische gegevens van transacties met dimensies "ResponseType" = "Geslaagd" | |
| TotalBillableRequests| Transacties | |
| TotalEgress | Uitgaand verkeer | |
| TotalIngress | Inkomend verkeer | |
| TotalRequests | Transacties | |

### <a name="microsoftinsightscomponents"></a>Microsoft.insights/components

Gelijkwaardige metrische gegevens zijn voor Application Insights, zoals hieronder wordt weergegeven:

| Metrische gegevens in de klassieke waarschuwingen | Gelijkwaardige metrische factor bij het nieuwe waarschuwingen | Opmerkingen|
|--------------------------|---------------------------------|---------|
| availability.availabilityMetric.value | availabilityResults/availabilityPercentage|   |
| availability.durationMetric.value | availabilityResults/duur| Vermenigvuldigt oorspronkelijke drempelwaarde met 1000 eenheden voor klassieke metrische gegevens zijn in een paar seconden en voor nieuwe een worden weergegeven in milliseconden.  |
| basicExceptionBrowser.count | uitzonderingen/de browser|  Gebruik `aggregationType` 'count' in plaats van 'sum'. |
| basicExceptionServer.count | uitzonderingen of de server| Gebruik `aggregationType` 'count' in plaats van 'sum'.  |
| clientPerformance.clientProcess.value | browserTimings/processingDuration| Vermenigvuldigt oorspronkelijke drempelwaarde met 1000 eenheden voor klassieke metrische gegevens zijn in een paar seconden en voor nieuwe een worden weergegeven in milliseconden.  |
| clientPerformance.networkConnection.value | browserTimings/networkDuration|  Vermenigvuldigt oorspronkelijke drempelwaarde met 1000 eenheden voor klassieke metrische gegevens zijn in een paar seconden en voor nieuwe een worden weergegeven in milliseconden. |
| clientPerformance.receiveRequest.value | browserTimings/receiveDuration| Vermenigvuldigt oorspronkelijke drempelwaarde met 1000 eenheden voor klassieke metrische gegevens zijn in een paar seconden en voor nieuwe een worden weergegeven in milliseconden.  |
| clientPerformance.sendRequest.value | browserTimings/sendDuration| Vermenigvuldigt oorspronkelijke drempelwaarde met 1000 eenheden voor klassieke metrische gegevens zijn in een paar seconden en voor nieuwe een worden weergegeven in milliseconden.  |
| clientPerformance.total.value | browserTimings/totalDuration| Vermenigvuldigt oorspronkelijke drempelwaarde met 1000 eenheden voor klassieke metrische gegevens zijn in een paar seconden en voor nieuwe een worden weergegeven in milliseconden.  |
| performanceCounter.available_bytes.value | performanceCounters/memoryAvailableBytes|   |
| performanceCounter.io_data_bytes_per_sec.value | performanceCounters/processIOBytesPerSecond|   |
| performanceCounter.number_of_exceps_thrown_per_sec.value | performanceCounters/exceptionsPerSecond|   |
| performanceCounter.percentage_processor_time_normalized.value | performanceCounters/processCpuPercentage|   |
| performanceCounter.percentage_processor_time.value | performanceCounters/processCpuPercentage| Drempel moet op de juiste wijze worden gewijzigd als de oorspronkelijke metriek is over alle cores en nieuwe metrische gegevens op één kern is genormaliseerd. Hulpprogramma voor migratie verandert niet drempelwaarden.  |
| performanceCounter.percentage_processor_total.value | performanceCounters/processorCpuPercentage|   |
| performanceCounter.process_private_bytes.value | performanceCounters/processPrivateBytes|   |
| performanceCounter.request_execution_time.value | performanceCounters/requestExecutionTime|   |
| performanceCounter.requests_in_application_queue.value | performanceCounters/requestsInQueue|   |
| performanceCounter.requests_per_sec.value | performanceCounters/requestsPerSecond|   |
| Request.duration | aanvragen/duur| Vermenigvuldigt oorspronkelijke drempelwaarde met 1000 eenheden voor klassieke metrische gegevens zijn in een paar seconden en voor nieuwe een worden weergegeven in milliseconden.  |
| Request.rate | aanvragen/snelheid|   |
| requestFailed.count | mislukte aanvragen /| Gebruik `aggregationType` 'count' in plaats van 'sum'.   |
| view.count | pageViews/count| Gebruik `aggregationType` 'count' in plaats van 'sum'.   |

### <a name="how-equivalent-action-groups-are-created"></a>Hoe gelijkwaardige actiegroepen worden gemaakt

Klassieke waarschuwing regels waren e-mail, webhook, logische app en runbook acties gekoppeld aan de waarschuwing regel zelf. Nieuwe regels waarschuwingen gebruik actiegroepen die kunnen worden hergebruikt voor meerdere regels voor waarschuwingen. Het hulpprogramma voor migratie maakt één actiegroep voor dezelfde acties, ongeacht het aantal regels voor waarschuwingen zijn met behulp van de actie. Actiegroepen gemaakt door het hulpprogramma voor migratie gebruiken de naamgevingsindeling 'Migrated_AG *'.

## <a name="rollout-phases"></a>Implementatiefasen

Het Migratiehulpmiddel is beschikbaar in fasen voor klanten die gebruikmaken van klassieke waarschuwingsregels. Eigenaars van abonnementen ontvangt een e-mailbericht wanneer het abonnement gereed om te worden gemigreerd is met behulp van het hulpprogramma.

> [!NOTE]
> Omdat het hulpprogramma wordt in fasen wordt geïmplementeerd, ziet u mogelijk dat sommige van uw abonnementen niet nog klaar bent om tijdens de eerste fasen worden gemigreerd.

De meeste van de abonnementen zijn momenteel gemarkeerd als gereed voor migratie. Alleen abonnementen met klassieke waarschuwingen op de volgende resourcetypen zijn nog niet gereed voor migratie.

- Microsoft.classicCompute/domainNames/slots/roles
- Microsoft.documentDB/databases
- Microsoft.insights/components

## <a name="who-can-trigger-the-migration"></a>Die de migratie kunt activeren?

Elke gebruiker die de ingebouwde rol van Inzender controleren op het abonnementsniveau van het kan de migratie te activeren. De migratie kunnen ook worden geactiveerd door gebruikers die een aangepaste rol met de volgende machtigingen hebben:

- \* / lezen
- Microsoft.Insights/actiongroups/*
- Microsoft.Insights/AlertRules/*
- Microsoft.Insights/metricAlerts/*

> [!NOTE]
> Naast de bovenstaande machtigingen, moet uw abonnement ook worden geregistreerd bij de resourceprovider Microsoft.AlertsManagement. Dit is vereist voor het migreren fout Anomaliedetectie waarschuwingen voor Application Insights. 

## <a name="common-problems-and-remedies"></a>Veelvoorkomende problemen en oplossingen

Nadat u [activeren van de migratie](alerts-using-migration-tool.md), ontvangt u e-mailadres op de adressen die u hebt opgegeven om u te waarschuwen dat de migratie voltooid is of als geen actie te ondernemen. Deze sectie beschrijft enkele veelvoorkomende problemen en hoe u omgaat met hen.

### <a name="validation-failed"></a>De validatie is mislukt

Door een recente wijzigingen in de klassieke waarschuwingsregels in uw abonnement, kan niet het abonnement worden gemigreerd. Dit probleem is tijdelijk. U kunt de migratie opnieuw opstarten nadat de migratiestatus van de wordt teruggeplaatst **gereed voor migratie** in een paar dagen.

### <a name="policy-or-scope-lock-preventing-us-from-migrating-your-rules"></a>Beleid of bereik vergrendelen, waardoor uw regels migreren

Als onderdeel van de migratie, nieuwe metrische waarschuwingen en nieuwe actiegroepen worden gemaakt en vervolgens regels voor klassieke waarschuwingen worden verwijderd. Er is echter een beleid of een bereik vergrendelen, waardoor het maken van resources. Sommige of alle regels kunnen niet worden gemigreerd, afhankelijk van de vergrendeling beleid of bereik. U kunt dit probleem oplossen door tijdelijk verwijderen van de scope vergrendelen of het beleid en het activeren van de migratie opnieuw uit.

## <a name="next-steps"></a>Volgende stappen

- [Het gebruik van het hulpprogramma voor migratie](alerts-using-migration-tool.md)
- [Voorbereiden voor de migratie](alerts-prepare-migration.md)
