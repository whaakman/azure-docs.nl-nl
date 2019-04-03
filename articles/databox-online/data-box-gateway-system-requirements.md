---
title: Systeemvereisten voor Microsoft Azure Data Box Gateway | Microsoft Docs
description: Meer informatie over de software en netwerkvereisten voor uw Azure Data Box-Gateway
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 04/02/2019
ms.author: alkohli
ms.openlocfilehash: cac451634bfa357784f9fd3d3a24e06ef3a4ee19
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58881763"
---
# <a name="azure-data-box-gateway-system-requirements"></a>Azure Data Box-Gateway-systeemvereisten

Dit artikel beschrijft de belangrijke systeemvereisten voor uw Microsoft Azure Data Box-Gateway-oplossing en de clients die verbinding maken met Azure Data Box-Gateway. Het is raadzaam dat u de informatie voordat u uw Data Box-Gateway implementeert, en vervolgens terug naar deze zo nodig tijdens de implementatie en het volgende gebruik verwijzen zorgvuldig te controleren.

De systeemvereisten voor het virtuele apparaat Data Box-Gateway zijn onder andere:

- **Softwarevereisten voor hosts** -beschrijving van de ondersteunde platforms, browsers voor de lokale configuratie-UI, SMB-clients en eventuele bijkomende vereisten voor de hosts die verbinding met het apparaat maken.
- **Netwerkvereisten voor het apparaat** -vindt u informatie over eventuele netwerkvereisten voor de bewerking van het virtuele apparaat.


## <a name="specifications-for-the-virtual-device"></a>De specificaties voor het virtuele apparaat

Het onderliggende hostsysteem voor de Data Box-Gateway kan besteden aan de volgende bronnen voor het inrichten van uw virtuele apparaat:

| Specificaties                                          | Description              |
|---------------------------------------------------------|--------------------------|
| Virtuele processors (kernen)   | Minimaal 4 |
| Geheugen  | Minimaal 8 GB|
| Beschikbaarheid|Eén knooppunt|
| Disks| Besturingssysteemschijf: 250 GB <br> Gegevensschijf: minimaal 2 TB, thin-provisioned en moet worden ondersteund door SSD's|
| Netwerkinterfaces|1 of meer virtuele netwerkinterfaces|


## <a name="supported-os-for-clients-connected-to-device"></a>Ondersteund besturingssysteem voor clients die zijn verbonden met het apparaat

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-supported-client-os.md)]

## <a name="supported-protocols-for-clients-accessing-device"></a>Ondersteunde protocollen voor toegang tot apparaat-clients

[!INCLUDE [Supported protocols for clients accessing device](../../includes/data-box-edge-gateway-supported-client-protocols.md)]

## <a name="supported-virtualization-platforms-for-device"></a>Ondersteunde virtualisatieplatforms voor apparaat

| **Besturingssysteem/platform**  |**Versies**   |**Opmerkingen**  |
|---------|---------|---------|
|Hyper-V  |  2012 R2 <br> 2016  |         |
|VMware ESXi     | 6.0 <br> 6.5 <br> 6.7       |VMware-hulpprogramma's worden niet ondersteund.         |


## <a name="supported-storage-accounts"></a>Ondersteunde opslagaccounts

[!INCLUDE [Supported storage accounts](../../includes/data-box-edge-gateway-supported-storage-accounts.md)]


## <a name="supported-storage-types"></a>Ondersteunde opslagtypen

[!INCLUDE [Supported storage types](../../includes/data-box-edge-gateway-supported-storage-types.md)]

## <a name="supported-browsers-for-local-web-ui"></a>Ondersteunde browsers voor de lokale web-UI

[!INCLUDE [Supported browsers for local web UI](../../includes/data-box-edge-gateway-supported-browsers.md)]

## <a name="networking-port-requirements"></a>Poort netwerkvereisten

De volgende tabel staan de poorten die moeten worden geopend in uw firewall om toe te staan voor SMB en de cloud en het beheer van verkeer. In deze tabel *in* of *inkomende* verwijst naar de richting van welke binnenkomende client aanvragen toegang tot uw apparaat. *Uit* of *uitgaande* verwijst naar de richting waarin uw gegevens in het Gateway-apparaat gegevens extern, na de implementatie verzendt: bijvoorbeeld uitgaand naar het Internet.

[!INCLUDE [Port configuration for device](../../includes/data-box-edge-gateway-port-config.md)]

## <a name="url-patterns-for-firewall-rules"></a>URL-patronen voor firewall-regels

Netwerkbeheerders kunnen vaak geavanceerde firewall-regels op basis van de URL-patronen voor het filteren van de inkomende en uitgaande verkeer configureren. Uw gegevens in het Gateway-apparaat en de gegevens in het Gateway-service, is afhankelijk van andere Microsoft-toepassingen, zoals Azure Service Bus, toegangsbeheer van Azure Active Directory, opslagaccounts en Microsoft Update-servers. De URL-patronen die zijn gekoppeld aan deze toepassingen kunnen worden gebruikt om de firewall-regels configureren. Het is belangrijk om te begrijpen dat de URL-patronen die zijn gekoppeld aan deze toepassingen kunnen wijzigen. Op zijn beurt hiervoor moet de netwerkbeheerder om te controleren en firewallregels bijwerken voor uw Data Box-Gateway als en wanneer dat nodig is.

U wordt aangeraden dat u uw firewall-regels voor uitgaand verkeer, op basis van vaste IP-adressen, opneemt in de meeste gevallen van Data Box Gateway ingesteld. Echter, kunt u de onderstaande informatie om in te stellen van geavanceerde firewallregels die nodig zijn om beveiligde omgevingen te maken.

> [!NOTE]
> - Het apparaat (bron) IP-adressen moet altijd worden ingesteld op alle netwerkinterfaces ingeschakeld voor de cloud.
> - De IP-adressen moet worden ingesteld op bestemming [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653).

[!INCLUDE [URL patterns for firewall](../../includes/data-box-edge-gateway-url-patterns-firewall.md)]

### <a name="url-patterns-for-azure-government"></a>URL-patronen voor Azure Government

[!INCLUDE [Azure Government URL patterns for firewall](../../includes/data-box-edge-gateway-gov-url-patterns-firewall.md)]

## <a name="internet-bandwidth"></a>Internet-bandbreedte

[!INCLUDE [Internet bandwidth](../../includes/data-box-edge-gateway-internet-bandwidth.md)]

## <a name="next-step"></a>Volgende stap

* [Uw Azure Data Box-Gateway implementeren](data-box-gateway-deploy-prep.md)

