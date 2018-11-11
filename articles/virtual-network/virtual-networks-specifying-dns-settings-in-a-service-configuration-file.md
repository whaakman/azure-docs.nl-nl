---
title: DNS-instellingen op te geven in een serviceconfiguratiebestand | Microsoft Docs
description: aangepaste DNS-instellingen met behulp van het configuratiebestand voor het virtuele netwerk op te geven
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
ms.assetid: 467a4b99-8691-40b3-b640-e25e49675c71
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2016
ms.author: genli
ms.openlocfilehash: 0ac488a67d8b9debf6539d199395997cf44cf1e4
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51247174"
---
# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>DNS-instellingen in een serviceconfiguratiebestand opgeven
## <a name="dns-elements"></a>DNS-elementen
Een serviceconfiguratiebestand mag een DnsServers-element met een lijst van IPv4-adressen voor de Domain Name System (DNS)-servers die de service wordt gebruikt. Instellingen in het serviceconfiguratiebestand hebben voorrang op de instellingen in het netwerkconfiguratiebestand. Zie voor meer informatie, [configuratieschema voor Azure-Service (.cscfg-bestand)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration-element**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

> [!WARNING]
> De **naam** kenmerk in de **DnsServer** element alleen als de naam van een verwijzing wordt gebruikt. Deze staat niet voor de hostnaam voor de DNS-server. Elke **DnsServer** kenmerkwaarde moet uniek zijn in de gehele Microsoft Azure-abonnement.
> 
> 

## <a name="see-also"></a>Zie ook
[Het configuratieschema van de Azure-Service (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Azure Virtual Network-configuratieschema](https://go.microsoft.com/fwlink/?LinkId=248093)

[Een virtueel netwerk met behulp van de configuratiebestanden netwerk configureren](https://go.microsoft.com/fwlink/?LinkId=248094)

[Over instellingen voor Virtueelnetwerk in de beheerportal](https://go.microsoft.com/fwlink/?LinkId=248092)

