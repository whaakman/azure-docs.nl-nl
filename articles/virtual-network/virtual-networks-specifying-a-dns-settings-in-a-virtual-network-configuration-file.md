---
title: DNS-instellingen op te geven in een virtueel netwerk-configuratiebestand | Microsoft Docs
description: Het wijzigen van instellingen van de DNS-server in een virtueel netwerk met behulp van een virtueel netwerk-configuratiebestand in het klassieke implementatiemodel
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
tags: azure-service-management
ms.assetid: a8905927-92ac-42b5-8c33-8e42c000692c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: genli
ms.openlocfilehash: 36f7ed9b02b66718327c1a05a6cf29eedf39e7a5
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51248382"
---
# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>DNS-instellingen op te geven in een virtueel netwerk-configuratiebestand
Een netwerkconfiguratiebestand bestaat uit twee elementen die u gebruiken kunt om op te geven van de Domain Name System (DNS)-instellingen: **DnsServers** en **DnsServerRef**. U kunt een lijst met DNS-servers toevoegen door op te geven hun IP-adressen en namen om te verwijzen naar de **DnsServers** element. Vervolgens kunt u een **DnsServerRef** element om op te geven welke DNS-servergegevens uit het element DnsServers worden gebruikt voor verschillende netwerksites binnen uw virtuele netwerk.

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het klassieke implementatiemodel.

Het netwerkconfiguratiebestand mag de volgende elementen bevatten. De titel van elk element is gekoppeld aan een pagina met aanvullende informatie over de instellingen van de waarde element.

> [!IMPORTANT]
> Zie voor meer informatie over het configureren van het netwerkconfiguratiebestand [configureren van een virtueel netwerk met behulp van een netwerkconfiguratiebestand](virtual-networks-using-network-configuration-file.md). Zie voor meer informatie over elk element dat voorkomt in het netwerkconfiguratiebestand [Azure Virtual Network-configuratieschema](https://msdn.microsoft.com/library/azure/jj157100.aspx).
> 
> 

[DNS-Element](https://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

> [!WARNING]
> De **naam** kenmerk in de **DnsServer** element wordt gebruikt als referentie voor alleen de **DnsServerRef** element. Deze staat niet voor de hostnaam voor de DNS-server. Elke **DnsServer** kenmerkwaarde moet uniek zijn in de gehele Microsoft Azure-abonnement
> 
> 

[Kan het Element Sites virtueel netwerk](https://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

> [!NOTE]
> Als u wilt deze instelling voor de virtuele netwerksites-element opgeven, moet deze worden eerder gedefinieerd in het DNS-element. De DnsServerRef *naam* element moet in de virtuele netwerksites verwijzen naar een naamwaarde in het element DNS-voor DNS-server *naam*.
> 
> 

## <a name="next-steps"></a>Volgende stappen
* Inzicht in de [Azure Virtual Network-configuratieschema](https://go.microsoft.com/fwlink/?LinkId=248093).
* Inzicht in de [configuratieschema voor Azure-Service](https://msdn.microsoft.com/library/windowsazure/ee758710).
* [Configureren van een virtueel netwerk met behulp van netwerk-configuratiebestanden](virtual-networks-using-network-configuration-file.md).

