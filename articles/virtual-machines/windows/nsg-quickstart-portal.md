---
title: Openen van poorten op een virtuele machine met behulp van de Azure portal | Microsoft Docs
description: Leer hoe u een poort openen / maken van een eindpunt voor uw Windows-VM met behulp van het resource manager-implementatiemodel in Azure Portal
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: f7cf0319-5ee7-435e-8f94-c484bf5ee6f1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: cynthn
ms.openlocfilehash: 2820dcabf042d7463f9776b42f277a0457caf3b6
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929012"
---
# <a name="how-to-open-ports-to-a-virtual-machine-with-the-azure-portal"></a>Het openen van poorten op een virtuele machine met de Azure-portal
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Snelle opdrachten
U kunt ook [deze stappen uitvoeren met Azure PowerShell](nsg-quickstart-powershell.md).

Maak eerst de Netwerkbeveiligingsgroep. Selecteer een resourcegroep in de portal, kiest u **toevoegen**, zoek en selecteer **netwerkbeveiligingsgroep**:

![Een Netwerkbeveiligingsgroep toevoegen](./media/nsg-quickstart-portal/add-nsg.png)

Voer een naam in voor uw Netwerkbeveiligingsgroep, selecteer of maak een resourcegroep en selecteer een locatie. Selecteer **maken** wanneer u klaar bent:

![Een Netwerkbeveiligingsgroep maken](./media/nsg-quickstart-portal/create-nsg.png)

Selecteer de nieuwe Netwerkbeveiligingsgroep. Selecteer 'Inkomende beveiligingsregels' en selecteer vervolgens de **toevoegen** knop om een regel te maken:

![Een inkomende regel toevoegen](./media/nsg-quickstart-portal/add-inbound-rule.png)

Voor het maken van een regel toestaat die verkeer:

- Selecteer de **Basic** knop. Standaard de **Geavanceerd** venster biedt enkele extra configuratieopties, zoals voor het definiëren van een specifieke bron-IP-adresbereik blokkeren of poort.
- Kies een algemene **Service** uit de vervolgkeuzelijst, zoals *HTTP*. U kunt ook selecteren *aangepaste* voor een specifieke poort te gebruiken. 
- Indien gewenst, de prioriteit of de naam wijzigen. De prioriteit bepaalt de volgorde waarin regels worden toegepast: des te lager de numerieke waarde, eerder de regel wordt toegepast.
- Wanneer u klaar bent, selecteert u **OK** om de regel te maken:

![Een inkomende regel maken](./media/nsg-quickstart-portal/create-inbound-rule.png)

De laatste stap bestaat uit het koppelen van uw Netwerkbeveiligingsgroep aan een subnet of een specifieke netwerkinterface. Laten we de Netwerkbeveiligingsgroep koppelen aan een subnet. Selecteer **subnetten**, en kies vervolgens **koppelen**:

![Een Netwerkbeveiligingsgroep koppelen aan een subnet](./media/nsg-quickstart-portal/associate-subnet.png)

Selecteer het virtuele netwerk en selecteer vervolgens het juiste subnet:

![Een Netwerkbeveiligingsgroep koppelen aan virtuele netwerken](./media/nsg-quickstart-portal/select-vnet-subnet.png)

U hebt nu een Netwerkbeveiligingsgroep, een binnenkomende regel gemaakt die verkeer op poort 80 toestaat en deze die zijn gekoppeld aan een subnet gemaakt. Alle virtuele machines die u verbinding met dat subnet maken zijn op poort 80 bereikbaar.

## <a name="more-information-on-network-security-groups"></a>Meer informatie over Netwerkbeveiligingsgroepen
De snelle opdrachten hier kunnen u aan de slag met de verkeersstroom naar uw virtuele machine. Netwerkbeveiligingsgroepen bevatten veel geweldige functies en granulariteit voor het beheren van toegang tot uw resources. U kunt meer lezen over [hier het maken van een Netwerkbeveiligingsgroep en ACL-regels](../../virtual-network/tutorial-filter-network-traffic.md).

Voor maximaal beschikbare webtoepassingen, moet u uw virtuele machines achter een Azure Load Balancer plaatsen. De load balancer verdeelt het verkeer naar virtuele machines met een Netwerkbeveiligingsgroep waarmee verkeer filteren. Zie voor meer informatie, [laden saldo van Linux virtuele machines in Azure om te maken van een maximaal beschikbare toepassing](tutorial-load-balancer.md).

## <a name="next-steps"></a>Volgende stappen
In dit voorbeeld moet u een eenvoudige regel zodat HTTP-verkeer gemaakt. U vindt meer informatie over het maken van meer gedetailleerde omgevingen in de volgende artikelen:

* [Overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
* [Wat is er een netwerkbeveiligingsgroep?](../../virtual-network/security-overview.md)