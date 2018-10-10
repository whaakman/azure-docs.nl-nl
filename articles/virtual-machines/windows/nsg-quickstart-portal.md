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
ms.date: 09/27/2018
ms.author: cynthn
ms.openlocfilehash: cac17403425f53593d4f48692b4216a92c8624e3
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48884382"
---
# <a name="how-to-open-ports-to-a-virtual-machine-with-the-azure-portal"></a>Het openen van poorten op een virtuele machine met de Azure-portal
[!INCLUDE [virtual-machines-common-nsg-quickstart](../../../includes/virtual-machines-common-nsg-quickstart.md)]


## <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken

1. Zoek en selecteer de resourcegroep voor de virtuele machine, kiest u **toevoegen**, zoek en selecteer **netwerkbeveiligingsgroep**.

2. Selecteer **Maken**.

    De **netwerkbeveiligingsgroep maken** venster wordt geopend.

    ![Een netwerkbeveiligingsgroep maken](./media/nsg-quickstart-portal/create-nsg.png)

2. Voer een naam voor de netwerkbeveiligingsgroep. 

3. Selecteer of maak een resourcegroep en selecteer vervolgens een locatie.

4. Selecteer **maken** te maken van de netwerkbeveiligingsgroep.

## <a name="create-an-inbound-security-rule"></a>Maak een beveiligingsregel voor binnenkomend verkeer

1. Selecteer de nieuwe netwerkbeveiligingsgroep. 

2. Selecteer **inkomende beveiligingsregels**en selecteer vervolgens **toevoegen**.

    ![Inkomende regel toevoegen](./media/nsg-quickstart-portal/add-inbound-rule.png)

3. Selecteer **Geavanceerd**. 

4. Kies een algemene **Service** uit de vervolgkeuzelijst, zoals **HTTP**. U kunt ook selecteren **aangepaste** als u wilt dat voor een specifieke poort te gebruiken. 

5. Wijzig eventueel de **prioriteit** of **naam**. De prioriteit is van invloed op de volgorde waarin regels worden toegepast: des te lager de numerieke waarde, eerder de regel wordt toegepast.

6. Selecteer **toevoegen** om de regel te maken.

## <a name="associate-your-network-security-group-with-a-subnet"></a>Uw netwerkbeveiligingsgroep aan een subnet koppelen

De laatste stap bestaat uit het koppelen van uw netwerkbeveiligingsgroep aan een subnet of een specifieke netwerkinterface. In dit voorbeeld hebt we de netwerkbeveiligingsgroep koppelen aan een subnet. 

1. Selecteer **subnetten**en selecteer vervolgens **koppelen**.

    ![Een netwerkbeveiligingsgroep koppelen aan een subnet](./media/nsg-quickstart-portal/associate-subnet.png)

2. Selecteer het virtuele netwerk en selecteer vervolgens het juiste subnet.

    ![Een netwerkbeveiligingsgroep koppelen aan virtuele netwerken](./media/nsg-quickstart-portal/select-vnet-subnet.png)

    Alle virtuele machines die u verbinding met dat subnet maken zijn nu bereikbaar is op poort 80.

## <a name="additional-information"></a>Aanvullende informatie

U kunt ook [de stappen in dit artikel uitvoert met behulp van Azure PowerShell](nsg-quickstart-powershell.md).

De opdrachten die worden beschreven in dit artikel kunnen u snel aan de slag verkeersstroom naar uw virtuele machine. Netwerkbeveiligingsgroepen bevatten veel geweldige functies en granulariteit voor het beheren van toegang tot uw resources. Zie voor meer informatie, [netwerkverkeer filteren met een netwerkbeveiligingsgroep](../../virtual-network/tutorial-filter-network-traffic.md).

Overweeg de plaatsing van uw virtuele machines achter een load balancer van Azure voor maximaal beschikbare webtoepassingen. De load balancer verdeelt het verkeer naar virtuele machines, met een netwerkbeveiligingsgroep waarmee verkeer filteren. Zie voor meer informatie, [taakverdeling instellen voor virtuele Windows-machines in Azure om te maken van een maximaal beschikbare toepassing](tutorial-load-balancer.md).

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u een netwerkbeveiligingsgroep gemaakt van een binnenkomende regel die HTTP-verkeer op poort 80 toestaat, en klik vervolgens in een subnet gekoppeld die regel gemaakt. 

U vindt meer informatie over het maken van meer gedetailleerde omgevingen in de volgende artikelen:
- [Overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
- [Beveiligingsgroepen](../../virtual-network/security-overview.md)