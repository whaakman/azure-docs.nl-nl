---
title: Problemen met een Azure VPN-verbinding voor site-naar-site die kan geen verbinding maken | Microsoft Docs
description: Informatie over het oplossen van problemen met een site-naar-site VPN-verbinding die plotseling werkt niet en kan niet worden hersteld.
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/13/2017
ms.author: genli
ms.openlocfilehash: 96a1705d651b9a2d17a466b9c43721bec7b4972c
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="troubleshooting-an-azure-site-to-site-vpn-connection-cannot-connect-and-stops-working"></a>Voor probleemoplossing: Een Azure site-naar-site VPN-verbinding kan geen verbinding maken en werkt niet

Nadat u een site-naar-site VPN-verbinding tussen een on-premises netwerk en een Azure-netwerk configureert, wordt de VPN-verbinding plotseling werkt niet en kan niet worden hersteld. Dit artikel bevat de stappen voor probleemoplossing waarmee u kunt dit probleem oplossen. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a>Stappen voor het oplossen van problemen

U lost het probleem door eerst [opnieuw instellen van de Azure VPN-gateway](vpn-gateway-resetgw-classic.md) en de tunnel van de on-premises VPN-apparaat opnieuw instellen. Als het probleem zich blijft voordoen, volg deze stappen om de oorzaak van het probleem te identificeren.

### <a name="prerequisite-step"></a>Vereiste stap

Controleer het type van de Azure VPN-gateway.

1. Ga naar de [Azure Portal](https://portal.azure.com).

2. Controleer de **overzicht** pagina van de VPN-gateway voor de type-informatie.
    
    ![Overzicht van de gateway](media\vpn-gateway-troubleshoot-site-to-site-cannot-connect\gatewayoverview.png)

### <a name="step-1-check-whether-the-on-premises-vpn-device-is-validated"></a>Step 1. Controleer of de on-premises VPN-apparaat wordt gevalideerd

1. Controleer of u een [gevalideerde VPN-apparaat en de versie van besturingssysteem](vpn-gateway-about-vpn-devices.md#devicetable). Als het apparaat niet gevalideerde VPN-apparaat, moet u wellicht contact op met de fabrikant van het apparaat om te zien of er een compatibiliteitsprobleem.

2. Zorg ervoor dat het VPN-apparaat correct is geconfigureerd. Zie voor meer informatie [bewerken van apparaatconfiguraties](vpn-gateway-about-vpn-devices.md#editing).

### <a name="step-2-verify-the-shared-key"></a>Stap 2. Controleer of de gedeelde sleutel

Vergelijk de gedeelde sleutel voor de on-premises VPN-apparaat met het virtuele netwerk voor Azure VPN om ervoor te zorgen dat de sleutels overeenkomen. 

Als u wilt weergeven van de gedeelde sleutel voor de Azure VPN-verbinding, moet u een van de volgende methoden gebruiken:

**Azure Portal**

1. Ga naar de VPN-gateway site-naar-site-verbinding die u hebt gemaakt.

2. In de **instellingen** sectie, klikt u op **gedeelde sleutel**.
    
    ![Gedeelde sleutel](media/vpn-gateway-troubleshoot-site-to-site-cannot-connect/sharedkey.png)

**Azure PowerShell**

Voor het Azure Resource Manager-implementatiemodel:

    Get-AzureRmVirtualNetworkGatewayConnectionSharedKey -Name <Connection name> -ResourceGroupName <Resource group name>

Voor het klassieke implementatiemodel:

    Get-AzureVNetGatewayKey -VNetName -LocalNetworkSiteName

### <a name="step-3-verify-the-vpn-peer-ips"></a>Stap 3. Controleer of de VPN-peer-IP-adressen

-   De IP-definitie in de **Local Network Gateway** -object in Azure, moet overeenkomen met het lokale apparaat IP-adres.
-   De Azure-gateway IP-definitie die is ingesteld op de on-premises-apparaat moet overeenkomen met het Azure-gateway IP-adres.

### <a name="step-4-check-udr-and-nsgs-on-the-gateway-subnet"></a>Stap 4. Controleer UDR en nsg's op het gatewaysubnet

Controleren op en verwijderen van de gebruiker gedefinieerde Routering of Netwerkbeveiligingsgroepen (nsg's) op het gatewaysubnet en vervolgens het resultaat te testen. Als het probleem opgelost is, controleert u de instellingen die UDR of NSG toegepast.

### <a name="step-5-check-the-on-premises-vpn-device-external-interface-address"></a>Stap 5. Controleer het adres van de lokale VPN-apparaat externe interface

- Als het internetgerichte IP-adres van de VPN-apparaat is opgenomen in de **lokale netwerk** definitie in Azure, kunt u tegenkomen sporadisch worden verbroken.
- De externe interface van het apparaat moet rechtstreeks op het Internet. Er zijn geen netwerkadresomzetting of de firewall tussen het Internet en het apparaat.
- Firewall clustering als u een virtueel IP-adres wilt configureren, moet u opsplitsen van het cluster en de VPN-apparaat rechtstreeks naar een openbare interface die de gateway kan verbinding maken met weer te geven.

### <a name="step-6-verify-that-the-subnets-match-exactly-azure-policy-based-gateways"></a>Stap 6. Controleer of de subnetten exact overeenkomen (Azure op beleid gebaseerde gateways)

-   Controleer of dat het virtuele netwerk adres space(s) precies tussen de virtuele Azure-netwerk en de definities van de lokale overeenkomen.
-   Controleer of de subnetten tussen exact overeenkomen met de **Local Network Gateway** en on-premises definities voor de on-premises netwerk.

### <a name="step-7-verify-the-azure-gateway-health-probe"></a>Stap 7. Controleer of de statuscontrole van de Azure-gateway

1. Ga naar de health-test.

2. Klik in de certificaatwaarschuwing.
3. Als u een antwoord ontvangt, wordt de VPN-gateway als in orde beschouwd. Als u niet een antwoord ontvangt, de gateway mogelijk niet in orde of het probleem wordt veroorzaakt door een NSG op het gatewaysubnet. De volgende tekst is een voorbeeldantwoord:

    &lt;? XML-versie = "1.0"? > <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">primaire exemplaar: GatewayTenantWorker_IN_1 GatewayTenantVersion: 14.7.24.6 < / tekenreeks&gt;

### <a name="step-8-check-whether-the-on-premises-vpn-device-has-the-perfect-forward-secrecy-feature-enabled"></a>Stap 8. Controleer of de on-premises VPN-apparaat de functie perfect forward secrecy is ingeschakeld heeft

De functie perfect forward secrecy kunt verbreken problemen veroorzaken. Als het VPN-apparaat perfect forward secrecy is ingeschakeld heeft, moet u de functie uitschakelen. Werk vervolgens de VPN-gateway IPSec-beleid.

## <a name="next-steps"></a>Volgende stappen

-   [Een site-naar-site-verbinding met een virtueel netwerk configureren](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
-   [Beleid voor IPsec/IKE voor site-naar-site VPN-verbindingen configureren](vpn-gateway-ipsecikepolicy-rm-powershell.md)
