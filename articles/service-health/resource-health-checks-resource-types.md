---
title: Ondersteunde resourcetypen via Azure resourcestatus | Microsoft Docs
description: Ondersteunde resourcetypen via Azure resourcestatus
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
ms.openlocfilehash: e37266f2438f9c6bc9de2d01624bda77f9d6ee8a
ms.sourcegitcommit: e34afd967d66aea62e34d912a040c4622a737acb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36945870"
---
# <a name="resource-types-and-health-checks-in-azure-resource-health"></a>Status van resourcetypen en controleert in Azure resourcestatus
Hieronder ziet u een volledige lijst met alle controles uitgevoerd via een resourcestatus door resourcetypen.

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers
|Uitgevoerde controles|
|---|
|<ul><li>Is de server actief en werkend?</li><li>Heeft de server onvoldoende geheugen beschikbaar?</li><li>Wordt de server gestart?</li><li>Is de server herstellen?</li></ul>|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service
|Uitgevoerde controles|
|---|
|<ul><li>De Api Management-service actief is en wordt uitgevoerd?</li></ul>|

## <a name="microsoftcacheredisredis"></a>Microsoft.CacheRedis/Redis
|Uitgevoerde controles|
|---|
|<ul><li>Alle knooppunten in de Cache actief zijn?</li><li>De Cache kan worden bereikt vanaf binnen het datacenter?</li><li>Heeft het maximum aantal verbindingen bereikt door de Cache?</li><li> Heeft de cache het beschikbare geheugen uitgeput? </li><li>De Cache korte tijd een groot aantal pagina's?</li><li>De Cache is zwaar wordt belast?</li></ul>|

## <a name="microsoftcdnprofile"></a>Microsoft.CDN/profile
|Uitgevoerde controles|
|---|
|<ul> <li>Een van de eindpunten is gestopt, verwijderd of niet juist geconfigureerd?</li><li>Is de aanvullende portal toegankelijk voor configuratiebewerkingen CDN?</li><li>Zijn er lopende levering problemen met de CDN-eindpunten?</li><li>Kunnen de configuratie van hun bronnen CDN wijzigen door gebruikers?</li><li>Configuratiewijzigingen doorgeven met de verwachte frequentie?</li><li>Kunnen gebruikers de CDN-configuratie met behulp van de Azure-portal, PowerShell of de API beheren?</li> </ul>|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.classiccompute/virtualmachines
|Uitgevoerde controles|
|---|
|<ul><li>De hostserver is actief en werkend?</li><li>Is de host-OS opstarten voltooid?</li><li>De container voor de virtuele machine is ingericht en ingeschakeld?</li><li>Er is een netwerkverbinding tussen de host en het storage-account?</li><li>Is het opstarten van het gastbesturingssysteem voltooid?</li><li>Is er lopende gepland onderhoud?</li></ul>|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.cognitiveservices/accounts
|Uitgevoerde controles|
|---|
|<ul><li>Het account kan worden bereikt vanaf binnen het datacenter?</li><li>Is de cognitieve Services Resource Provider beschikbaar?</li><li>De cognitieve Service beschikbaar is in de juiste regio?</li><li>Kan lezen bewerkingen worden uitgevoerd op het opslagaccount van de resource-metagegevens?</li><li>Het quotum van API-aanroep is bereikt?</li><li>Is de API-aanroep lezen-limiet bereikt?</li></ul>|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.COMPUTE/virtualmachines
|Uitgevoerde controles|
|---|
|<ul><li>De server die als host fungeert deze virtuele machine omhoog en uitgevoerd?</li><li>Is de host-OS opstarten voltooid?</li><li>De container voor de virtuele machine is ingericht en ingeschakeld?</li><li>Er is een netwerkverbinding tussen de host en het storage-account?</li><li>Is het opstarten van het gastbesturingssysteem voltooid?</li><li>Is er lopende gepland onderhoud?</li></ul>|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.datalakeanalytics/accounts
|Uitgevoerde controles|
|---|
|<ul><li>Hebben gebruikers ervaren problemen bij het verzenden van of hun Data Lake Analytics-taken weergeven?</li><li>Data Lake Analytics-taken kan niet worden voltooid op fouten in het bestandssysteem zijn?</li></ul>|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.datalakestore/accounts
|Uitgevoerde controles|
|---|
|<ul><li>Hebben gebruikers ondervonden problemen met het uploaden van gegevens naar Data Lake Store?</li><li>Hebben gebruikers ondervonden problemen bij het downloaden van gegevens in Data Lake Store?</li></ul>|

## <a name="microsoftdevicesiothubs"></a>Microsoft.devices/iothubs

|Uitgevoerde controles|
|---|
|<ul><li>De IoT-hub is actief en werkend?</li></ul>|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.documentdb/databaseAccounts
|Uitgevoerde controles|
|---|
|<ul><li>Er is een database of een verzameling aanvragen niet worden geleverd vanwege een Cosmos-DB Azure-service niet beschikbaar zijn?</li><li>Er zijn aanvragen-document niet worden geleverd vanwege een Cosmos-DB Azure-service niet beschikbaar zijn?</li></ul>|

