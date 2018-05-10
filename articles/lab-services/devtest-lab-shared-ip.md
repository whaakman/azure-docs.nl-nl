---
title: Inzicht in gedeelde IP-adressen in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over hoe Azure DevTest Labs gedeelde IP-adressen gebruikt om te beperken van het openbare IP-adressen vereist voor toegang tot uw lab virtuele machines.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: c62f8808565022371484b936f5a2bdaba1f8900e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a>Gedeelde IP-adressen in Azure DevTest Labs begrijpen

Azure DevTest Labs kunt lab VMs delen hetzelfde openbare IP-adres om te beperken het aantal openbare IP-adressen die zijn vereist voor toegang tot uw afzonderlijke lab virtuele machines.  Dit artikel wordt beschreven hoe gedeelde IP-adressen werk en hun bijbehorende configuratie-opties.

## <a name="shared-ip-setting"></a>Gedeelde IP-instelling

Wanneer u een testomgeving maakt, bevindt het zich in een subnet van een virtueel netwerk.  Dit subnet is standaard gemaakt met **Schakel gedeelde openbare IP-adres** ingesteld op *Ja*.  Deze configuratie maakt een openbaar IP-adres voor het hele subnet.  Zie voor meer informatie over het configureren van virtuele netwerken en subnetten [configureren van een virtueel netwerk in Azure DevTest Labs](devtest-lab-configure-vnet.md).

![Nieuw lab-subnet](media/devtest-lab-shared-ip/lab-subnet.png)

Voor bestaande labs, kunt u deze optie inschakelen door het selecteren van **configuratie en het beleid > virtuele netwerken**. Vervolgens selecteert u een virtueel netwerk in de lijst en kies **inschakelen gedeelde openbare IP-adres** voor een geselecteerde subnet. U kunt deze optie in een testomgeving ook uitschakelen als u niet wilt delen van een openbaar IP-adres in het lab virtuele machines.

Alle virtuele machines in dit lab standaard voor het gebruik van een gedeelde IP-adres hebt gemaakt.  Bij het maken van de virtuele machine met deze instelling kan worden waargenomen de **geavanceerde instellingen** blade onder **IP-adresconfiguratie**.

![Nieuwe virtuele machine](media/devtest-lab-shared-ip/new-vm.png)

- **Gedeelde:** alle virtuele machines die zijn gemaakt als **gedeelde** in één resourcegroep (RG) worden geplaatst. Een enkel IP-adres is toegewezen voor die RG en alle virtuele machines in de RG dat IP-adres wordt gebruikt.
- **Openbaar:** elke VM die u maakt een eigen IP-adres heeft en in een eigen resourcegroep wordt gemaakt.
- **Persoonlijk:** elke VM die u maakt een particuliere IP-adres gebruikt. Niet mogelijk rechtstreeks vanaf het internet met extern bureaublad verbinding maken met deze virtuele machine.

Wanneer een virtuele machine met gedeelde IP ingeschakeld wordt toegevoegd aan het subnet, DevTest Labs automatisch voegt van de virtuele machine aan een load balancer toe en wijst een TCP-poortnummer op het openbare IP-adres, worden doorgestuurd naar de RDP-poort op de virtuele machine.  

## <a name="using-the-shared-ip"></a>Met behulp van het gedeelde IP-adres

- **Linux-gebruikers:** SSH naar de virtuele machine met behulp van de IP-adres of FQDN-naam, gevolgd door een dubbelepunt, gevolgd door de poort. In de onderstaande afbeelding de RDP-adres voor de verbinding met de virtuele machine is bijvoorbeeld `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.

  ![Voorbeeld van de virtuele machine](media/devtest-lab-shared-ip/vm-info.png)

- **Windows-gebruikers:** selecteert u de **Connect** knop op de Azure-portal voor het downloaden van een vooraf geconfigureerde RDP-bestand en toegang tot de virtuele machine.

## <a name="next-steps"></a>Volgende stappen

* [Labbeleidsregels definiëren in Azure DevTest Labs](devtest-lab-set-lab-policy.md)
* [Een virtueel netwerk configureren in Azure DevTest Labs](devtest-lab-configure-vnet.md)





