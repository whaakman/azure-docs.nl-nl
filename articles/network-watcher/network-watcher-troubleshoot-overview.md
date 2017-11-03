---
title: Inleiding tot het oplossen van problemen in de Azure-netwerk-Watcher resource | Microsoft Docs
description: Deze pagina bevat een overzicht van de mogelijkheden van resource-voor probleemoplossing voor de netwerk-Watcher
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: c1145cd6-d1cf-4770-b1cc-eaf0464cc315
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: a37c92e1aa58184ed29185742ec727c120fe593f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-resource-troubleshooting-in-azure-network-watcher"></a>Inleiding tot het oplossen van problemen in de Azure-netwerk-Watcher resource

Virtuele netwerkgateways bieden connectiviteit tussen lokale bronnen en andere virtuele netwerken in Azure. Bewaking van deze gateways en hun verbindingen essentieel is voor communicatie is niet verbroken. Netwerk-Watcher biedt de mogelijkheid om op te lossen virtuele netwerkgateways en verbindingen. Dit kan via de portal, PowerShell, CLI of REST-API worden aangeroepen. Aangeroepen, netwerk-Watcher diagnose stelt de status van de gateway van virtueel netwerk of de verbinding als de juiste resultaten retourneren. Deze aanvraag is een langdurige transactie, worden de resultaten geretourneerd nadat de diagnose voltooid is.

![portal][2]

## <a name="results"></a>Resultaten

De voorlopige resultaten geven een overzicht van de status van de resource. Meer gedetailleerde informatie kan worden opgegeven voor resources, zoals wordt weergegeven in de volgende sectie:

De volgende lijst bevat de waarden geretourneerd met de problemen met API:

* **startTime** -deze waarde is de tijd die de problemen met API-aanroep is gestart.
* **endTime** -deze waarde is de tijd waarop de probleemoplossing is beëindigd.
* **code** -deze waarde is niet in orde, als er een fout één diagnose.
* **resultaten** -resultaten is een verzameling met resultaten geretourneerd op de netwerkverbinding of de virtuele netwerkgateway.
    * **id** -deze waarde is het fouttype.
    * **Samenvatting** -deze waarde is een overzicht van de fout.
    * **gedetailleerde** -deze waarde bevat een gedetailleerde beschrijving van de fout.
    * **recommendedActions** -deze eigenschap is een verzameling aanbevolen acties te ondernemen.
      * **actionText** -deze waarde bevat de tekst beschrijven welke actie moet worden uitgevoerd.
      * **actionUri** -deze waarde bevat de URI-documentatie over het om te fungeren.
      * **actionUriText** -deze waarde is een korte beschrijving van de tekst in te grijpen.

De volgende tabellen geven de typen met verschillende domeinen met fouten (id onder de resultaten van de voorgaande lijst) die beschikbaar zijn en als de fout Logboeken maakt.

### <a name="gateway"></a>Gateway

| Fouttype | Reden | Logboek|
|---|---|---|
| NoFault | Wanneer er geen fout wordt gedetecteerd. |Ja|
| GatewayNotFound | Kan de Gateway of Gateway niet is ingericht niet vinden. |Nee|
| PlannedMaintenance |  Gateway-instantie is in onderhoud.  |Nee|
| UserDrivenUpdate | Wanneer een gebruiker wordt bijgewerkt. Dit wordt mogelijk een bewerking formaat wijzigen. | Nee |
| VipUnResponsive | Kan het primaire exemplaar van de Gateway niet bereiken. Dit gebeurt wanneer de health-test is mislukt. | Nee |
| PlatformInActive | Er is een probleem met het platform. | Nee|
| ServiceNotRunning | De onderliggende service is niet uitgevoerd. | Nee|
| NoConnectionsFoundForGateway | Er bestaat geen verbindingen op de gateway. Dit is alleen een waarschuwing.| Nee|
| ConnectionsNotConnected | Verbindingen zijn niet verbonden. Dit is alleen een waarschuwing.| Ja|
| GatewayCPUUsageExceeded | De huidige Gateway CPU-gebruik is > 95%. | Ja |

