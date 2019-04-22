---
title: Azure Data Box-Gateway-beveiliging | Microsoft Docs
description: Beschrijft de functies voor beveiliging en privacy die bescherming van uw virtuele apparaat Azure Data Box-Gateway, service en gegevens on-premises en in de cloud.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 04/16/2019
ms.author: alkohli
ms.openlocfilehash: b27b712128ddfbb07a7a7f68f616c20ec3fb53d3
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59685793"
---
# <a name="azure-data-box-gateway-security-and-data-protection"></a>Azure Data Box-Gateway-beveiliging en gegevensbescherming

Beveiliging is van groot belang bij het kiezen van een nieuwe technologie, met name als de technologie wordt gebruikt met vertrouwelijke of geheime gegevens. Microsoft Azure Data Box-Gateway-oplossing zorgt ervoor dat alleen geautoriseerde entiteiten kunnen weergeven, wijzigen of verwijderen van uw gegevens.

Dit artikel beschrijft de beveiligingsfuncties van Azure Data Box-Gateway die u helpen beschermen van elk van de onderdelen van de oplossing en de gegevens die zijn opgeslagen op deze.

De gegevens in het Gateway-oplossing bestaat uit vier hoofdonderdelen die met elkaar communiceren:

- **Gegevens in het Gateway-service die wordt gehost in Azure** – de management-resource die u gebruiken voor het maken van de volgorde van de apparaten, het apparaat configureren en vervolgens de volgorde voor voltooiing bijhouden.
- **Data Box-gatewayapparaat** : het virtuele apparaat dat u hebt ingericht in de hypervisor van het systeem die u hebt opgegeven. Dit virtuele apparaat wordt gebruikt om uw on-premises gegevens importeren in Azure.
- **Clients /-hosts die zijn verbonden met het apparaat** – de clients in uw infrastructuur die verbinding maken met de gegevens in het Gateway-apparaat en die gegevens bevatten die moet worden beveiligd.
- **Cloudopslag** – De locatie in de Azure-cloud waar gegevens worden opgeslagen. Deze locatie is meestal het opslagaccount dat is gekoppeld aan de gegevens in het Gateway-resource die u hebt gemaakt.


## <a name="data-box-gateway-service-protection"></a>De beveiliging van gegevens in het Gateway-service

De gegevens in het Gateway-service is een management-service die wordt gehost in Microsoft Azure. De service wordt gebruikt voor het configureren en beheren van het apparaat.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-service-protection.md)]

## <a name="data-box-gateway-device-protection"></a>Bescherming van gegevens in het Gateway-apparaat

De gegevens in het Gateway-apparaat is een virtueel apparaat is ingericht in de hypervisor van een on-premises systeem die u opgeeft. Het apparaat helpt bij het verzenden van gegevens naar Azure. Uw apparaat:

- Moet een activeringscode voor toegang tot de gegevens in Edge/gegevens in het Gateway-service.
- Wordt beveiligd op altijd door het wachtwoord van een apparaat.
<!---  secure boot enabled.
- Runs Windows Defender Device Guard. Device Guard allows you to run only trusted applications that you define in your code integrity policies.-->

### <a name="protect-the-device-via-activation-key"></a>Het apparaat via de activeringscode beveiligen

Alleen een geautoriseerde Data Box-Gateway-apparaat is toegestaan om toe te voegen van de gegevens in het Gateway-service die u hebt gemaakt in uw Azure-abonnement. Als u wilt toestaan dat een apparaat, moet u een activeringscode te activeren van het apparaat met de gegevens in het Gateway-service.

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-activation-key.md)]

Ga voor meer informatie naar [ophalen van een activeringscode](data-box-gateway-deploy-prep.md#get-the-activation-key).

### <a name="protect-the-device-via-password"></a>Het apparaat via wachtwoord beveiligen

Wachtwoorden ervoor te zorgen dat uw gegevens toegankelijk voor alleen gemachtigde gebruikers is. Gegevens in het Gateway-apparaten opstart in een vergrendelde status.

U kunt:

- Verbinding maken met de lokale web-UI van het apparaat via een browser en geef vervolgens een wachtwoord aan te melden bij het apparaat.
- Op afstand verbinding maken met de PowerShell-interface van het apparaat via HTTP. Extern beheer is standaard ingeschakeld. Vervolgens kunt u het wachtwoord van het apparaat aan te melden bij het apparaat opgeven. Ga voor meer informatie naar [verbinding maken met uw gegevens in het Gateway-apparaat op afstand](data-box-gateway-connect-powershell-interface.md#connect-to-the-powershell-interface).

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-password-best-practices.md)]
- Gebruik de lokale web-UI te [het wachtwoord wijzigen](data-box-gateway-manage-access-power-connectivity-mode.md#manage-device-access). Als u het wachtwoord wijzigt, zorg er dan voor dat alle externe toegang-gebruikers een melding ontvangen zodat ze zich niet een aanmelding mislukt.


## <a name="protect-the-data"></a>De gegevens te beveiligen

Deze sectie beschrijft de beveiligingsfuncties van Data Box-Gateway waarmee de gegevens die onderweg zijn en opgeslagen gegevens worden beveiligd.

### <a name="protect-data-at-rest"></a>Gegevens in rust beveiligen

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-rest.md)]

### <a name="protect-data-in-flight"></a>Beveiligen van gegevens tijdens de overdracht

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-data-flight.md)]

### <a name="protect-data-via-storage-accounts"></a>Bescherm gegevens via de storage-accounts

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-protect-data-storage-accounts.md)]
- Draaien en vervolgens [uw opslagaccountsleutels synchroniseren](data-box-gateway-manage-shares.md#sync-storage-keys) regelmatig om ervoor te zorgen dat uw storage-account niet is geopend door onbevoegde gebruikers.

## <a name="manage-personal-information"></a>Persoonlijke gegevens beheren

De gegevens in het Gateway-service worden verzameld van persoonlijke gegevens in de volgende belangrijke gevallen:

[!INCLUDE [data-box-edge-gateway-data-rest](../../includes/data-box-edge-gateway-manage-personal-data.md)]

Volg de stappen in de lijst van gebruikers die toegang tot of verwijderen van een share wilt weergeven, [shares op de Data Box-Gateway beheren](data-box-gateway-manage-shares.md).

Bekijk het Microsoft-privacybeleid in het [Vertrouwenscentrum](https://www.microsoft.com/trustcenter) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

[Data Box-Gateway-apparaat implementeren](data-box-gateway-deploy-prep.md).
