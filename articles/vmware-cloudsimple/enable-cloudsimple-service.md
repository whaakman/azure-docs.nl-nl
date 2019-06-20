---
title: VMware-oplossing van Azure door CloudSimple-service inschakelen
description: Beschrijft hoe u ervoor zorgen dat de CloudSimple op een Azure-abonnement en registreer de resourceprovider CLoudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 2553aa95d5028c510b4e1a1b7f51a9f410bcea51
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67154859"
---
# <a name="register-the-microsoftvmwarecloudsimple-resource-provider-on-your-azure-subscription"></a>Registreer de resourceprovider Microsoft.VMwareCloudSimple op uw Azure-abonnement

De service CloudSimple kunt u Azure VMware-oplossing door CloudSimple gebruiken. U kunt de Microsoft.VMwareCloudSimple service registreren als resourceprovider.

## <a name="register-the-resource-provider"></a>De resourceprovider registreren

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer **Alle services**.

3. Zoek en selecteer **abonnementen**.

    ![Abonnementen selecteren](media/cloudsimple-service-select-subscriptions.png)

4. Selecteer het abonnement waarvoor u wilt ervoor zorgen dat CloudSimple.

5. Klik op **resourceproviders** voor het abonnement.

6. Gebruik **Microsoft.VMwareCloudSimple** voor het filteren van de resourceprovider.

7. Selecteer de resourceprovider en klik op **registreren**.

    ![Resourceprovider registreren](media/cloudsimple-service-enable-resource-provider.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [een CloudSimple-service maken](create-cloudsimple-service.md)
* Meer informatie over het [een privécloud-omgeving configureren](quickstart-create-private-cloud.md)
