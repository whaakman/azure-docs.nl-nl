---
title: "Virtueel netwerk zakelijke continuïteit | Microsoft Docs"
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
ms.openlocfilehash: d993144006d1fb17d79ffee4f2da538264a309a4
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="virtual-network--business-continuity"></a>Virtueel netwerk – bedrijfscontinuïteit

## <a name="overview"></a>Overzicht
Een virtueel netwerk (VNet) is een logische representatie van uw netwerk in de cloud. Hiermee kunt u voor het definiëren van uw eigen persoonlijke IP-adresruimte en het netwerk te segmenteren in subnetten. VNets fungeert als de grens van een vertrouwensrelatie voor het hosten van uw rekenresources zoals Azure Virtual Machines en Cloud-Services (web/werkrollen). Een VNet kunt rechtstreekse persoonlijke IP-communicatie tussen de bronnen die worden gehost in het. U kunt een virtueel netwerk koppelen aan een on-premises netwerk via een VPN-Gateway of ExpressRoute.

Een VNet wordt gemaakt binnen het bereik van een regio. U kunt VNets maken met dezelfde adresruimte in twee verschillende regio's (bijvoorbeeld VS-Oost en VS-West), maar kan geen verbinding maken ze samen. 

## <a name="business-continuity"></a>Bedrijfscontinuïteit

Er zijn verschillende manieren dat uw toepassing kan worden onderbroken. Een regio kan worden volledig afgekapt als gevolg van een natuurramp of een gedeeltelijke ramp, vanwege een fout van meerdere apparaten of services. De gevolgen voor de VNet-service is anders in elk van deze situaties.

**V: wat moet ik doen als er een storing optreedt voor een hele regio? Als bijvoorbeeld een regio volledig is afgekapt vanwege een natuurramp? Wat gebeurt er met de virtuele netwerken die worden gehost in de regio?**

A: het virtuele netwerk en de resources in de desbetreffende regio niet toegankelijk is gedurende de tijd van de service wordt onderbroken.

![Eenvoudige virtuele-netwerkdiagram](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**V: wat kan ik hetzelfde virtuele netwerk in een andere regio opnieuw te maken?**

A: virtuele netwerken zijn redelijk lightweight resources. U kunt Azure API's voor het maken van een VNet met dezelfde adresruimte in een andere regio aanroepen. Om dezelfde omgeving die aanwezig in de desbetreffende regio was opnieuw te maken, u het API-aanroepen te implementeren voor de Cloud Services-web- en werkrollen- en de virtuele machines die u had. Als u lokale connectiviteit hebt, hebt zoals in een hybride implementatie, u een nieuwe VPN-Gateway implementeren en verbinding maken met uw on-premises netwerk.

Zie het maken van een virtueel netwerk [een virtueel netwerk maken](manage-virtual-network.md#create-a-virtual-network).

**V: kan een replica van een VNet in een bepaald gebied niet opnieuw worden gemaakt in een andere regio tevoren?**

A: Ja, kunt u twee VNets met de dezelfde persoonlijke IP-adresruimte en resources in twee verschillende regio's tevoren maken. Als u services in het VNet internetgerichte host, kan hebben stelt u up Traffic Manager geo-om verkeer te leiden naar de regio die actief is. Twee VNets met dezelfde adresruimte van naar uw on-premises netwerk zijn echter niet verbinden omdat dit routering problemen veroorzaakt. Op het moment van een noodgeval en verlies van een VNet in één regio is, kunt u het andere VNet in de beschikbare regio verbinden met de overeenkomstige adresruimte naar uw on-premises netwerk.

Zie het maken van een virtueel netwerk [een virtueel netwerk maken](manage-virtual-network.md#create-a-virtual-network).

