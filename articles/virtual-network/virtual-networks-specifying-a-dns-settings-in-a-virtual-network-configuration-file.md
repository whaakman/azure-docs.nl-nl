---
title: DNS-instellingen opgeven in een virtueel netwerk configuratiebestand | Microsoft Docs
description: Het wijzigen van DNS-serverinstellingen in een virtueel netwerk met een virtueel netwerk-configuratiebestand in het klassieke implementatiemodel
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: a8905927-92ac-42b5-8c33-8e42c000692c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: jdial
ms.openlocfilehash: ec33268915a1888509834ce6a5b2bc782a12ce4a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>DNS-instellingen opgeven in een configuratiebestand voor het virtuele netwerk
Een configuratiebestand netwerk bestaat uit twee elementen die u gebruiken kunt om Domain Name System (DNS)-instellingen te geven: **DnsServers** en **DnsServerRef**. U kunt een lijst met DNS-servers toevoegen door te geven van de IP-adressen en namen om te verwijzen naar de **DnsServers** element. U kunt een **DnsServerRef** element dat u wilt opgeven welke server DNS-vermeldingen van het element DnsServers worden gebruikt voor andere netwerksites binnen het virtuele netwerk.

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het klassieke implementatiemodel.

Het configuratiebestand van het netwerk mag de volgende elementen bevatten. De titel van elk element is gekoppeld aan een pagina met aanvullende informatie over de instellingen van de waarde element.

> [!IMPORTANT]
> Zie voor meer informatie over het configureren van het configuratiebestand netwerk [configureren van een virtueel netwerk via een netwerk-configuratiebestand](virtual-networks-using-network-configuration-file.md). Zie voor meer informatie over elk element in het configuratiebestand van het netwerk bevat [Azure Virtual Network-configuratieschema](https://msdn.microsoft.com/library/azure/jj157100.aspx).
> 
> 

[DNS-Element](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

> [!WARNING]
> De **naam** kenmerk in de **DnsServer** element dient slechts als referentie voor de **DnsServerRef** element. Vertegenwoordigt niet de hostnaam voor de DNS-server. Elke **DnsServer** kenmerkwaarde moet uniek zijn in het hele Microsoft Azure-abonnement
> 
> 

[Virtueel netwerk Sites Element](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

> [!NOTE]
> Om deze instelling opgeven voor het element virtuele netwerksites, moet er worden eerder gedefinieerd in het DNS-element. De DnsServerRef *naam* element moet in de virtuele netwerksites verwijzen naar een naamwaarde in het element DNS-voor de DNS-server *naam*.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* Inzicht in de [Azure Virtual Network-configuratieschema](http://go.microsoft.com/fwlink/?LinkId=248093).
* Inzicht in de [Azure Service configuratieschema](https://msdn.microsoft.com/library/windowsazure/ee758710).
* [Configureren van een virtueel netwerk met behulp van de configuratiebestanden netwerk](virtual-networks-using-network-configuration-file.md).

