---
title: Over de cryptografische vereisten en Azure VPN-gateways | Microsoft Docs
description: Dit artikel wordt beschreven cryptografische vereisten en Azure VPN-gateways
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/22/2017
ms.author: yushwang
ms.openlocfilehash: d2f3da47f1d4eebe1b81964790ff6612dd78155d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="about-cryptographic-requirements-and-azure-vpn-gateways"></a>Over de cryptografische vereisten en Azure VPN-gateways

Dit artikel wordt beschreven hoe u Azure VPN-gateways om te voldoen aan de vereisten van uw cryptografische voor cross-premises S2S VPN-tunnels en VNet-naar-VNet-verbindingen in Azure kunt configureren. 

## <a name="about-ipsec-and-ike-policy-parameters-for-azure-vpn-gateways"></a>Informatie over IPsec en IKE beleidsparameters voor Azure VPN-gateways
IPsec en IKE-protocol standaard ondersteunt een groot aantal cryptografische algoritmen in verschillende combinaties. Als een specifieke combinatie van cryptografische algoritmen en parameters klanten geen aanvragen, gebruikt u Azure VPN-gateways een reeks standaard voorstellen. De standaard beleid sets zijn wilt maximaliseren interoperabiliteit met een breed scala aan derden VPN-apparaten in de standaardconfiguraties. Als gevolg hiervan, dekken het beleid en het aantal voorstellen niet alle mogelijke combinaties van beschikbare cryptografische algoritmen en de belangrijkste sterkte.

Het standaardbeleid voor Azure VPN-gateway wordt weergegeven in het document is ingesteld: [over VPN-apparaten en IPsec/IKE-parameters voor de Site-naar-Site-VPN-gatewayverbindingen](vpn-gateway-about-vpn-devices.md).

## <a name="cryptographic-requirements"></a>Cryptografische vereisten
Voor communicatie waarvoor specifieke cryptografische algoritmen of parameters, meestal als gevolg van naleving of beveiligingsvereisten kunnen klanten nu configureren hun Azure VPN-gateways voor het gebruik van een aangepast beleid voor IPsec/IKE met specifieke cryptografische algoritmen en kracht van de plaats van Hiermee stelt u het standaardbeleid voor Azure.

Bijvoorbeeld, het beleid van de hoofdmodus IKEv2 voor Azure VPN-gateways alleen Diffie-Hellman-groep 2 (1024 bits) gebruiken dat klanten willen sterkere groepen worden gebruikt in IKE, zoals groep 14 (2048-bits), groep 24 (2048-bits MODP groep) of ECP (elliptic curve groepen) opgeven 256 of 384 bits (een groep 19 en 20 groep, respectievelijk). Vergelijkbare vereisten voor IPSec-snelle modus beleid ook van toepassing.

## <a name="custom-ipsecike-policy-with-azure-vpn-gateways"></a>Aangepaste IPsec/IKE-beleid met Azure VPN-gateways
Nu ondersteuning voor Azure VPN-gateways per verbinding, aangepaste IPsec/IKE-beleid. Voor een Site-naar-Site of een VNet-naar-VNet-verbinding kunt u een specifieke combinatie van cryptografische algoritmen voor IPsec en IKE met de gewenste sleutelsterkte, zoals wordt weergegeven in het volgende voorbeeld:

![ike-IPSec-beleid](./media/vpn-gateway-about-compliance-crypto/ipsecikepolicy.png)

U kunt een IPsec/IKE-beleid maken en toepassen op een nieuwe of bestaande verbinding. 

### <a name="workflow"></a>Werkstroom

1. De virtuele netwerken, VPN-gateways of lokale netwerkgateways voor uw topologie connectiviteit zoals beschreven in andere instructiedocumenten maken
2. Een IPsec/IKE-beleid maken
3. U kunt het beleid toepassen wanneer u een S2S of VNet-naar-VNet-verbinding maken
4. Als de verbinding al gemaakt is, kunt u deze toepassing of het beleid bijwerken naar een bestaande verbinding


## <a name="ipsecike-policy-faq"></a>Beleid voor IPsec/IKE Veelgestelde vragen

[!INCLUDE [vpn-gateway-ipsecikepolicy-faq-include](../../includes/vpn-gateway-faq-ipsecikepolicy-include.md)]


## <a name="next-steps"></a>Volgende stappen
Zie [configureren IPsec/IKE-beleid](vpn-gateway-ipsecikepolicy-rm-powershell.md) voor stapsgewijze instructies over het configureren van aangepaste IPsec/IKE-beleid op een verbinding.

Zie ook [meerdere op beleid gebaseerde VPN-apparaten aansluiten](vpn-gateway-connect-multiple-policybased-rm-ps.md) voor meer informatie over de optie UsePolicyBasedTrafficSelectors.