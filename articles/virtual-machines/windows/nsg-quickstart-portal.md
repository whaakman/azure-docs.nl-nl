---
title: Openen van poorten voor een virtuele machine met de Azure portal | Microsoft Docs
description: Meer informatie over het openen van een poort of een eindpunt met uw Windows-virtuele machine met behulp van het implementatiemodel van resource manager in de Azure Portal maken
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: f7cf0319-5ee7-435e-8f94-c484bf5ee6f1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: iainfou
ms.openlocfilehash: 33bc0be0aeae6d0276fd8999b9ac0a010e3067ba
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-open-ports-to-a-virtual-machine-with-the-azure-portal"></a>Het openen van poorten op een virtuele machine met de Azure-portal
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Snelle opdrachten
U kunt ook [u deze stappen uitvoert met Azure PowerShell](nsg-quickstart-powershell.md).

Maak eerst de Netwerkbeveiligingsgroep. Selecteer een resourcegroep in de portal, kiest u **toevoegen**, zoekt en selecteert u **netwerkbeveiligingsgroep**:

![Een Netwerkbeveiligingsgroep toevoegen](./media/nsg-quickstart-portal/add-nsg.png)

Voer een naam voor de beveiligingsgroep van uw netwerk, selecteert u of een resourcegroep maken en selecteer een locatie. Selecteer **maken** wanneer voltooid:

![Een Netwerkbeveiligingsgroep maken](./media/nsg-quickstart-portal/create-nsg.png)

Selecteer uw nieuwe Netwerkbeveiligingsgroep. Selecteer 'Inkomende beveiligingsregels' en selecteer vervolgens de **toevoegen** knop om een regel te maken:

![Een inkomende regel toevoegen](./media/nsg-quickstart-portal/add-inbound-rule.png)

Kies een gemeenschappelijke **Service** uit de vervolgkeuzelijst zoals *HTTP*. U kunt ook selecteren *aangepaste* voor een specifieke poort te gebruiken. Indien gewenst, wijzigt u de naam van de prioriteit of. De prioriteit bepaalt de volgorde waarin regels worden toegepast - des te lager de numerieke waarde, eerder de regel wordt toegepast. U kunt ook selecteren **Geavanceerd** boven aan dit scherm naar een specifieke bron IP-blok of poort bereik invoeren, bijvoorbeeld. Wanneer u klaar bent, selecteert u **OK** om de regel te maken:

![Een inkomende regel maken](./media/nsg-quickstart-portal/create-inbound-rule.png)

De laatste stap wordt de Netwerkbeveiligingsgroep koppelen aan een subnet of een specifieke netwerkinterface. Laten we de Netwerkbeveiligingsgroep aan een subnet koppelen. Selecteer **subnetten**, en kies vervolgens **koppelen**:

![Een Netwerkbeveiligingsgroep aan een subnet koppelen](./media/nsg-quickstart-portal/associate-subnet.png)

Selecteer het virtuele netwerk, en selecteer vervolgens het juiste subnet:

![Koppelen van een Netwerkbeveiligingsgroep aan een virtueel netwerk](./media/nsg-quickstart-portal/select-vnet-subnet.png)

U hebt nu een Netwerkbeveiligingsgroep gemaakt van een inkomende regel staat toe dat verkeer op poort 80 en die aan een subnet is gekoppeld gemaakt. Er zijn geen VM's die u verbinding met dat subnet maken bereikbaar op poort 80.

## <a name="more-information-on-network-security-groups"></a>Meer informatie over Netwerkbeveiligingsgroepen
De snelle opdrachten hier kunt u aan de slag te kunnen met verkeer naar uw virtuele machine. Netwerkbeveiligingsgroepen bieden veel handige functies en granulatie voor het beheren van toegang tot uw resources. U kunt meer lezen over [hier maken van een Netwerkbeveiligingsgroep en ACL regels](../../virtual-network/virtual-networks-create-nsg-arm-ps.md).

Voor maximaal beschikbare webtoepassingen, moet u uw virtuele machines achter een Load Balancer van Azure plaatsen. De load balancer wordt verkeer naar virtuele machines met een Netwerkbeveiligingsgroep waarmee wordt verkeer gefilterd. Zie voor meer informatie [het laden van Linux virtuele machines in Azure maken van een maximaal beschikbare toepassing in evenwicht](tutorial-load-balancer.md).

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld moet u een eenvoudige regel zodat HTTP-verkeer gemaakt. Hier vindt u informatie over het maken van meer gedetailleerde omgevingen in de volgende artikelen:

* [Overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [Wat is een netwerkbeveiligingsgroep (NSG)?](../../virtual-network/virtual-networks-nsg.md)