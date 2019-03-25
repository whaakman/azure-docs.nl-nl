---
title: Systeemvereisten voor Microsoft Azure Data Box Edge | Microsoft Docs
description: Meer informatie over de software en netwerkvereisten voor uw Azure Data Box-Edge
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/22/2019
ms.author: alkohli
ms.openlocfilehash: 125ad28f049662ae6d91c61bb5ee79c1c1428af5
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58401758"
---
# <a name="azure-data-box-edge-system-requirements"></a>Azure Data Box-Edge-systeemvereisten

Dit artikel beschrijft de belangrijke systeemvereisten voor uw Microsoft Azure Data Box Edge-oplossing en de clients die verbinding maken met Azure Data Box Edge. Het is raadzaam dat u de informatie voordat u uw Data Box-Edge implementeren zorgvuldig te controleren. U kunt raadplegen tot deze gegevens zo nodig tijdens de implementatie en het volgende gebruik.

De systeemvereisten voor de Data Box-rand zijn onder andere:

- **Softwarevereisten voor hosts** -beschrijving van de ondersteunde platforms, browsers voor de lokale configuratie-UI, SMB-clients en eventuele bijkomende vereisten voor de clients die toegang het apparaat tot.
- **Netwerkvereisten voor het apparaat** -vindt u informatie over eventuele netwerkvereisten voor de werking van het fysieke apparaat.

## <a name="supported-os-for-clients-connected-to-device"></a>Ondersteund besturingssysteem voor clients die zijn verbonden met het apparaat

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-supported-client-os.md)]

## <a name="supported-protocols-for-clients-accessing-device"></a>Ondersteunde protocollen voor toegang tot apparaat-clients

[!INCLUDE [Supported protocols for clients accessing device](../../includes/data-box-edge-gateway-supported-client-protocols.md)]

## <a name="supported-storage-accounts"></a>Ondersteunde opslagaccounts

[!INCLUDE [Supported storage accounts](../../includes/data-box-edge-gateway-supported-storage-accounts.md)]

## <a name="supported-storage-types"></a>Ondersteunde opslagtypen

[!INCLUDE [Supported storage types](../../includes/data-box-edge-gateway-supported-storage-types.md)]

## <a name="supported-browsers-for-local-web-ui"></a>Ondersteunde browsers voor de lokale web-UI

[!INCLUDE [Supported browsers for local web UI](../../includes/data-box-edge-gateway-supported-browsers.md)]

## <a name="networking-port-requirements"></a>Poort netwerkvereisten

### <a name="port-requirements-for-data-box-edge"></a>Poortvereisten voor gegevens in Microsoft Edge

De volgende tabel staan de poorten die moeten worden geopend in uw firewall om toe te staan voor SMB en de cloud en het beheer van verkeer. In deze tabel *in* of *inkomende* verwijst naar de richting van welke binnenkomende client aanvragen toegang tot uw apparaat. *Uit* of *uitgaande* verwijst naar de richting waarin uw gegevens in het Edge-apparaat gegevens extern, na de implementatie, bijvoorbeeld, uitgaand naar het internet verzendt.

[!INCLUDE [Port configuration for device](../../includes/data-box-edge-gateway-port-config.md)]

### <a name="port-requirements-for-iot-edge"></a>Poortvereisten voor IoT Edge

Azure IoT Edge kunt uitgaande communicatie tussen een on-premises Edge-apparaat en Azure-cloud met ondersteunde protocollen van IoT-Hub. Binnenkomende communicatie is alleen vereist voor specifieke scenario's waar Azure IoT Hub moeten zijn om berichten naar de Azure IoT Edge-apparaat (bijvoorbeeld Cloud naar apparaatmessaging).

Gebruik de volgende tabel voor de configuratie van de poort voor de servers die als host fungeert voor Azure IoT Edge-runtime:

| Poort niet. | In- of uitschalen | Poort-bereik | Vereist | Richtlijnen |
|----------|-----------|------------|----------|----------|
| TCP 443 (HTTPS)| Uit       | WAN        | Ja      | Uitgaande open voor IoT Edge wordt ingericht. Deze configuratie is vereist bij het gebruik van handmatige scripts of Azure IoT Device Provisioning Service (DPS).|

Voor meer informatie gaat u naar [Firewall en configuratieregels poort voor de implementatie van IoT Edge](https://docs.microsoft.com/azure/iot-edge/troubleshoot).

## <a name="url-patterns-for-firewall-rules"></a>URL-patronen voor firewall-regels

Netwerkbeheerders kunnen vaak geavanceerde firewall-regels op basis van de URL-patronen voor het filteren van de inkomende en uitgaande verkeer configureren. Uw gegevens in het Edge-apparaat en de service, is afhankelijk van andere Microsoft-toepassingen, zoals Azure Service Bus, toegangsbeheer van Azure Active Directory, opslagaccounts en Microsoft Update-servers. De URL-patronen die zijn gekoppeld aan deze toepassingen kunnen worden gebruikt om de firewall-regels configureren. Het is belangrijk om te begrijpen dat de URL-patronen die zijn gekoppeld aan deze toepassingen kunnen wijzigen. Deze wijzigingen vereisen de netwerkbeheerder om te controleren en firewallregels bijwerken voor uw gegevens in Edge als en wanneer dat nodig is.

U wordt aangeraden dat u uw firewall-regels voor uitgaand verkeer, op basis van gegevens in Edge vaste IP-adressen, opneemt in de meeste gevallen hebt ingesteld. Echter, kunt u de onderstaande informatie om in te stellen van geavanceerde firewallregels die nodig zijn om beveiligde omgevingen te maken.

> [!NOTE]
> - Het apparaat (bron) IP-adressen moet altijd worden ingesteld op alle netwerkinterfaces ingeschakeld voor de cloud.
> - De IP-adressen moet worden ingesteld op bestemming [Azure datacenter IP-adresbereiken](https://www.microsoft.com/download/confirmation.aspx?id=41653).

### <a name="url-patterns-for-gateway-feature"></a>URL-patronen voor de functie voor de gateway

[!INCLUDE [URL patterns for firewall](../../includes/data-box-edge-gateway-url-patterns-firewall.md)]

### <a name="url-patterns-for-compute-feature"></a>URL-patronen voor compute-functie

| URL-patroon                      | Onderdeel of functionaliteit                     |   |
|----------------------------------|---------------------------------------------|---|
| `https://mcr.microsoft.com`<br></br>https://\*.cdn.mscr.io | Microsoft-containerregister (vereist)               |   |
| https://\*.azurecr.io                     | Persoonlijke en van derden container Registry (optioneel) |   |
| https://\*.azure-devices.net              | IoT Hub-toegang (vereist)                             |   |

## <a name="internet-bandwidth"></a>Internet-bandbreedte

[!INCLUDE [Internet bandwidth](../../includes/data-box-edge-gateway-internet-bandwidth.md)]

## <a name="next-step"></a>Volgende stap

- [Uw Azure Data Box-Edge implementeren](data-box-Edge-deploy-prep.md)