### <a name="connection"></a>Verbinding

| Fouttype | Reden | Logboek|
|---|---|---|
| NoFault | Wanneer er geen fout wordt gedetecteerd. |Ja|
| GatewayNotFound | Kan de Gateway of Gateway niet is ingericht niet vinden. |Nee|
| PlannedMaintenance | Gateway-instantie is in onderhoud.  |Nee|
| UserDrivenUpdate | Wanneer een gebruiker wordt bijgewerkt. Dit wordt mogelijk een bewerking formaat wijzigen.  | Nee |
| VipUnResponsive | Kan het primaire exemplaar van de Gateway niet bereiken. Fout treedt op wanneer de health-test is mislukt. | Nee |
| ConnectionEntityNotFound | Verbindingsconfiguratie ontbreekt. | Nee |
| ConnectionIsMarkedDisconnected | De verbinding is gemarkeerd als 'verbinding verbroken'. |Nee|
| ConnectionNotConfiguredOnGateway | De onderliggende service heeft niet de verbinding geconfigureerd. | Ja |
| ConnectionMarkedStandy | De onderliggende service is gemarkeerd als stand-by.| Ja|
| Authentication | Vooraf gedeelde sleutel komt niet overeen. | Ja|
| PeerReachability | De peer-gateway is niet bereikbaar. | Ja|
| IkePolicyMismatch | De gateway van de peer heeft IKE-beleidsregels die worden niet ondersteund door Azure. | Ja|
| WfpParse fout | Er is een fout opgetreden bij het parseren van het WPF-logboek. |Ja|

## <a name="supported-gateway-types"></a>Ondersteunde gatewaytypen

De volgende lijst bevat de ondersteuning ziet u welke gateways en verbindingen worden ondersteund bij het oplossen van netwerk-Watcher.
|  |  |
|---------|---------|
|**Gatewaytypen**   |         |
|VPN      | Ondersteund        |
|ExpressRoute | Niet ondersteund |
|Hypernet | Niet ondersteund|
|**VPN-typen** | |
|Route gebaseerd | Ondersteund|
|Op basis van beleid | Niet ondersteund|
|**Verbindingstypen**||
|IPSec| Ondersteund|
|VNet2Vnet| Ondersteund|
|ExpressRoute| Niet ondersteund|
|Hypernet| Niet ondersteund|
|VPNClient| Niet ondersteund|

## <a name="log-files"></a>Logboekbestanden

De resource voor probleemoplossing logboekbestanden worden opgeslagen in een opslagaccount nadat de resource probleemoplossing is voltooid. De volgende afbeelding toont een voorbeeld van de inhoud van een aanroep van dat heeft geresulteerd in een fout opgetreden.

![ZIP-bestand][1]

> [!NOTE]
> In sommige gevallen wordt alleen een subset van de logboekbestanden geschreven naar de opslag.

