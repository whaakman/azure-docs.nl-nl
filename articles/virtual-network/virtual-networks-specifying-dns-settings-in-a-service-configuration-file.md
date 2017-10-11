---
title: DNS-instellingen opgeven in een serviceconfiguratiebestand | Microsoft Docs
description: aangepaste DNS-instellingen voor virtueel netwerk met behulp van serviceconfiguratiebestand opgeven
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 467a4b99-8691-40b3-b640-e25e49675c71
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2016
ms.author: jdial
ms.openlocfilehash: 0fba2ea06827aff29a7a092933edb8120d668b29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>DNS-instellingen in een serviceconfiguratiebestand opgeven
## <a name="dns-elements"></a>DNS-elementen
Een service-configuratiebestand mag bevatten een DnsServers-element met een lijst met IPv4-adressen voor de Domain Name System (DNS)-servers die door de service wordt gebruikt. Instellingen in het configuratiebestand van de service hebben voorrang boven de instellingen in het configuratiebestand van het netwerk. Zie voor meer informatie [configuratieschema voor Azure-Service (.cscfg-bestand)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration-element**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

> [!WARNING]
> De **naam** kenmerk in de **DnsServer** element alleen als een verwijzingsnaam wordt gebruikt. Vertegenwoordigt niet de hostnaam voor de DNS-server. Elke **DnsServer** kenmerkwaarde moet uniek zijn in het hele Microsoft Azure-abonnement.
> 
> 

## <a name="see-also"></a>Zie ook
[Het configuratieschema Azure-Service (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Azure Virtual Network-configuratieschema](http://go.microsoft.com/fwlink/?LinkId=248093)

[Een virtueel netwerk met behulp van de configuratiebestanden netwerk configureren](http://go.microsoft.com/fwlink/?LinkId=248094)

[Over de instellingen van het virtuele netwerk in de beheerportal](http://go.microsoft.com/fwlink/?LinkId=248092)

