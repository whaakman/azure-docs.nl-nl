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
ms.openlocfilehash: cacd5510147ce997efec922f4b4656956a098d88
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66676936"
---
# <a name="register-the-microsoftvmwarecloudsimple-resource-provider-on-your-azure-subscription"></a>Registreer de resourceprovider Microsoft.VMwareCloudSimple op uw Azure-abonnement

De service CloudSimple kunt u Azure VMware-oplossing door CloudSimple gebruiken. Als u wilt de CloudSimple-service gebruikt, moet dit eerst worden ingeschakeld op uw Azure-abonnement. U kunt de service Microsoft.VMwareCloudSimple vervolgens registreren als resourceprovider.

## <a name="enable-the-cloudsimple-service"></a>De service CloudSimple inschakelen

Om in te schakelen de CloudSimple-service op uw Azure-abonnement, opent u een ondersteuningsaanvraag met [Microsoft ondersteuning](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Selecteer de volgende opties wanneer u de aanvraag indient.

* Type probleem: **Technische**
* Abonnement: **Uw abonnements-ID**
* Service type: **VMware-oplossing door CloudSimple**
* Probleemtype: **Toegewezen knooppunten quotum**
* Probleem-subtype: **Verhoog het quotum van toegewezen knooppunten**
* Onderwerp: **CloudSimple-service inschakelen**

U kunt ook contact opnemen met uw Microsoft-accountvertegenwoordiger op [ azurevmwaresales@microsoft.com ](mailto:azurevmwaresales@microsoft.com). Geef uw Azure-abonnement-ID in het e-mailbericht.  

Nadat de service CloudSimple is ingeschakeld voor uw abonnement, kunt u de resourceprovider van het abonnement.

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