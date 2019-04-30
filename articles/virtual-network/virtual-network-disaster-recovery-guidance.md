---
title: Bedrijfscontinuïteit voor virtueel netwerk | Microsoft Docs
description: Meer informatie over wat te doen in het geval van een onderbreking van de Azure-service die invloed hebben op Azure Virtual Networks.
services: virtual-network
documentationcenter: ''
author: NarayanAnnamalai
manager: jefco
editor: ''
ms.assetid: ad260ab9-d873-43b3-8896-f9a1db9858a5
ms.service: virtual-network
ms.workload: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: narayan;aglick
ms.openlocfilehash: f8068524a33c6d29056af456a26b8bd87fc3e3d6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60742700"
---
# <a name="virtual-network--business-continuity"></a>Virtueel netwerk: zakelijke continuïteit

## <a name="overview"></a>Overzicht
Een Virtueelnetwerk (VNet) is een logische weergave van uw netwerk in de cloud. Hiermee kunt u voor het definiëren van uw eigen privé IP-adresruimte en het netwerk segmenteren in subnetten. Vnet's fungeert als grens van een vertrouwensrelatie voor het hosten van uw rekenresources, zoals Azure Virtual Machines en Cloudservices (web/worker-rollen). Een VNet kan direct IP privécommunicatie tussen de bronnen die wordt gehost. U kunt een virtueel netwerk koppelen aan een on-premises netwerk via een VPN-Gateway of ExpressRoute.

Een VNet wordt gemaakt binnen het bereik van een regio. U kunt vnet's maken met dezelfde-adresruimte in twee verschillende regio's (bijvoorbeeld VS-Oost en VS West), maar kan geen verbinding maken ze samen. 

## <a name="business-continuity"></a>Bedrijfscontinuïteit

Het is mogelijk dat er verschillende manieren dat uw toepassing kan worden onderbroken. Een regio kan volledig worden geknipt uitgeschakeld vanwege een natuurramp of een gedeeltelijke ramp, vanwege een fout van meerdere apparaten of services. De impact op de VNet-service is anders in elk van deze situaties.

**V: Wat moet ik doen als er een storing optreedt voor een hele regio? Bijvoorbeeld, als een regio volledig is afgekapt vanwege een natuurramp? Wat gebeurt er met de virtuele netwerken die worden gehost in de regio?**

A: Het virtuele netwerk en de resources in de betreffende regio blijft niet toegankelijk tijdens het ophalen van de service wordt onderbroken.

![Eenvoudige virtueel-netwerkdiagram](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**V: Wat kan ik hetzelfde virtuele netwerk in een andere regio opnieuw te maken?**

A: Virtuele netwerken zijn redelijk lichtgewicht bronnen. U kunt Azure API's voor het maken van een VNet met dezelfde-adresruimte in een andere regio aanroepen. Als u dezelfde omgeving die aanwezig in het betrokken gebied was opnieuw, wilt u API-aanroepen om de web-Cloud Services en -werkrollen en de virtuele machines die u opnieuw te implementeren. Als u on-premises connectiviteit hebt, hebt zoals in een hybride implementatie, u een nieuwe VPN-Gateway implementeren en verbinding maken met uw on-premises netwerk.

Zie voor het maken van een virtueel netwerk, [maken van een virtueel netwerk](manage-virtual-network.md#create-a-virtual-network).

**V: Kan een replica van een VNet in een bepaalde regio worden opnieuw gemaakt in een andere regio tevoren?**

A: Ja, kunt u twee VNets met de dezelfde persoonlijke IP-adresruimte en resources in twee verschillende regio's vooraf maken. Als u internet gerichte services in het VNet host, kan hebt u van Traffic Manager ingesteld op gebruik geografische routering van verkeer naar de regio die actief is. U kan niet echter twee VNets verbinden met dezelfde-adresruimte op uw on-premises netwerk, als dit leiden problemen met routering tot zou. Op het moment van een ramp en het verlies van een VNet in één regio, kunt u het andere VNet in de beschikbare regio verbinden met de overeenkomende adresruimte op uw on-premises netwerk.

Zie voor het maken van een virtueel netwerk, [maken van een virtueel netwerk](manage-virtual-network.md#create-a-virtual-network).

