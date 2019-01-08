---
title: Over cryptografische vereisten en Azure VPN-gateways | Microsoft Docs
description: In dit artikel wordt beschreven cryptografische vereisten en Azure VPN-gateways
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/22/2017
ms.author: yushwang
ms.openlocfilehash: 060e647badcc3bad7b44d7cef3530c36b8ecdf57
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "23884249"
---
# <a name="about-cryptographic-requirements-and-azure-vpn-gateways"></a>Over cryptografische vereisten en Azure VPN-gateways

In dit artikel wordt beschreven hoe u Azure VPN-gateways om te voldoen aan uw cryptografische vereisten voor S2S-VPN-tunnels voor cross-premises en VNet-naar-VNet-verbindingen in Azure kunt configureren. 

## <a name="about-ipsec-and-ike-policy-parameters-for-azure-vpn-gateways"></a>Informatie over IPsec en IKE-beleid-parameters voor Azure VPN-gateways
IPsec en IKE-protocol standaard ondersteunt een breed scala aan cryptografische algoritmen in verschillende combinaties. Als klanten geen een specifieke combinatie van cryptografische algoritmen en parameters aanvragen, gebruikt Azure VPN-gateways een reeks standaard voorstellen. De standaard-beleidssets zijn gekozen voor een maximale compatibiliteit met een breed scala aan VPN-apparaten van derden in de standaardconfiguraties. Als gevolg hiervan dekken de beleidsregels en het aantal voorstellen niet alle mogelijke combinaties van beschikbare cryptografische algoritmen en belangrijkste sterke punten.

Het standaardbeleid instellen voor Azure VPN-gateway wordt weergegeven in het document: [Over VPN-apparaten en IPsec/IKE-parameters voor Site-naar-Site VPN-gatewayverbindingen](vpn-gateway-about-vpn-devices.md).

## <a name="cryptographic-requirements"></a>Cryptografische vereisten
Voor communicatie waarvoor specifieke cryptografische algoritmen of parameters, kunnen meestal vanwege nalevings- of beveiligingsvereisten, klanten nu configureren hun Azure VPN-gateways voor het gebruik van een aangepast IPsec/IKE-beleid met specifieke cryptografische algoritmen en belangrijkste sterke punten dan de standaard Azure-beleidssets.

Bijvoorbeeld, de IKEv2 hoofdmodus beleid voor Azure VPN-gateways maken gebruik van alleen Diffie-Hellman-groep 2 (1024 bits), terwijl klanten mogelijk om op te geven sterkere groepen moet worden gebruikt in IKE, zoals groep 14 (2048-bits), groep 24 uur per dag (groep 2048-bits MODP) of ECP (elliptic curve groepen) 256 of 384 bits (19 en 20 van de groep, groep respectievelijk). Dezelfde vereisten gelden ook beleid voor IPsec snelle modus.

## <a name="custom-ipsecike-policy-with-azure-vpn-gateways"></a>Aangepast IPsec/IKE-beleid met Azure VPN-gateways
Azure VPN-gateways bieden nu ondersteuning per verbinding, aangepast IPsec/IKE-beleid. Voor een Site-naar-Site of VNet-naar-VNet-verbinding kunt u een specifieke combinatie van cryptografische algoritmen voor IPsec en IKE met de gewenste sleutelsterkte, zoals wordt weergegeven in het volgende voorbeeld:

![ike-IPSec-beleid](./media/vpn-gateway-about-compliance-crypto/ipsecikepolicy.png)

U kunt een IPsec/IKE-beleid maken en toepassen op een nieuwe of bestaande verbinding. 

### <a name="workflow"></a>Werkstroom

1. De virtuele netwerken, VPN-gateways of lokale netwerkgateways voor de topologie van uw verbinding zoals beschreven in andere procedures voor documenten maken
2. Een IPsec/IKE-beleid maken
3. U kunt het beleid toepassen bij het maken van een S2S- of VNet-naar-VNet-verbinding
4. Als de verbinding al gemaakt is, kunt u van toepassing of het beleid aan een bestaande verbinding bijwerken


## <a name="ipsecike-policy-faq"></a>Veelgestelde vragen over voor IPsec/IKE-beleid

[!INCLUDE [vpn-gateway-ipsecikepolicy-faq-include](../../includes/vpn-gateway-faq-ipsecikepolicy-include.md)]


## <a name="next-steps"></a>Volgende stappen
Zie [configureren IPsec/IKE-beleid](vpn-gateway-ipsecikepolicy-rm-powershell.md) voor stapsgewijze instructies over het configureren van aangepast IPsec/IKE-beleid op een verbinding.

Zie ook [verbinding maken met meerdere op beleid gebaseerde VPN-apparaten](vpn-gateway-connect-multiple-policybased-rm-ps.md) voor meer informatie over de optie UsePolicyBasedTrafficSelectors.