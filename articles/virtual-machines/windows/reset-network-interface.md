---
title: Het opnieuw instellen van de netwerkinterface voor virtuele machine van Windows Azure | Microsoft Docs
description: Laat zien hoe netwerkinterface voor virtuele machine van Windows Azure opnieuw instellen
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: genlin
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 6bf5c991e8a96cfdcbad971e0f2ea2dfd01f2893
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="how-to-reset-network-interface-for-azure-windows-vm"></a>Het opnieuw instellen van de netwerkinterface voor virtuele machine van Windows Azure 

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

U kunt naar Microsoft Azure Windows virtuele Machine (VM) kan geen verbinding na het uitschakelen van de standaard Network Interface (NIC) of handmatig Hiermee stelt u een statisch IP-adres voor de NIC. In dit artikel laat zien hoe de netwerkinterface instellen voor de virtuele Windows Azure, die het externe verbindingsprobleem wordt opgelost.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]
## <a name="reset-network-interface"></a>Netwerkinterface opnieuw instellen

### <a name="for-classic-vms"></a>Voor klassieke virtuele machines

Om in te stellen de netwerkinterface, de volgende stappen uit:

1.  Ga naar de [Azure Portal]( https://ms.portal.azure.com).
2.  Selecteer **virtuele Machines (klassiek)**.
3.  Selecteer de betreffende virtuele Machine.
4.  Selecteer **IP-adressen**.
5.  Als de **particuliere IP-toewijzing** is niet **statische**, wijzig dit in **statische**.
6.  Wijzig de **IP-adres** naar een ander IP-adres dat beschikbaar is in het Subnet.
7.  Schakel opslaan.
8.  De virtuele machine wordt opnieuw opgestart om de initialisatie van de nieuwe NIC van het systeem.
9.  Probeer te RDP naar uw computer. Als dit lukt, kunt u het privé IP-adres terug naar de oorspronkelijke indien gewenst. Anders kunt u deze. 

### <a name="for-vms-deployed-in-resource-group-model"></a>Virtuele machines die worden geïmplementeerd in de groep resourcemodel

1.  Ga naar de [Azure Portal]( https://ms.portal.azure.com).
2.  Selecteer de betreffende virtuele Machine.
3.  Selecteer **netwerkinterfaces**.
4.  Selecteer de netwerkinterface die is gekoppeld aan uw computer
5.  Selecteer **IP-configuraties**.
6.  Selecteer het IP-adres. 
7.  Als de **particuliere IP-toewijzing** is niet **statische**, wijzig dit in **statische**.
8.  Wijzig de **IP-adres** naar een ander IP-adres dat beschikbaar is in het Subnet.
9. De virtuele machine wordt opnieuw opgestart om de initialisatie van de nieuwe NIC van het systeem.
10. Probeer te RDP naar uw computer. Als dit lukt, kunt u het privé IP-adres terug naar de oorspronkelijke indien gewenst. Anders kunt u deze. 

## <a name="delete-the-unavailable-nics"></a>Verwijderen van de niet beschikbaar NIC 's
Nadat u extern bureaublad met de machine kunt, moet u de oude NIC's om te voorkomen dat het eventuele probleem verwijderen:

1.  Open Apparaatbeheer.
2.  Selecteer **weergave** > **verborgen apparaten**.
3.  Selecteer **netwerkadapters**. 
4.  Controleer of de adapters 'Microsoft Hyper-V-netwerkadapter' genoemd.
5.  U ziet mogelijk een niet beschikbaar-adapter die is niet beschikbaar. Met de rechtermuisknop op de adapter en selecteer vervolgens verwijderen.

    ![de installatiekopie van de NIC](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > Verwijder alleen de niet beschikbaar adapters die de naam 'Microsoft Hyper-V-netwerkadapter' hebben. Als u een van de andere verborgen adapters verwijdert, kan dit ertoe leiden dat andere problemen.
    >
    >

6.  Alle niet beschikbaar adapter moet nu uit van uw systeem worden gewist.
