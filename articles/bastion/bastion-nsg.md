---
title: Werken met virtuele machines en nsg's in Azure bastionhost | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt NSG-toegang met Azure bastionhost opnemen
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: e7210b2b1be072f5326070d768d5fe12c386ee0b
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191601"
---
# <a name="working-with-nsg-access-and-azure-bastion-preview"></a>Werken met NSG-toegang en Azure bastionhost (Preview)

Als u werkt met Azure bastionhost, kunt u netwerkbeveiligingsgroepen (nsg's). Zie voor meer informatie, [beveiligingsgroepen](../virtual-network/security-overview.md). 

> [!IMPORTANT]
> Deze openbare preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Bepaalde functies worden mogelijk niet ondersteund, zijn mogelijk beperkt of zijn mogelijk niet beschikbaar in alle Azure-locaties. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

![Architectuur](./media/bastion-nsg/nsg_architecture.png)

In dit diagram:

* De bastionhost wordt geïmplementeerd in het virtuele netwerk.
* De gebruiker verbinding maakt met de Azure-portal met behulp van een HTML5-browser.
* De gebruiker selecteert de virtuele machine verbinding maken met.
* De RDP/SSH-sessie geopend met één klik, in de browser.
* Er is geen openbaar IP-adres is vereist op de virtuele machine van Azure.

## <a name="nsg"></a>Netwerkbeveiligingsgroepen

* **AzureBastionSubnet:** Azure bastionhost wordt geïmplementeerd in de specifieke AzureBastionSubnet.  
    * **Inkomend verkeer van openbare internet:** De Bastionomgeving Azure maakt u een openbaar IP-adres dat 443 ingeschakeld op het openbare IP-adres voor inkomend verkeer vereist. Poort 3389/22 hoeven niet te worden geopend op de AzureBastionSubnet.
    * **Uitgaand verkeer naar de doel-VM's:** Azure bastionhost zal contact met de doel-VM's via privé IP-adres. De nsg's moeten toegestaan uitgaand verkeer naar de andere VM-subnetten voor target.
* **Doel-VM-Subnet:** Dit is het subnet waarin de virtuele doelmachine die u RDP/SSH-verbinding wilt.
    * **Inkomend verkeer van Azure bastionhost:** Azure bastionhost zal contact met u op de doel-VM via privé IP-adres. RDP/SSH-poorten (poort 3389 en 22, respectievelijk) moet worden geopend op de doel-VM aan clientzijde via privé IP-adres.

## <a name="apply"></a>Nsg's toepassen op AzureBastionSubnet

Als u nsg's aan de **AzureBastionSubnet**, kunnen de volgende twee servicetags voor Azure-besturingselement vlak- en -infrastructuur:

* **GatewayManager (alleen Resource Manager)** : Met deze tag worden de adresvoorvoegsels van de service Gatewaybeheer van Azure aangeduid. Als u GatewayManager voor de waarde opgeeft, wordt verkeer toegestaan of geweigerd voor GatewayManager.

* **AzureCloud (alleen Resource Manager)** : Deze tag geeft de IP-adresruimte voor Azure, met inbegrip van alle datacenter openbare IP-adressen. Als u AzureCloud voor de waarde opgeeft, wordt verkeer toegestaan of geweigerd voor Azure openbare IP-adressen. Als u wilt dat alleen voor toegang tot AzureCloud in een bepaalde regio, kunt u de regio opgeven. Als u toestaan dat alleen toegang tot Azure AzureCloud in de regio VS-Oost wilt, kan u bijvoorbeeld AzureCloud.EastUS opgeven als servicetag.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Azure bastionhost, de [Veelgestelde vragen](bastion-faq.md)