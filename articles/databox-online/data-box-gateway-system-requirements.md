---
title: Systeemvereisten voor Microsoft Azure Data Box Gateway | Microsoft Docs
description: Meer informatie over de software en netwerkvereisten voor uw Azure Data Box-Gateway
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 10/17/2018
ms.author: alkohli
ms.openlocfilehash: 6978360769ba350d85a06116bfee6db9f80b0607
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50157357"
---
# <a name="azure-data-box-gateway-system-requirements-preview"></a>Systeemvereisten voor Azure Data Box-Gateway (Preview)

Dit artikel beschrijft de belangrijke systeemvereisten voor uw Microsoft Azure Data Box-Gateway-oplossing en de clients die verbinding maken met Azure Data Box-Gateway. Het is raadzaam dat u de informatie voordat u uw Data Box-Gateway implementeert, en vervolgens terug naar deze zo nodig tijdens de implementatie en het volgende gebruik verwijzen zorgvuldig te controleren.

De systeemvereisten voor het virtuele apparaat Data Box-Gateway zijn onder andere:

- **Softwarevereisten voor hosts** -beschrijving van de ondersteunde platforms, browsers voor de lokale configuratie-UI, SMB-clients en eventuele bijkomende vereisten voor de hosts die verbinding met het apparaat maken.
- **Netwerkvereisten voor het apparaat** -vindt u informatie over eventuele netwerkvereisten voor de bewerking van het virtuele apparaat.