Zie voor instructies over het downloaden van bestanden van azure storage-accounts, [aan de slag met Azure Blob storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Een ander hulpprogramma dat kan worden gebruikt, is Storage Explorer. Meer informatie over Opslagverkenner vindt u hier op de volgende koppeling: [Opslagverkenner](http://storageexplorer.com/)

### <a name="connectionstatstxt"></a>ConnectionStats.txt

De **ConnectionStats.txt** bestand bevat de algemene statistieken van de verbinding, met inbegrip van inkomende en uitgaande bytes, de status van de verbinding en de tijd dat de verbinding tot stand is gebracht.

> [!NOTE]
> Als de aanroep van de API voor probleemoplossing worden geretourneerd in orde, het enige wat geretourneerd in het zip-bestand is een **ConnectionStats.txt** bestand.

De inhoud van dit bestand zijn vergelijkbaar met het volgende voorbeeld:

```
Connectivity State : Connected
Remote Tunnel Endpoint :
Ingress Bytes (since last connected) : 288 B
Egress Bytes (Since last connected) : 288 B
Connected Since : 2/1/2017 8:22:06 PM
```

### <a name="cpustatstxt"></a>CPUStats.txt

De **CPUStats.txt** -bestand bevat CPU-gebruik en geheugen beschikbaar op het moment van de testen.  De inhoud van dit bestand is vergelijkbaar met het volgende voorbeeld:

```
Current CPU Usage : 0 % Current Memory Available : 641 MBs
```

### <a name="ikeerrorstxt"></a>IKEErrors.txt

De **IKEErrors.txt** bestand bevat alle IKE-fouten die zijn gevonden tijdens de bewaking.

Het volgende voorbeeld ziet de inhoud van een bestand IKEErrors.txt. Uw fouten kunnen afwijken, afhankelijk van het probleem.

```
Error: Authentication failed. Check shared key. Check crypto. Check lifetimes. 
     based on log : Peer failed with Windows error 13801(ERROR_IPSEC_IKE_AUTH_FAIL)
Error: On-prem device sent invalid payload. 
     based on log : IkeFindPayloadInPacket failed with Windows error 13843(ERROR_IPSEC_IKE_INVALID_PAYLOAD)
```

### <a name="scrubbed-wfpdiagtxt"></a>Verwijderd wfpdiag.txt

De **Scrubbed wfpdiag.txt** logboekbestand bevat het WPF-logboek. Dit logboek bevat de logboekregistratie van het pakket verwijderen en IKE/AuthIP fouten.

Het volgende voorbeeld ziet de inhoud van het bestand Scrubbed wfpdiag.txt. In dit voorbeeld wordt is de gedeelde sleutel van een verbinding niet juist kan worden afgeleid uit de 3e regel van de onderkant. Het volgende voorbeeld is alleen een fragment van het hele logboek, omdat het logboek langdurige, afhankelijk van het probleem kan zijn.

```
...
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Deleted ICookie from the high priority thread pool list
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|IKE diagnostic event:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Event Header:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Timestamp: 1601-01-01T00:00:00.000Z
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Flags: 0x00000106
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Local address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Remote address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IP version field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP version: IPv4
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP protocol: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local address: 13.78.238.92
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote address: 52.161.24.36
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Application ID:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  User SID: <invalid>
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Failure type: IKE/Authip Main Mode Failure
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Type specific info:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure error code:0x000035e9
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IKE authentication credentials are unacceptable
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure point: Remote
...
```

### <a name="wfpdiagtxtsum"></a>wfpdiag.txt.Sum

De **wfpdiag.txt.sum** bestand is een logboek weergegeven met de buffers en gebeurtenissen verwerkt.

Het volgende voorbeeld wordt de inhoud van een bestand wfpdiag.txt.sum.
```
Files Processed:
    C:\Resources\directory\924336c47dd045d5a246c349b8ae57f2.GatewayTenantWorker.DiagnosticsStorage\2017-02-02T17-34-23\wfpdiag.etl
Total Buffers Processed 8
Total Events  Processed 2169
Total Events  Lost      0
Total Format  Errors    0
Total Formats Unknown   486
Elapsed Time            330 sec
+-----------------------------------------------------------------------------------+
|EventCount    EventName            EventType   TMF                                 |
+-----------------------------------------------------------------------------------+
|        36    ikeext               ike_addr_utils_c844  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        12    ikeext               ike_addr_utils_c857  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        96    ikeext               ike_addr_utils_c832  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|         6    ikeext               ike_bfe_callbacks_c133  1dc2d67f-8381-6303-e314-6c1452eeb529|
|         6    ikeext               ike_bfe_callbacks_c61  1dc2d67f-8381-6303-e314-6c1452eeb529|
|        12    ikeext               ike_sa_management_c5698  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c8447  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c494  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c642  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c3162  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c3307  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het onderzoeken van VPN-Gateways en verbindingen via de portal in via [Gateway probleemoplossing - Azure-portal](network-watcher-troubleshoot-manage-portal.md).
<!--Image references-->

[1]: ./media/network-watcher-troubleshoot-overview/GatewayTenantWorkerLogs.png
[2]: ./media/network-watcher-troubleshoot-overview/portal.png
