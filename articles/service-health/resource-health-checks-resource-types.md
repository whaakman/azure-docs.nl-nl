---
title: Ondersteunde resourcetypen via Azure Resource Health | Microsoft Docs
description: Ondersteunde resourcetypen via Azure Resource health
services: Resource health
documentationcenter: ''
author: BernardoAMunoz
manager: ''
editor: ''
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 10/09/2017
ms.author: BernardoAMunoz
ms.openlocfilehash: 633501b03a362fbd07e14e308e18488f03af5af5
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39069198"
---
# <a name="resource-types-and-health-checks-in-azure-resource-health"></a>Resourcetypen en statuscontroles in Azure resource health
Hieronder volgt een volledige lijst van alle controles uitgevoerd via resource health door resourcetypen.

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers
|Uitgevoerde controles|
|---|
|<ul><li>De server actief is?</li><li>Heeft de server onvoldoende geheugen uitvoeren?</li><li>Wordt de server gestart?</li><li>Is de server herstellen?</li></ul>|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service
|Uitgevoerde controles|
|---|
|<ul><li>De Api Management-service actief is en wordt uitgevoerd?</li></ul>|

## <a name="microsoftcacheredisredis"></a>Microsoft.CacheRedis/Redis
|Uitgevoerde controles|
|---|
|<ul><li>Alle knooppunten in de Cache zijn actief en werkend?</li><li>De Cache kan worden bereikt vanaf binnen het datacenter?</li><li>Heeft het maximum aantal verbindingen bereikt door de Cache?</li><li> Heeft de cache het beschikbare geheugen uitgeput? </li><li>De Cache een groot aantal wisselfouten ondervindt?</li><li>De Cache is zwaar belast?</li></ul>|

## <a name="microsoftcdnprofile"></a>Microsoft.CDN/profile
|Uitgevoerde controles|
|---|
|<ul> <li>Is de aanvullende portal toegankelijk is voor CDN-configuratiebewerkingen?</li><li>Zijn er problemen met continue levering met de CDN-eindpunten?</li><li>Kunnen de configuratie van de CDN-resources wijzigen door gebruikers?</li><li>Worden wijzigingen in de configuratie doorgeven aan de verwachte frequentie?</li><li>Kunnen gebruikers beheren voor de CDN-configuratie met behulp van de Azure-portal, PowerShell of de API?</li> </ul>|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.classiccompute/virtualmachines
|Uitgevoerde controles|
|---|
|<ul><li>De host-server actief is?</li><li>Is de host OS opstarten voltooid?</li><li>De container voor de virtuele machine is ingericht en ingeschakeld?</li><li>Er een netwerkverbinding is tussen de host en de storage-account?</li><li>Is het opstarten van het gastbesturingssysteem voltooid?</li><li>Is er lopende gepland onderhoud?</li></ul>|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.cognitiveservices/accounts
|Uitgevoerde controles|
|---|
|<ul><li>Het account kan worden bereikt vanaf binnen het datacenter?</li><li>Is de Cognitive Services-Resourceprovider beschikbaar?</li><li>De Cognitive Service is beschikbaar in de juiste regio?</li><li>Kan lezen bewerkingen worden uitgevoerd op het storage-account met de metagegevens van de resource?</li><li>De API-aanroep quotum is bereikt?</li><li>De API-aanroep lezen-limiet is bereikt?</li></ul>|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.COMPUTE/virtualmachines
|Uitgevoerde controles|
|---|
|<ul><li>Is de server die als host fungeert deze virtuele machine omhoog en uitgevoerd?</li><li>Is de host OS opstarten voltooid?</li><li>De container voor de virtuele machine is ingericht en ingeschakeld?</li><li>Er een netwerkverbinding is tussen de host en de storage-account?</li><li>Is het opstarten van het gastbesturingssysteem voltooid?</li><li>Is er lopende gepland onderhoud?</li></ul>|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.datalakeanalytics/accounts
|Uitgevoerde controles|
|---|
|<ul><li>Hebt u ervaren problemen van gebruikers verzenden of de Data Lake Analytics-taken weergeven?</li><li>Data Lake Analytics-taken kan niet worden voltooid op systeemfouten?</li></ul>|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.datalakestore/accounts
|Uitgevoerde controles|
|---|
|<ul><li>Gebruikers gegevens uploaden naar Data Lake Store problemen ondervonden?</li><li>Gebruikers downloaden uit Data Lake Store problemen ondervonden?</li></ul>|

## <a name="microsoftdevicesiothubs"></a>Microsoft.devices/iothubs

|Uitgevoerde controles|
|---|
|<ul><li>De IoT-hub is actief en werkend?</li></ul>|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.documentdb/databaseAccounts
|Uitgevoerde controles|
|---|
|<ul><li>Er is een database of een verzameling aanvragen niet worden geleverd vanwege een Azure Cosmos DB-service niet beschikbaar zijn?</li><li>Er zijn documentaanvragen niet worden geleverd vanwege een Azure Cosmos DB-service niet beschikbaar zijn?</li></ul>|

## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults
|Uitgevoerde controles|
|---|
|<ul><li>Aanvragen voor key vault mislukken vanwege problemen met het platform van Azure Key Vault?</li><li>Worden de aanvragen voor key vault wordt beperkt vanwege te veel aanvragen door de klant?</li></ul>|

## <a name="microsoftnetworkconnections"></a>Microsoft.Network/Connections
|Uitgevoerde controles|
|---|
|<ul><li>Is de VPN-tunnel verbonden?</li><li>Zijn er configuratieconflicten in de verbinding?</li><li>De vooraf gedeelde sleutels correct zijn geconfigureerd?</li><li>De on-premises VPN-apparaat bereikbaar is?</li><li>Zijn er verschillen in het IPSec/IKE-beveiligingsbeleid?</li><li>De S2S VPN-verbinding is goed ingericht of in een foutstatus?</li><li>De VNET-naar-VNET-verbinding is goed ingericht of in een foutstatus?</li></ul>|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.network/virtualNetworkGateways
|Uitgevoerde controles|
|---|
|<ul><li>Is de VPN-gateway bereikbaar is vanaf het internet?</li><li>De VPN-Gateway in standby-modus is?</li><li>Wordt de VPN-service uitgevoerd op de gateway?</li></ul>|

## <a name="microsoftnotificationhubsnamespace"></a>Microsoft.NotificationHubs/namespace
|Uitgevoerde controles|
|---|
|<ul><li> Kunnen de runtime-bewerkingen, zoals registratie, installatie of verzenden worden uitgevoerd op de naamruimte?</li></ul>|

## <a name="microsoftpowerbiworkspacecollections"></a>Microsoft.PowerBI/workspaceCollections
|Uitgevoerde controles|
|---|
|<ul><li>Het hostbesturingssysteem actief is?</li><li>Is de workspaceCollection bereikbaar is vanaf buiten het datacenter?</li><li>Is de Resourceprovider van Power BI beschikbaar?</li><li>De PowerBI-Service is beschikbaar in de juiste regio?</li></ul>|

## <a name="microsoftsearchsearchservices"></a>Microsoft.search/searchServices
|Uitgevoerde controles|
|---|
|<ul><li>Kunnen diagnostische gegevens over bewerkingen worden uitgevoerd op het cluster?</li></ul>|

## <a name="microsoftsqlserverdatabase"></a>Microsoft.SQL/Server/database
|Uitgevoerde controles|
|---|
|<ul><li> Zijn er aanmeldingen bij de database?</li></ul>|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts
|Uitgevoerde controles|
|---|
|<ul><li>Aanvragen voor het lezen van gegevens van het opslagaccount mislukken vanwege problemen met het platform van Azure Storage?</li><li>Aanvragen voor het schrijven van gegevens naar het opslagaccount mislukken vanwege problemen met het platform van Azure Storage?</li><li>Is het opslagcluster waarin het opslagaccount zich bevindt die niet beschikbaar?</li></ul>|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs
|Uitgevoerde controles|
|---|
|<ul><li>Zijn alle hosts waar de taak is uitgevoerd om en uitgevoerd?</li><li>Is de taak kan niet worden gestart?</li><li>Zijn er lopende runtime upgrades?</li><li>De taak is een verwachte status (bijvoorbeeld wordt uitgevoerd of gestopt door de klant)?</li><li>De taak opgetreden voor uit geheugen uitzonderingen?</li><li>Zijn er lopende geplande compute updates?</li><li>De Manager kan worden uitgevoerd (besturingselement-plan) beschikbaar is?</li></ul>|

## <a name="microsoftwebserverfarms"></a>Microsoft.web/serverFarms
|Uitgevoerde controles|
|---|
|<ul><li>De host-server actief is?</li><li>Internet Information Services wordt uitgevoerd</li><li>Wordt de Load balancer uitgevoerd?</li><li>Kan de App Service-Plan worden bereikt vanaf binnen het datacenter?</li><li>Het opslagaccount dat als host fungeert voor de inhoud van de sites voor de serverFarm beschikbaar?</li></ul>|

## <a name="microsoftwebsites"></a>Microsoft.web/sites
|Uitgevoerde controles|
|---|
|<ul><li>De host-server actief is?</li><li>Wordt Internet Information server uitgevoerd?</li><li>Wordt de Load balancer uitgevoerd?</li><li>De Web-App kan worden bereikt vanaf binnen het datacenter?</li><li>Het opslagaccount dat als host fungeert voor de site-inhoud beschikbaar?</li></ul>|

# <a name="next-steps"></a>Volgende stappen
-  Zie [Inleiding tot Azure Service Health dashboard](service-health-overview.md) en [Inleiding tot Azure Resource Health](resource-health-overview.md) voor meer informatie over deze. 
-  [Veelgestelde vragen over Azure Resource Health](resource-health-faq.md)
- Stel waarschuwingen in zodat u een melding van statusproblemen ontvangt. Zie voor meer informatie, [waarschuwingen voor servicestatusgebeurtenissen configureren](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md). 
