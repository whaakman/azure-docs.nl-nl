---
title: VMware-oplossing van Azure door CloudSimple - service maken
description: Beschrijft hoe u de service CloudSimple maakt in Azure portal
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: fc08ce48b3b5a4b2cd28cb66e6dd4d9eae3f3885
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66676951"
---
# <a name="create-azure-vmware-solution-by-cloudsimple---service"></a>Azure VMware-oplossing door CloudSimple - Service maken

Als u wilt aan de slag met Azure VMware-oplossing door CloudSimple, maken de VMware-oplossing van Azure door CloudSimple-service in Azure portal.

> [!NOTE]
> Voordat u de service CloudSimple maakt, moet u de resourceprovider Microsoft.VMwareCloudSimple registreren op uw Azure-abonnement. Volg de stappen in [inschakelen van de resourceprovider Microsoft.VMwareCloudSimple op uw Azure-abonnement](enable-cloudsimple-service.md).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-the-service"></a>De service maken

1. Selecteer **Alle services**.

2. Zoeken naar **CloudSimple Services**.

    ![Search CloudSimple Service](media/create-cloudsimple-service-search.png)

3. Selecteer **CloudSimple Services**.

4. Klik op **toevoegen** om een nieuwe service te maken.

    ![CloudSimple Service toevoegen](media/create-cloudsimple-service-add.png)

5. Selecteer het abonnement waar u de service CloudSimple maken.

6. Selecteer de resourcegroep voor de service. Als u wilt een nieuwe resourcegroep toevoegen, klikt u op **nieuw**.

7. Voer naam in om de service te identificeren.

8. De CIDR invoert voor de service-gateway. Geef een/28 subnet dat geen overlap heeft met een van uw bestaande subnetten.  On-premises subnetten, Azure subnetten, het gaat hierbij of een geplande CloudSimple subnetten. U kunt de CIDR niet wijzigen nadat de service is gemaakt.

    ![Het maken van de service CloudSimple](media/create-cloudsimple-service.png)

9. Klik op **OK**.

De service is gemaakt en toegevoegd aan de lijst met services.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [een privécloud maken](https://docs.azure.cloudsimple.com/create-private-cloud/)
* Meer informatie over het [een privécloud-omgeving configureren](quickstart-create-private-cloud.md)
