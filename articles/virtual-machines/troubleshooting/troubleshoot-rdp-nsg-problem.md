---
title: Kan geen verbinding met virtuele Azure-machines, omdat de RDP-poort niet is ingeschakeld in NSG | Microsoft Docs
description: Meer informatie over het oplossen van problemen waarbij RDP is mislukt vanwege de NSG-configuratie in Azure portal | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: v-jesits
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/20/2018
ms.author: genli
ms.openlocfilehash: c32612c411f275220f549eea79276fa5a7232fd0
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52954609"
---
#  <a name="cannot-connect-remotely-to-a-vm-because-rdp-port-is-not-enabled-in-nsg"></a>Kan niet extern verbinding maken met een virtuele machine omdat RDP-poort niet is ingeschakeld in NSG

In dit artikel wordt uitgelegd hoe u een probleem waarbij u geen verbinding met een Windows Azure-machine (VM maken) omdat de Remote Desktop Protocol (RDP)-poort is niet ingeschakeld in de netwerkbeveiligingsgroep (NSG) op te lossen.


> [!NOTE] 
> Azure heeft twee implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../azure-resource-manager/resource-manager-deployment-model.md). U wordt aangeraden dat u in plaats van het klassieke implementatiemodel voor nieuwe implementaties het Resource Manager-implementatiemodel gebruiken. 

## <a name="symptom"></a>Symptoom

U kan een RDP-verbinding maken met een virtuele machine in Azure, omdat de RDP-poort niet is geopend in de netwerkbeveiligingsgroep.

## <a name="solution"></a>Oplossing 

Wanneer u een nieuwe virtuele machine maakt, is standaard al het verkeer van Internet geblokkeerd. 

Volg deze stappen zodat de RDP-poort in een NSG:
1. Aanmelden bij [de Azure-portal](https://portal.azure.com).
2. In **virtuele Machines**, selecteer de virtuele machine waarop het probleem optreedt. 
3. In **instellingen**, selecteer **netwerken**. 
4. In **regels voor binnenkomende poort**, controleert u of de RDP-poort juist is ingesteld. Hier volgt een voorbeeld van de configuratie: 

    **Prioriteit**: 300 </br>
    **Poort**: 3389 </br>
    **Naam**: Port_3389 </br>
    **Poort**: 3389 </br>
    **Protocol**: TCP </br>
    **Bron**: </br>
    **Bestemmingen**: </br>
    **Actie**: toestaan </br>

Als u het bron-IP-adres opgeeft, kunt deze instelling alleen verkeer vanaf een specifiek IP-adres of een bereik van IP-adressen verbinding maken met de virtuele machine. Zorg ervoor dat de computer die u gebruikt voor de RDP-sessie te starten binnen het bereik is.

Zie voor meer informatie over nsg's [netwerkbeveiligingsgroep](../../virtual-network/security-overview.md).

> [!NOTE]
> RDP-poort 3389 wordt blootgesteld aan Internet. Daarom raden wij aan dat u deze poort alleen aanbevolen voor testen. Voor productieomgevingen, raden wij aan dat u een VPN- of particuliere verbinding.

## <a name="next-steps"></a>Volgende stappen

Als de RDP-poort is al ingeschakeld in NSG, Zie [problemen met een algemene fout RDP in Azure VM](./troubleshoot-rdp-general-error.md).



