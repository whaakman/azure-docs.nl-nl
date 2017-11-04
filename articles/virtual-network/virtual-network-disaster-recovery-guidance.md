---
title: Wat te doen in het geval van een onderbreking van de Azure-service die invloed hebben op Azure Virtual Networks | Microsoft Docs
description: Meer informatie over wat te doen in het geval van een onderbreking van de Azure-service die invloed hebben op Azure Virtual Networks.
services: virtual-network
documentationcenter: 
author: NarayanAnnamalai
manager: jefco
editor: 
ms.assetid: ad260ab9-d873-43b3-8896-f9a1db9858a5
ms.service: virtual-network
ms.workload: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: narayan;aglick
ms.openlocfilehash: 4e125406d2e798138c45e3fbbf61a610afab69fc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="virtual-network--business-continuity"></a>Virtueel netwerk – bedrijfscontinuïteit
## <a name="overview"></a>Overzicht
Een virtueel netwerk (VNet) is een logische representatie van uw netwerk in de cloud. Hiermee kunt u voor het definiëren van uw eigen persoonlijke IP-adresruimte en het netwerk te segmenteren in subnetten. VNets fungeert als de grens van een vertrouwensrelatie voor het hosten van uw rekenresources zoals Azure Virtual Machines en Cloud-Services (web/werkrollen). Een VNet kunt rechtstreekse persoonlijke IP-communicatie tussen de bronnen die worden gehost in het. Een virtueel netwerk kan ook worden gekoppeld aan een on-premises netwerk via een van de hybride-opties, zoals een VPN-Gateway of ExpressRoute.

Een VNet wordt gemaakt binnen het bereik van een regio. U kunt VNets maken met dezelfde adresruimte in twee verschillende regio's (dat wil zeggen VS-Oost en VS-West, maar kan geen verbinding maken ze met elkaar rechtstreeks). 

## <a name="business-continuity"></a>Bedrijfscontinuïteit
Er zijn verschillende manieren dat uw toepassing kan worden onderbroken. Een bepaald gebied kan worden volledig afgekapt als gevolg van een natuurramp of een gedeeltelijke noodgeval vanwege een fout van meerdere apparaten/services. De gevolgen voor de VNet-service is anders in elk van deze situaties.

**V: wat moet u doen in het geval van een storing voor een hele regio? dat wil zeggen als een regio volledig ingestelde vanwege een natuurramp is? Wat gebeurt er met de virtuele netwerken die worden gehost in de regio?**

A: het virtuele netwerk en de bronnen in de desbetreffende regio niet toegankelijk is gedurende de tijd van de service wordt onderbroken.

![Eenvoudige virtuele-netwerkdiagram](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**V: wat kan ik hetzelfde virtuele netwerk in een andere regio opnieuw te maken?**

A: virtueel netwerk (VNet) is redelijk lightweight bron. U kunt Azure API's voor het maken van een VNet met dezelfde adresruimte in een andere regio aanroepen. Om dezelfde omgeving die aanwezig in de desbetreffende regio was opnieuw te maken, hebt u API-aanroepen uw Cloudservices (web/werkrollen) en virtuele Machines die u had opnieuw te implementeren. U moet ook draaien van een VPN-Gateway en verbinding maken met uw on-premises netwerk als u had lokale connectiviteit (zoals in een hybride implementatie).

Vindt u de instructies voor het maken van een VNet [hier](virtual-networks-create-vnet-arm-pportal.md). 

**V: kan een replica van een VNet in een bepaald gebied niet opnieuw worden gemaakt in een andere regio tevoren?**

A: Ja, kunt u twee VNets met de dezelfde persoonlijke IP-adresruimte en resources in twee verschillende regio's tevoren maken. Als een klant internetgericht services in het VNet host is, kunnen die zijn ingesteld van Traffic Manager geo-om verkeer te leiden naar de regio die actief is. Echter, een klant twee VNets kan geen verbinding met dezelfde adresruimte naar hun on-premises netwerk als dit routering problemen veroorzaakt. Op het moment van een noodgeval en verlies van een VNet in één regio is, kunt een klant het andere VNet in de beschikbare regio verbinden met de overeenkomende adresruimte op hun on-premises netwerk.

Vindt u de instructies voor het maken van een VNet [hier](virtual-networks-create-vnet-arm-pportal.md).

