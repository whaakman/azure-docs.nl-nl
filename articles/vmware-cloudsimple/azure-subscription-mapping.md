---
title: Azure-abonnement worden toegewezen aan resourcegroepen op Azure VMware-oplossing door CloudSimple
description: Hierin wordt beschreven hoe u een resourcegroep maken op Azure VMware-oplossing door CloudSimple toewijzen aan uw Azure-abonnement
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/05/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: efda996e03d46a2f97d19558f7c2930b623a639e
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67333249"
---
# <a name="map-resource-pools-from-your-private-cloud-to-your-azure-subscription"></a>Resourcegroepen in uw Privécloud aan uw Azure-abonnement toewijzen

Toewijzing van de Azure-abonnement kunt u resourcegroepen in uw vCenter Private Cloud aan uw Azure-abonnement toewijzen. Alleen voor het abonnement waarin u de service CloudSimple hebt gemaakt, kunt u toewijzen.  Het maken van een virtuele VMware-machine vanuit Azure portal, wordt de virtuele machine in de toegewezen resourcegroep geïmplementeerd.  In de portal voor CloudSimple, kunt u weergeven en beheren van de Azure-abonnement voor uw Privéclouds.

Een abonnement kan worden toegewezen aan meerdere resourcegroepen voor vCenter van een privécloud.  U hebt om toe te wijzen resourcegroepen van elke privécloud.  Alleen de toegewezen resourcegroepen zijn beschikbaar voor het maken van een virtuele VMware-machine vanuit Azure portal.

> [!IMPORTANT]
> Toewijzing van een resourcegroep bronnengroepen onderliggende ook worden toegewezen. Een bovenliggende resource pool kan niet worden toegewezen als onderliggend bronnengroepen zijn al toegewezen.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u hebt een CloudSimple service en een privécloud in uw abonnement.  Zie voor het maken van een service CloudSimple [Quick Start - service maken](quickstart-create-cloudsimple-service.md).  Als u nodig hebt om een privécloud te maken, Zie [-Snelstart: een privécloud-omgeving configureren](quickstart-create-private-cloud.md).

U kunt de vCenter-cluster (root-resourcegroep) toewijzen aan uw abonnement.  Als u wilt een nieuwe resourcegroep maken, Zie [een bronnengroep maken](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.resmgmt.doc/GUID-0F6C6709-A5DA-4D38-BE08-6CB1002DD13D.html) artikel op de website van de VMware-documentatie.

## <a name="default-resource-group"></a>Standaard-resourcegroep

Het maken van een nieuwe CloudSimple virtuele machine van Azure-portal, kunt u de resourcegroep selecteren.  Een virtuele machine gemaakt op vCenter-privécloud in een toegewezen resourcegroep zijn zichtbaar in Azure portal.  De gedetecteerde virtuele machine wordt geplaatst in de standaard Azure-resourcegroep.  U kunt de naam van de standaard-resourcegroep wijzigen.

## <a name="map-azure-subscription"></a>Azure-abonnement toewijzen

1. Toegang tot de [CloudSimple portal](access-cloudsimple-portal.md).

2. Open de **Resources** pagina en selecteer de privécloud die u wilt toewijzen.

3. Selecteer **Azure-abonnementen toewijzen**.

4. Klik op **bewerken Azure-abonnement toewijzing**.

5. Beschikbare resourcegroepen toewijzen, selecteert u deze aan de linkerkant en klik op de pijl naar rechts.

6. Als toewijzingen wilt verwijderen, selecteert u deze aan de rechterkant en klikt u op de pijl naar links.

    ![Azure-abonnementen](media/resources-azure-mapping.png)

7. Klik op **OK**.

## <a name="change-default-resource-group-name"></a>Naam van resourcegroep wijzigen standaard

1. Toegang tot de [CloudSimple portal](access-cloudsimple-portal.md).

2. Open de **Resources** pagina en selecteer de privécloud die u wilt toewijzen.

3. Selecteer **Azure-abonnementen toewijzen**.

4. Klik op **bewerken** onder de naam van de Azure-resourcegroep.

    ![De naam van resourcegroep bewerken](media/resources-edit-resource-group-name.png)

5. Voer een nieuwe naam voor de resourcegroep en klik op **indienen**.

    ![Nieuwe Resourcegroepnaam invoeren](media/resources-new-resource-group-name.png)

## <a name="next-steps"></a>Volgende stappen

* [Virtuele VMware-machines op Azure gebruiken](quickstart-create-vmware-virtual-machine.md)
* Meer informatie over [CloudSimple virtuele machines](cloudsimple-virtual-machines.md)