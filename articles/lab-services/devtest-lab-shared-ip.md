---
title: Informatie over gedeelde IP-adressen in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over hoe Azure DevTest Labs gedeelde IP-adressen gebruikt voor het minimaliseren van het openbare IP-adressen vereist voor toegang tot uw lab-virtuele machines.
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
ms.openlocfilehash: e7080901118dde33ed07c8a80f254b9b0d2e221c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60622995"
---
# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a>Informatie over gedeelde IP-adressen in Azure DevTest Labs

Azure DevTest Labs kunt lab-virtuele machines delen hetzelfde openbare IP-adres voor het minimaliseren van het aantal openbare IP-adressen die zijn vereist voor toegang tot uw afzonderlijke lab-virtuele machines.  Dit artikel wordt beschreven hoe gedeelde IP-adressen werk en hun verwante configuratie-opties.

## <a name="shared-ip-setting"></a>Gedeelde IP-instelling

Wanneer u een lab hebt gemaakt, bevindt deze zich in een subnet van een virtueel netwerk.  Dit subnet wordt standaard gemaakt met **inschakelen gedeeld openbaar IP-adres** ingesteld op *Ja*.  Deze configuratie maakt u een openbaar IP-adres voor het hele subnet.  Zie voor meer informatie over het configureren van virtuele netwerken en subnetten [een virtueel netwerk configureren in Azure DevTest Labs](devtest-lab-configure-vnet.md).

![Nieuw lab-subnet](media/devtest-lab-shared-ip/lab-subnet.png)

Voor bestaande labs kunt u deze optie inschakelen door te selecteren **configuratie en het beleid > virtuele netwerken**. Vervolgens selecteert u een virtueel netwerk in de lijst en kies **inschakelen gedeeld openbaar IP-adres** voor een geselecteerd subnet. U kunt deze optie in een testomgeving ook uitschakelen als u niet wilt delen van een openbaar IP-adres op lab VM's.

Virtuele machines in dit lab maken standaard gebruik van een gedeelde IP-adres hebt gemaakt.  Bij het maken van de virtuele machine, deze instelling kan worden waargenomen de **geavanceerde instellingen** blade onder **IP-adresconfiguratie**.

![Nieuwe virtuele machine](media/devtest-lab-shared-ip/new-vm.png)

- **Gedeeld:** Alle virtuele machines die zijn gemaakt als **gedeelde** in één resourcegroep (RG) worden geplaatst. Een enkel IP-adres is toegewezen voor die RG en alle virtuele machines in de RG dat IP-adres wordt gebruikt.
- **Openbaar:** Elke virtuele machine die u maakt een eigen IP-adres heeft en in een eigen resourcegroep wordt gemaakt.
- **Persoonlijke:** Elke virtuele machine die u maakt een privé IP-adres gebruikt. U pas weer verbinding maken met deze virtuele machine rechtstreeks vanuit het internet met extern bureaublad.

Wanneer een virtuele machine met gedeelde IP-adres ingeschakeld wordt toegevoegd aan het subnet, DevTest Labs automatisch voegt van de virtuele machine aan een load balancer toe en wijst een TCP-poortnummer op het openbare IP-adres worden doorgestuurd naar de RDP-poort op de virtuele machine.  

## <a name="using-the-shared-ip"></a>Met behulp van het gedeelde IP-adres

- **Linux-gebruikers:** SSH naar de virtuele machine met behulp van de IP-adres of FQDN-naam, gevolgd door een dubbele punt, gevolgd door de poort. Bijvoorbeeld, in de onderstaande afbeelding, de RDP-adres voor de verbinding met de virtuele machine is `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.

  ![Voorbeeld van de virtuele machine](media/devtest-lab-shared-ip/vm-info.png)

- **Windows-gebruikers:** Selecteer de **Connect** knop op de Azure portal een vooraf geconfigureerde RDP-bestand downloaden en toegang tot de VM.

## <a name="next-steps"></a>Volgende stappen

* [Het definiëren van beleid voor lab maken in Azure DevTest Labs](devtest-lab-set-lab-policy.md)
* [Een virtueel netwerk configureren in Azure DevTest Labs](devtest-lab-configure-vnet.md)





