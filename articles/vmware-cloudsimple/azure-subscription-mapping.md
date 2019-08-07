---
title: Azure-abonnement toewijzen aan resource groepen in azure VMware-oplossing op CloudSimple
description: Hierin wordt beschreven hoe u een resource groep in een Azure VMware-oplossing toewijst door CloudSimple aan uw Azure-abonnement
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/05/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 1bf721f35500d2ff1344996e7750c5e574f40f31
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816264"
---
# <a name="map-resource-pools-from-your-private-cloud-to-your-azure-subscription"></a>Resource groepen toewijzen vanuit uw Privécloud aan uw Azure-abonnement

Met de toewijzing van Azure-abonnementen kunt u resource groepen vanuit uw Privécloud-vCenter toewijzen aan uw Azure-abonnement. U kunt alleen het abonnement toewijzen waar u de CloudSimple-service hebt gemaakt.  Als u een virtuele VMware-machine maakt op basis van de Azure Portal implementeert de virtuele machine in de toegewezen resource groep.  In de CloudSimple-Portal kunt u het Azure-abonnement voor uw privé-Clouds weer geven en beheren.

Een abonnement kan worden toegewezen aan meerdere vCenter-resource groepen van een privécloud.  U moet resource groepen van elke privécloud toewijzen.  Alleen de toegewezen resource groepen zijn beschikbaar voor het maken van een virtuele VMware-machine vanuit de Azure Portal.

> [!IMPORTANT]
> Als u een resource groep toewijst, worden ook alle onderliggende resource groepen toegewezen. Een bovenliggende resource groep kan niet worden toegewezen als er al onderliggende resource groepen zijn toegewezen.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u een CloudSimple-service en een privécloud hebt in uw abonnement.  Als u een CloudSimple-service wilt maken, raadpleegt u [Quick start-service maken](quickstart-create-cloudsimple-service.md).  Als u een privécloud wilt maken, raadpleegt u [Quick Start-een Privécloud configureren](quickstart-create-private-cloud.md).

U kunt het vCenter-cluster (de hoofd resource groep) toewijzen aan uw abonnement.  Als u een nieuwe resource groep wilt maken, raadpleegt u een artikel van [een resource groep maken op de](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.resmgmt.doc/GUID-0F6C6709-A5DA-4D38-BE08-6CB1002DD13D.html) documentatie site van VMware.

## <a name="default-resource-group"></a>Standaard resource groep

Als u een nieuwe virtuele CloudSimple-machine maakt op basis van Azure Portal, kunt u de resource groep selecteren.  Een virtuele machine die is gemaakt in een privécloud in de cloud in een toegewezen resource groep, wordt weer gegeven op Azure Portal.  De gedetecteerde virtuele machine wordt in de standaard Azure-resource groep geplaatst.  U kunt de naam van de standaard resource groep wijzigen.

## <a name="map-azure-subscription"></a>Azure-abonnement toewijzen

1. Toegang tot de [CloudSimple-Portal](access-cloudsimple-portal.md).

2. Open de pagina **resources** en selecteer de privécloud die u wilt toewijzen.

3. Selecteer **Azure-abonnementen toewijzen**.

4. Klik op **Azure-abonnements toewijzing bewerken**.

5. Als u beschik bare resource groepen wilt toewijzen, selecteert u deze aan de linkerkant en klikt u op de pijl naar rechts.

6. Als u toewijzingen wilt verwijderen, selecteert u deze aan de rechter kant en klikt u op de pijl naar links.

    ![Azure-abonnementen](media/resources-azure-mapping.png)

7. Klik op **OK**.

## <a name="change-default-resource-group-name"></a>Naam van standaard resource groep wijzigen

1. Toegang tot de [CloudSimple-Portal](access-cloudsimple-portal.md).

2. Open de pagina **resources** en selecteer de privécloud die u wilt toewijzen.

3. Selecteer **Azure-abonnementen toewijzen**.

4. Klik op **bewerken** onder naam van Azure-resource groep.

    ![Naam van resource groep bewerken](media/resources-edit-resource-group-name.png)

5. Voer een nieuwe naam in voor de resource groep en klik op **verzenden**.

    ![Nieuwe naam voor de resource groep invoeren](media/resources-new-resource-group-name.png)

## <a name="next-steps"></a>Volgende stappen

* [VMware-Vm's in azure gebruiken](quickstart-create-vmware-virtual-machine.md)
* Meer informatie over [virtuele CloudSimple-machines](cloudsimple-virtual-machines.md)