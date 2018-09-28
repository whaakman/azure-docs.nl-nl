---
title: Het opnieuw instellen van de netwerkinterface voor virtuele Azure Windows-machine | Microsoft Docs
description: Laat zien hoe u de netwerkinterface voor Azure Windows VM opnieuw instellen
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: willchen
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 05/11/2018
ms.author: genli
ms.openlocfilehash: 509de7a31f48091374dba52869f6d839fd107312
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47413714"
---
# <a name="how-to-reset-network-interface-for-azure-windows-vm"></a>Het opnieuw instellen van de netwerkinterface voor Azure Windows VM 

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

U kan geen verbinding maken op Microsoft Azure Windows Virtual Machine (VM) nadat u de standaard Standaardnetwerkinterface (NIC) uitschakelt of handmatig een statisch IP-adres ingesteld voor de NIC. In dit artikel laat zien hoe opnieuw instellen van de netwerkinterface voor Azure Windows VM, die de verbinding met extern-probleem wordt opgelost.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]
## <a name="reset-network-interface"></a>De netwerkinterface opnieuw instellen

### <a name="for-classic-vms"></a>Voor klassieke VM 's

Als u wilt herstellen van de netwerkinterface, de volgende stappen uit:

1.  Ga naar de [Azure Portal]( https://ms.portal.azure.com).
2.  Selecteer **virtuele Machines (klassiek)**.
3.  Selecteer de betreffende virtuele Machine.
4.  Selecteer **IP-adressen**.
5.  Als de **privé-IP-toewijzing** is niet **statische**, wijzig dit in **statische**.
6.  Wijzig de **IP-adres** naar een ander IP-adres dat beschikbaar is in het Subnet.
7.  Selecteer opslaan.
8.  De virtuele machine wordt opnieuw opgestart om te initialiseren van de nieuwe NIC in het systeem.
9.  Probeer via RDP verbinding naar uw computer. Als dit lukt, kunt u het particuliere IP-adres terug naar de oorspronkelijke wijzigen als u wilt. Anders kunt u deze. 

### <a name="for-vms-deployed-in-resource-group-model"></a>Voor virtuele machines die worden geïmplementeerd in de groep resourcemodel

1.  Ga naar de [Azure Portal]( https://ms.portal.azure.com).
2.  Selecteer de betreffende virtuele Machine.
3.  Selecteer **netwerkinterfaces**.
4.  Selecteer de netwerkinterface die is gekoppeld aan uw computer
5.  Selecteer **IP-configuraties**.
6.  Selecteer het IP-adres. 
7.  Als de **privé-IP-toewijzing** is niet **statische**, wijzig dit in **statische**.
8.  Wijzig de **IP-adres** naar een ander IP-adres dat beschikbaar is in het Subnet.
9. De virtuele machine wordt opnieuw opgestart om te initialiseren van de nieuwe NIC in het systeem.
10. Probeer via RDP verbinding naar uw computer. Als dit lukt, kunt u het particuliere IP-adres terug naar de oorspronkelijke wijzigen als u wilt. Anders kunt u deze. 

## <a name="delete-the-unavailable-nics"></a>Het is niet beschikbaar NIC's verwijderen
Nadat u met de machine via Extern bureaublad kunt, moet u de oude NIC's om het potentiële probleem te voorkomen verwijderen:

1.  Open Apparaatbeheer.
2.  Selecteer **weergave** > **verborgen apparaten weergeven**.
3.  Selecteer **netwerkadapters**. 
4.  Controleer of u adapters met de naam als 'Microsoft Hyper-V-netwerkadapter'.
5.  U ziet mogelijk een niet-beschikbare adapter. Deze wordt grijs weergegeven. Met de rechtermuisknop op de adapter en selecteer verwijderen.

    ![de installatiekopie van de NIC](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > Verwijder alleen de niet beschikbaar-adapters die de naam 'Microsoft Hyper-V-netwerkadapter' hebben. Als u een van de andere verborgen adapters verwijdert, kan dit ertoe leiden dat andere problemen.
    >
    >

6.  Van uw systeem moet nu alle niet-beschikbare adapters zijn gewist.