## <a name="microsoftnetworkconnections"></a>Microsoft.Network/Connections
|Uitgevoerde controles|
|---|
|<ul><li>Is de VPN-tunnel aangesloten?</li><li>Zijn er conflicten bij de configuratie in de verbinding?</li><li>De vooraf gedeelde sleutels juist zijn geconfigureerd?</li><li>De on-premises VPN-apparaat bereikbaar is?</li><li>Zijn er verschillen in het beveiligingsbeleid IPSec/IKE?</li><li>Is de S2S VPN-verbinding juist is ingericht of in een foutstatus?</li><li>Is de VNET-naar-VNET-verbinding juist is ingericht of in een foutstatus?</li></ul>|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.network/virtualNetworkGateways
|Uitgevoerde controles|
|---|
|<ul><li>Is de VPN-gateway bereikbaar is vanaf internet?</li><li>De VPN-Gateway in standby-modus is?</li><li>Wordt de VPN-service uitgevoerd op de gateway?</li></ul>|

## <a name="microsoftnotificationhubsnamespace"></a>Microsoft.NotificationHubs/namespace
|Uitgevoerde controles|
|---|
|<ul><li> Kunnen de runtime-bewerkingen zoals registratie, installatie of verzenden worden uitgevoerd op de naamruimte?</li></ul>|

## <a name="microsoftpowerbiworkspacecollections"></a>Microsoft.PowerBI/workspaceCollections
|Uitgevoerde controles|
|---|
|<ul><li>Het hostbesturingssysteem is actief en werkend?</li><li>Is de workspaceCollection bereikbaar is vanaf buiten het datacenter?</li><li>Is de Bronprovider van Power BI beschikbaar?</li><li>De PowerBI-Service is beschikbaar in de juiste regio?</li></ul>|

## <a name="microsoftsearchsearchservices"></a>Microsoft.search/searchServices
|Uitgevoerde controles|
|---|
|<ul><li>Kunnen diagnostische gegevens bewerkingen worden uitgevoerd op het cluster?</li></ul>|

## <a name="microsoftsqlserverdatabase"></a>Microsoft.SQL/Server/database
|Uitgevoerde controles|
|---|
|<ul><li> Zijn er aanmeldingen bij de database?</li></ul>|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts
|Uitgevoerde controles|
|---|
|<ul><li>Aanvragen voor gegevens lezen uit het opslagaccount mislukken vanwege problemen met het Azure Storage-platform?</li><li>Aanvragen om gegevens te schrijven naar het opslagaccount mislukken vanwege problemen met het Azure Storage-platform?</li><li>Het cluster waar het opslagaccount bevindt zich niet beschikbaar is?</li></ul>|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs
|Uitgevoerde controles|
|---|
|<ul><li>Alle hosts waarbij de taak uitvoeren en actief zijn?</li><li>Is de taak kan niet worden gestart?</li><li>Zijn er lopende runtime upgrades?</li><li>De taak is een verwachte status (bijvoorbeeld uitgevoerd of gestopt door de klant)?</li><li>De taak opgetreden uit geheugen uitzonderingen?</li><li>Zijn er lopende geplande compute updates?</li><li>De uitvoeringsbeheer (dit plan) beschikbaar is?</li></ul>|

## <a name="microsoftwebserverfarms"></a>Microsoft.web/serverFarms
|Uitgevoerde controles|
|---|
|<ul><li>De hostserver is actief en werkend?</li><li>Internet Information Services wordt uitgevoerd</li><li>Is de Load balancer wordt uitgevoerd?</li><li>Kan de App Service-Plan worden bereikt vanaf binnen het datacenter?</li><li>Het opslagaccount als host fungeert voor de sites inhoud voor de serverFarm beschikbaar veldnamenrij?</li></ul>|

## <a name="microsoftwebsites"></a>Microsoft.web/sites
|Uitgevoerde controles|
|---|
|<ul><li>De hostserver is actief en werkend?</li><li>Is Internet Information server wordt uitgevoerd?</li><li>Is de Load balancer wordt uitgevoerd?</li><li>De Web-App kan worden bereikt vanaf binnen het datacenter?</li><li>Het opslagaccount als host fungeert voor de site-inhoud beschikbaar?</li></ul>|

# <a name="next-steps"></a>Volgende stappen
-  Zie [Inleiding tot Azure-Service Health dashboard](service-health-overview.md) en [Inleiding tot Azure-resourcestatus](resource-health-overview.md) voor meer informatie hierover begrip. 
-  [Veelgestelde vragen over Azure-resourcestatus](resource-health-faq.md)
- Waarschuwingen instellen, zodat u wordt gewaarschuwd van statusproblemen. Zie voor meer informatie [waarschuwingen configureren voor gebeurtenissen van de health service](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md). 
