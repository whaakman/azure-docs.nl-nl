---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 55e46e058bddca717929df61b2bc766b89e0f885
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58122755"
---
![Virtuele machines in een zelfstandige cloudservice](./media/virtual-machines-common-classic-connect-vms/CloudServiceExample.png)

Als u uw virtuele machines in een virtueel netwerk, kunt u bepalen hoeveel cloudservices die u gebruiken wilt voor load balancing en beschikbaarheidssets. Bovendien kunt u de virtuele machines op subnetten ordenen op dezelfde manier als uw on-premises netwerk en het virtuele netwerk verbinden met uw on-premises netwerk. Hier volgt een voorbeeld:

![Virtuele machines in een virtueel netwerk](./media/virtual-machines-common-classic-connect-vms/VirtualNetworkExample.png)

Virtuele netwerken zijn de aanbevolen manier om virtuele machines in Azure verbinding te maken. De aanbevolen procedure is het configureren van elke laag van uw toepassing in een afzonderlijke cloudservice. Mogelijk moet u echter een aantal virtuele machines van verschillende toepassingslagen combineren in dezelfde cloudservice om te blijven binnen de maximale waarde van 200 cloudservices per abonnement. U kunt deze en andere beperkingen, bekijken [Azure-abonnement en Servicelimieten, Quotums en beperkingen](../articles/azure-subscription-service-limits.md).

## <a name="connect-vms-in-a-virtual-network"></a>Verbinding maken met virtuele machines in een virtueel netwerk
Verbinding met het maken van virtuele machines in een virtueel netwerk:

1. Maken van het virtuele netwerk in de [Azure-portal](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md) en geef 'klassieke implementatie'.
2. Maak de set met cloudservices voor uw implementatie in overeenstemming met uw ontwerp voor beschikbaarheidssets en taakverdeling. Klik in de Azure-portal op **een resource maken > Compute > Cloudservice** voor elke service in de cloud.

   Als u de details van de cloudservice invullen, kies dezelfde _resourcegroep_ gebruikt in combinatie met het virtuele netwerk.

3. Klik op om de nieuwe virtuele machine **een resource maken > Compute**, selecteer vervolgens de juiste VM-installatiekopie uit de **aanbevolen apps**.

   In de virtuele machine **basisbeginselen** blade, kies dezelfde _resourcegroep_ gebruikt in combinatie met het virtuele netwerk.

   ![Blade grondbeginselen van de virtuele machine bij het gebruik van een VNet](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_VN.png)

4. Als u de virtuele machine invullen **instellingen**, kiest u de juiste _Cloudservice_ of _virtueel netwerk_ voor de virtuele machine.

   Azure selecteert het andere item op basis van uw selectie.

   ![Blade VM-instellingen bij het gebruik van een VNet](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_VN.png)


## <a name="connect-vms-in-a-standalone-cloud-service"></a>Verbinding maken met virtuele machines in een zelfstandige cloudservice
Verbinding met het maken van virtuele machines in een zelfstandige cloudservice:

1. Maken van de cloudservice in de [Azure-portal](http://portal.azure.com). Klik op **New > Compute > Cloudservice**. Of u de cloudservice voor uw implementatie kunt maken bij het maken van uw eerste virtuele machine.
2. Wanneer u de virtuele machines maakt, kiest u dezelfde resourcegroep gebruikt in combinatie met de cloudservice.

   ![Een virtuele machine toevoegen aan een bestaande cloudservice](./media/virtual-machines-common-classic-connect-vms/CreateVM_Basics_SA.png)

3. Als u de details van de virtuele machine invullen, kies de naam van de cloudservice in de eerste stap hebt gemaakt.

   ![Een cloudservice voor een virtuele machine selecteren](./media/virtual-machines-common-classic-connect-vms/CreateVM_Settings_SA.png)