> [!IMPORTANT]
> Data Box Gateway is in de previewfase. Lees de [gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing implementeert. 

## <a name="supported-os-for-clients-connected-to-device"></a>Ondersteund besturingssysteem voor clients die zijn verbonden met het apparaat

Hier volgt een lijst van de ondersteunde besturingssystemen voor clients of hosts die zijn verbonden met de Data Box-Gateway.

| **Besturingssysteem/platform** | **Versies** |
| --- | --- |
| Windows Server |2012 R2 <br> 2016 |
| Windows |8, 10 |
| SUSE Linux |Enterprise Server 12 (x86_64)|
| Ubuntu |16.04.3 LTS|
| CentOS | 7.0 |

## <a name="supported-protocols-for-clients-accessing-device"></a>Ondersteunde protocollen voor toegang tot apparaat-clients

|**Protocol** |**Versies**   |**Opmerkingen**  |
|---------|---------|---------|
|SMB    | 2.X 3.X      | SMB-1 wordt niet ondersteund.|
|NFS     | V3- en V4-processors        |         |

## <a name="supported-virtualization-platforms-for-device"></a>Ondersteunde virtualisatieplatforms voor apparaat

| **Besturingssysteem/platform**  |**Versies**   |**Opmerkingen**  |
|---------|---------|---------|
|Hyper-V  |  2012 R2 <br> 2016  |         |
|VMware ESXi     | 6.0 <br> 6.5        |VMware-hulpprogramma's worden niet ondersteund.         |


## <a name="supported-storage-accounts"></a>Ondersteunde opslagaccounts

Hier volgt een lijst met ondersteunde opslagaccounts voor de Data Box-Gateway.

| **Opslagaccount** | **Opmerkingen** |
| --- | --- |
| Klassiek | Standard |
| Algemeen doel  |Standaard; zowel V1 als V2 worden ondersteund. Warme en koude lagen worden ondersteund. |


## <a name="supported-storage-types"></a>Ondersteunde opslagtypen

Hier volgt een lijst van de typen ondersteunde opslag voor de Data Box-Gateway.

| **Bestandsindeling** | **Opmerkingen** |
| --- | --- |
| Azure blok-blob | |
| Azure-pagina-blobs  | |
| Azure Files | |

## <a name="supported-browsers-for-local-web-ui"></a>Ondersteunde browsers voor de lokale web-UI

Hier volgt een lijst van de browsers die worden ondersteund voor de lokale webgebruikersinterface voor het virtuele apparaat.

|Browser  |Versies  |Aanvullende vereisten/opmerkingen  |
|---------|---------|---------|
|Google Chrome   |Meest recente versie         |         |
|Microsoft Edge    | Meest recente versie        |         |
|Internet Explorer     | Meest recente versie        |         |
|FireFox    |Meest recente versie         |         |


## <a name="networking-requirements"></a>Netwerkvereisten

De volgende tabel staan de poorten die moeten worden geopend in uw firewall om toe te staan voor SMB en de cloud en het beheer van verkeer. In deze tabel *in* of *inkomende* verwijst naar de richting van welke binnenkomende client aanvragen toegang tot uw apparaat. *Uit* of *uitgaande* verwijst naar de richting waarin uw gegevens in het Gateway-apparaat gegevens extern, na de implementatie verzendt: bijvoorbeeld uitgaand naar het Internet.

| Poort niet.| In- of uitschalen | Poort-bereik| Vereist|   Opmerkingen                                                             |                                                                                     |
|--------|---------|----------|--------------|----------------------|---------------|
| TCP 80 (HTTP)|Uit|WAN |Nee|Uitgaande poort wordt gebruikt voor toegang tot Internet om op te halen van updates. <br>De webproxy uitgaande is gebruiker worden geconfigureerd. |                          
| TCP 443 (HTTPS)|Uit|WAN|Ja|Uitgaande poort wordt gebruikt voor toegang tot gegevens in de cloud.<br>De webproxy uitgaande is gebruiker worden geconfigureerd.|   
| UDP 53 (DNS)|Uit|WAN|In sommige gevallen<br>Zie Opmerkingen bij de|Deze poort is alleen vereist als u van een op basis van een Internet-DNS-server gebruikmaakt.<br>We raden u aan met behulp van de lokale DNS-server. |
| UDP 123 (NTP)|Uit|WAN|In sommige gevallen<br>Zie Opmerkingen bij de|Deze poort is alleen vereist als u een Internet-gebaseerde NTP-server gebruikt.  |
| UDP 67 (DHCP)|Uit|WAN|In sommige gevallen<br>Zie Opmerkingen bij de|Deze poort is alleen vereist als u met behulp van een DHCP-server.  |
| TCP 80 (HTTP)|In|LAN|Ja|Dit is de binnenkomende poort voor lokale UI op het apparaat voor lokaal beheer. <br>Toegang tot de gebruikersinterface voor het lokale via HTTP worden automatisch omgeleid naar HTTPS.  | 
| TCP 443 (HTTPS)|In|LAN|Ja|Dit is de binnenkomende poort voor lokale UI op het apparaat voor lokaal beheer. | 

## <a name="url-patterns-for-firewall-rules"></a>URL-patronen voor firewall-regels

Netwerkbeheerders kunnen vaak geavanceerde firewall-regels op basis van de URL-patronen voor het filteren van de inkomende en uitgaande verkeer configureren. Uw gegevens in het Gateway-apparaat en de gegevens in het Gateway-service, is afhankelijk van andere Microsoft-toepassingen, zoals Azure Service Bus, toegangsbeheer van Azure Active Directory, opslagaccounts en Microsoft Update-servers. De URL-patronen die zijn gekoppeld aan deze toepassingen kunnen worden gebruikt om de firewall-regels configureren. Het is belangrijk om te begrijpen dat de URL-patronen die zijn gekoppeld aan deze toepassingen kunnen wijzigen. Op zijn beurt hiervoor moet de netwerkbeheerder om te controleren en firewallregels bijwerken voor uw Data Box-Gateway als en wanneer dat nodig is.

U wordt aangeraden dat u uw firewall-regels voor uitgaand verkeer, op basis van vaste IP-adressen, opneemt in de meeste gevallen van Data Box Gateway ingesteld. Echter, kunt u de onderstaande informatie om in te stellen van geavanceerde firewallregels die nodig zijn om beveiligde omgevingen te maken.

> [!NOTE]
> - Het apparaat (bron) IP-adressen moet altijd worden ingesteld op alle netwerkinterfaces ingeschakeld voor de cloud.
> - De IP-adressen moet worden ingesteld op bestemming [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653).

|     URL-patroon                                                                                                                                                                                                                                                                                                                                                                                                                                       |     Onderdeel/functionaliteit                                                                             |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
|    https://*.databoxedge.azure.com/*<br>https://*.servicebus.windows.net/*<br>https://login.windows.net                                                                                                                                                                                                                                                                                                        |    Azure Data Box-Gateway-service<br>Azure Service Bus<br>Verificatieservice    |
|    http://*.Backup.windowsazure.com                                                                                                                                                                                                                                                                                                                                                                                                                   |    Activering van apparaat                                                                                    |
|    http://crl.microsoft.com/pki/*   http://www.microsoft.com/pki/*                                                                                                                                                                                                                                                                                                                                                                                    |    Het intrekken van certificaten                                                                               |
|    https://*.core.windows.net/* https://*. data.microsoft.com http://*. msftncsi.com                                                                                                                                                                                                                                                                                                                                                                |    Azure storage-accounts en bewaking                                                                |
|    http://windowsupdate.microsoft.com<br>http://*. windowsupdate.microsoft.com<br>https://*. windowsupdate.microsoft.com<br>http://*. update.microsoft.com<br>https://*. update.microsoft.com<br>http://*. windowsupdate.com<br>http://download.microsoft.com<br>http://*. download.windowsupdate.com<br>http://wustat.windows.com<br>http://ntservicepack.microsoft.com<br>http://*. ws.microsoft.com<br>https://*. ws.microsoft.com<br>http://*.MP.Microsoft.com        |    Microsoft Update-servers                                                                             |
|    http://*.Deploy.akamaitechnologies.com                                                                                                                                                                                                                                                                                                                                                                                                             |    Akamai CDN                                                                                           |
|    https://*.partners.extranet.microsoft.com/*                                                                                                                                                                                                                                                                                                                                                                                                        |    Ondersteuningspakket                                                                                      |
|    http://*.Data.Microsoft.com                                                                                                                                                                                                                                                                                                                                                                                                                        |    Telemetrieservice in Windows, Zie de update voor de gebruikerservaring en diagnostische telemetrie      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                       |                                                                                                         |



## <a name="internet-bandwidth"></a>Internet-bandbreedte

De volgende vereisten zijn van toepassing op ten minste internetbandbreedte die beschikbaar is voor uw gegevens in het Gateway-apparaten.

- Uw Data Box Gateway te allen tijde beschikt over een toegewezen internetbandbreedte van 20 Mbps (of meer). Deze bandbreedte mag niet worden gedeeld met andere toepassingen. 
- Uw Data Box-Gateway is een speciale 32 Mbps internetbandbreedte (of meer) bij het gebruik van netwerkbeperking.

## <a name="next-step"></a>Volgende stap

* [Uw Azure Data Box-Gateway implementeren](data-box-gateway-deploy-prep.md)

