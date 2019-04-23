---
title: Systeemvereisten voor Microsoft Azure Data Box Edge | Microsoft Docs
description: Meer informatie over de software en netwerkvereisten voor uw Azure Data Box-Edge
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 04/03/2019
ms.author: alkohli
ms.openlocfilehash: 90c60d586d505ca0c9bd787c37e137f7a38ee1f7
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59996744"
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

| URL-patroon                      | Onderdeel of functionaliteit                     |   
|----------------------------------|---------------------------------------------|
| https:\//mcr.microsoft.com<br></br>https://\*.cdn.mscr.io | Microsoft-containerregister (vereist)               |
| https://\*.azurecr.io                     | Persoonlijke en van derden container Registry (optioneel) | 
| https://\*.azure-devices.net              | IoT Hub-toegang (vereist)                             | 

### <a name="url-patterns-for-gateway-for-azure-government"></a>URL-patronen voor gateway voor Azure Government

[!INCLUDE [Azure Government URL patterns for firewall](../../includes/data-box-edge-gateway-gov-url-patterns-firewall.md)]

### <a name="url-patterns-for-compute-for-azure-government"></a>URL-patronen voor compute voor Azure Government

| URL-patroon                      | Onderdeel of functionaliteit                     |  
|----------------------------------|---------------------------------------------|
| https:\//mcr.microsoft.com<br></br>https://\*.cdn.mscr.com | Microsoft-containerregister (vereist)               |
| https://\*.azure-devices.us              | IoT Hub-toegang (vereist)           |
| https://\*.azurecr.us                    | Persoonlijke en van derden container Registry (optioneel) | 

## <a name="internet-bandwidth"></a>Internet-bandbreedte

[!INCLUDE [Internet bandwidth](../../includes/data-box-edge-gateway-internet-bandwidth.md)]

## <a name="compute-sizing-considerations"></a>COMPUTE dimensioneringsoverwegingen

Uw ervaring bij het ontwikkelen en testen van uw oplossing gebruiken om te controleren of er voldoende capaciteit is op uw gegevens in het Edge-apparaat en u de optimale prestaties van uw apparaat.

U moet rekening houden met factoren zijn onder andere:

- **Details van de container** -denken over het volgende.

    - Hoeveel containers zijn in uw werkbelasting? U kunt veel van lichtgewicht containers ten opzichte van enkele resource-intensieve labels hebben.
    - Wat zijn de resources die zijn toegewezen aan deze containers ten opzichte van wat zijn de resources die ze gebruiken?
    - Het aantal lagen maken gebruik van uw containers?
    - Zijn er niet-gebruikte containers? Een gestopte container neemt nog steeds schijfruimte.
    - In welke taal zijn uw containers geschreven?
- **Grootte van de gegevens verwerkte** -hoeveel gegevens uw containers wordt verwerkt? Deze gegevens verbruikt schijfruimte of de gegevens in het geheugen wordt verwerkt?
- **Prestaties verwacht** -wat zijn de gewenste prestatiekenmerken van uw oplossing? 

Om te begrijpen en verfijnen van de prestaties van uw oplossing, kunt u het volgende gebruiken:

- De compute-meetgegevens beschikbaar zijn in Azure portal. Ga naar uw gegevens in het Edge-resource en ga vervolgens naar **bewaking > metrische gegevens**. Bekijk de **Edge compute - geheugengebruik** en **Edge compute - Percentage CPU** om te begrijpen van de beschikbare resources en hoe worden de resources ophalen die worden gebruikt.
- De bewaking opdrachten die beschikbaar zijn via de PowerShell-interface van het apparaat, zoals:

    - `dkr` statistieken voor een live stream van containers Haal verbruiksstatistieken op resource. De opdracht biedt ondersteuning voor CPU, geheugengebruik, geheugenlimiet en netwerk-i/o-metrische gegevens.
    - `dkr system df` voor informatie met betrekking tot de hoeveelheid schijfruimte die wordt gebruikt. 
    - `dkr image [prune]` opschonen van niet-gebruikte installatiekopieën en ruimte vrij te maken.
    - `dkr ps --size` om te bekijken van de geschatte grootte van een container die wordt uitgevoerd. 

    Voor meer informatie over de beschikbare opdrachten gaat u naar [bewaken en problemen oplossen compute modules](data-box-edge-connect-powershell-interface.md#monitor-and-troubleshoot-compute-modules).

Ten slotte, zorg ervoor dat u uw oplossing voor uw gegevensset te valideren en de prestaties voor gegevens in het edge-apparaten voordat u implementeert in productie kwantificeren.


## <a name="next-step"></a>Volgende stap

- [Uw Azure Data Box-Edge implementeren](data-box-edge-deploy-prep.md)
