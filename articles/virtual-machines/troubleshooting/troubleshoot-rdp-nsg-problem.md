---
title: Kan geen verbinding maken naar Azure Virtual Machines, omdat het RDP-poort is niet ingeschakeld in NSG | Microsoft Docs
description: Meer informatie over het oplossen van problemen waarbij RDP is mislukt vanwege de NSG-configuratie in Azure portal | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/20/2018
ms.author: genli
ms.openlocfilehash: 89af30533e10df0968b60039d7ea15886e89bc25
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52266819"
---
#  <a name="cannot-rdp-to-a-vm-because-rdp-port-is-not-enabled-in-nsg"></a>Niet van RDP-verbinding met een virtuele machine omdat RDP-poort niet is ingeschakeld in NSG

In dit artikel laat zien hoe een probleem waarbij u geen verbinding naar Azure Windows Virtual Machines (VM's maken) omdat de poort niet is ingeschakeld in de netwerkbeveiligingsgroep op te lossen.


> [!NOTE] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over het Resource Manager-implementatiemodel, dat wordt u aangeraden voor nieuwe implementaties in plaats van het klassieke implementatiemodel. 

## <a name="symptom"></a>Symptoom

U kunt een Remote Desktop Protocol (RDP)-verbinding met een virtuele machine in Azure vanwege de RDP-poort niet is geopend in de netwerkbeveiligingsgroep niet maken.

## <a name="solution"></a>Oplossing 

Wanneer u een nieuwe virtuele machine maakt, is standaard al het verkeer van Internet geblokkeerd. 

Volg deze stappen zodat de RDP-poort in de netwerkbeveiligingsgroep:
1. Meld u aan bij de [Azure-portal](https://portal.azure.com).
2. In **virtuele Machines**, de virtuele machine die het probleem zich voordoet. 
3. In **instellingen**, selecteer **netwerken**. 
4. In **regels voor binnenkomende poort**, of als de RDP-poort juist is ingesteld. Hier volgt een voorbeeld van de configuratie. 

    **Prioriteit**: 300 </br>
    **Poort**: 3389 </br>
    **Naam**: Port_3389 </br>
    **Poort**: 3389 </br>
    **Protocol**: TCP </br>
    **Bron**: </br>
    **Bestemmingen**: </br>
    **Actie**: toestaan </br>

In geeft u het bron-IP-adres, deze instelling kunt verkeer alleen vanaf een specifiek IP- of bereik van IP-adressen verbinding maken met de virtuele machine. Zorg ervoor dat de computer die u gebruikt voor het initialiseren van de RDP-sessie in het bereik.

Zie voor meer informatie over de netwerkbeveiligingsgroep [netwerkbeveiligingsgroep](../../virtual-network/security-overview.md).

> [!NOTE]
> RDP-poort 3389 wordt blootgesteld aan Internet. Dit wordt alleen aanbevolen voor testen. Voor productieomgevingen wordt u aangeraden een VPN- of particuliere verbinding.

## <a name="next-steps"></a>Volgende stappen

Als de RDP-poort is al ingeschakeld in de netwerkbeveiligingsgroep, Zie [problemen met een algemene fout RDP in Azure VM](./troubleshoot-rdp-general-error.md).



