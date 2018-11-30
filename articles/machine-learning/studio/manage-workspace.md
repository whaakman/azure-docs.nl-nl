---
title: Een Machine Learning-werkruimte - Azure Machine Learning Studio beheren | Microsoft Docs
description: Toegang tot Azure Machine Learning-werkruimten, beheren en implementeren en beheren van ML API-webservices
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: daf3d413-7a77-4beb-9a7a-6b4bdf717719
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.openlocfilehash: c470d10b933b31d4cfe151fb541c2182562a2805
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52314101"
---
# <a name="manage-an-azure-machine-learning-workspace"></a>Een Azure Machine Learning-werkruimte beheren

> [!NOTE]
> Zie voor meer informatie over het beheren van webservices in Machine Learning-webservicesportal [beheren van een webservice met behulp van de Azure Machine Learning-webserviceportal](manage-new-webservice.md).
> 
> 

U kunt Machine Learning-werkruimten in Azure portal beheren.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Voor het beheren van een werkruimte in de Azure-portal:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) met een administrator-account van Azure-abonnement.
2. Voer in het zoekvak boven aan de pagina 'werkruimten voor machine learning' en selecteer vervolgens **Machine Learning-werkruimten**.
3. Klik op de werkruimte die u wilt beheren.

Naast de standaard resource management informatie en de opties die beschikbaar zijn, kunt u het volgende doen:

- Weergave **eigenschappen** : deze pagina wordt de werkruimte en resource-informatie weergegeven en kunt u de groep en de resourcegroep die deze werkruimte is gekoppeld aan.
- **Opslagsleutels** -onderhoudt sleutels met de storage-account van de werkruimte. Als het opslagaccount sleutels wijzigt, en vervolgens klikt u op **sleutels opnieuw synchroniseren** om te synchroniseren van de sleutels met de werkruimte.

Voor het beheren van de webservices die zijn gekoppeld aan deze werkruimte gebruik van Machine Learning-webserviceportal. Zie [beheren van een webservice met behulp van de Azure Machine Learning-webserviceportal](manage-new-webservice.md) voor volledige informatie.

> [!NOTE]
> Als u wilt implementeren of beheren van nieuwe webservices als u een rol Bijdrager of beheerder zijn op het abonnement waaraan de web-service is geïmplementeerd zijn toegewezen. Als u een andere gebruiker voor een machine learning-werkruimte kunt uitnodigen, moet u deze toewijzen aan een rol Bijdrager of beheerder zijn op het abonnement voordat u ze kunnen implementeren of beheren van webservices. 
> 
>Zie voor meer informatie over toegangsmachtigingen instellen [toegang met RBAC en de Azure-portal beheren](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Machine Learning met Azure Resource Manager-sjablonen implementeren](deploy-with-resource-manager-template.md). 