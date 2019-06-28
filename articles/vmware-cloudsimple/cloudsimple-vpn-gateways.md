---
title: VPN-gateways in VMware CloudSimple - Azure-oplossing
description: Meer informatie over CloudSimple site-naar-site VPN- en punt-naar-site VPN-concepten
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: a91d0f75b7e27065bd2ee0ea430a859d265afacb
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332037"
---
# <a name="vpn-gateways-overview"></a>Overzicht van VPN-gateways

Een VPN-gateway wordt gebruikt voor het verzenden van versleuteld verkeer tussen een netwerk CloudSimple regio op een on-premises locatie of een computer via het openbare Internet.  Elke regio kan slechts één VPN-gateway hebben. U kunt echter meerdere verbindingen met dezelfde VPN-gateway maken. Wanneer u meerdere verbindingen naar dezelfde VPN-gateway hebt gemaakt, delen alle VPN-tunnels de bandbreedte die voor de gateway beschikbaar is.

CloudSimple bevat twee soorten VPN-gateways:

* Site-naar-site VPN-Gateway
* Punt-naar-site VPN-Gateway

## <a name="site-to-site-vpn-gateway"></a>Site-naar-site VPN-gateway

Een site-naar-site VPN-gateway wordt gebruikt voor het verzenden van versleuteld verkeer tussen een regio CloudSimple netwerk en een on-premises datacenter. Deze verbinding gebruiken voor het definiëren van de subnetten/CIDR-bereik, voor de communicatie tussen uw on-premises netwerk en het netwerk van de regio CloudSimple.

De VPN-gateway kunt u gebruiken voor services van on-premises in uw privécloud, en services op uw persoonlijke cloud, van de on-premises netwerk.  CloudSimple biedt een op beleid gebaseerde VPN-server voor het maken van verbinding van uw on-premises netwerk.

Voor site-naar-site VPN-verbinding van use cases zijn onder andere:

* Toegankelijkheid van uw privécloud vCenter vanaf elk werkstation in uw on-premises netwerk.
* Gebruik van uw on-premises Active Directory als de bron van een vCenter-identiteit.
* Handige overdracht van VM-sjablonen, ISO's en andere bestanden van uw on-premises resources met de vCenter-privécloud.
* Toegankelijkheid van workloads die worden uitgevoerd op uw privécloud van uw on-premises netwerk.

![Site-naar-Site VPN-verbinding-topologie](media/cloudsimple-site-to-site-vpn-connection.png)

> [!IMPORTANT]
> U moet MSS TCP vastzetten op 1078 bytes of lager. Of als uw VPN-apparaten het vastzetten van MSS niet ondersteunen, kunt u ook instellen de MTU op de tunnelinterface 1118 bytes in plaats daarvan. 

### <a name="cryptographic-parameters"></a>Cryptografische parameters

Een site-naar-site VPN-verbinding maakt gebruik van de volgende standaard cryptografische parameters om een beveiligde verbinding te maken.  Wanneer u een verbinding van on-premises VPN-apparaat maakt, worden de parameters moeten overeenkomen.

Een site-naar-site VPN-verbinding maakt gebruik van de volgende standaard cryptografische parameters om een beveiligde verbinding te maken.  Wanneer u een verbinding van on-premises VPN-apparaat maakt, gebruikt u een van de volgende parameters die worden ondersteund door uw on-premises VPN-gateway.

#### <a name="phase-1-proposals"></a>Fase 1 voorstellen

| Parameter | Voorstel 1 | Voorstel 2 | Voorstel 3 |
|-----------|------------|------------|------------|
| IKE-versie | IKEv1 | IKEv1 | IKEv1 |
| Versleuteling | AES 128 | AES 256 | AES 256 |
| Hash-algoritme| SHA 256 | SHA 256 | SHA 1 |
| Diffie-Hellman-groep (DH-groep) | 2 | 2 | 2 |
| Levensduur | 28.800 seconden | 28.800 seconden | 28.800 seconden |
| Gegevensgrootte | 4 GB | 4 GB | 4 GB |


#### <a name="phase-2-proposals"></a>Fase 2 voorstellen 

| Parameter | Voorstel 1 | Voorstel 2 | Voorstel 3 |
|-----------|------------|------------|------------|
| Versleuteling | AES 128 | AES 256 | AES 256 |
| Hash-algoritme| SHA 256 | SHA 256 | SHA 1 |
| Perfect Forward Secrecy groep (PFS-groep) | Geen | Geen | Geen |
| Levensduur | 1\.800 seconden | 1\.800 seconden | 1\.800 seconden |
| Gegevensgrootte | 4 GB | 4 GB | 4 GB |

## <a name="point-to-site-vpn-gateway"></a>Punt-naar-site VPN-gateway

Een punt-naar-site-VPN wordt gebruikt voor het verzenden van versleuteld verkeer tussen een regio CloudSimple netwerk en een clientcomputer.  Punt-naar-site VPN is de eenvoudigste manier om toegang tot uw persoonlijke cloudnetwerk, met inbegrip van uw privécloud vCenter en workload-VM's.  Punt-naar-site VPN-verbinding gebruiken als u verbinding met de privécloud op afstand maakt.

## <a name="next-steps"></a>Volgende stappen

* [VPN-gateway instellen](https://docs.azure.cloudsimple.com/vpn-gateway/)