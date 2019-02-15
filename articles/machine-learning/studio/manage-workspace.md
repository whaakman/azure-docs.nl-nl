---
title: Een Machine Learning Studio-werkruimte beheren
titleSuffix: Azure Machine Learning Studio
description: Toegang tot Azure Machine Learning Studio-werkruimten, beheren en implementeren en beheren van Machine Learning API-webservices
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 02/27/2017
ms.openlocfilehash: f792c2b23c8d36225b4c05cf7a334721bd08272a
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56269022"
---
# <a name="manage-an-azure-machine-learning-studio-workspace"></a>Een Azure Machine Learning Studio-werkruimte beheren

> [!NOTE]
> Zie voor meer informatie over het beheren van webservices in Machine Learning-webservicesportal [beheren van een webservice met behulp van de Azure Machine Learning-webserviceportal](manage-new-webservice.md).
> 
> 

U kunt Machine Learning Studio-werkruimten in Azure portal beheren.



## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Voor het beheren van een Studio-werkruimte in de Azure-portal:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) met een administrator-account van Azure-abonnement.
2. Voer in het zoekvak boven aan de pagina 'machine learning Studio-werkruimten' en selecteer vervolgens **werkruimten voor Machine Learning Studio**.
3. Klik op de werkruimte die u wilt beheren.

Naast de standaard resource management informatie en de opties die beschikbaar zijn, kunt u het volgende doen:

- Weergave **eigenschappen** : deze pagina wordt de werkruimte en resource-informatie weergegeven en kunt u de groep en de resourcegroep die deze werkruimte is gekoppeld aan.
- **Opslagsleutels** -onderhoudt sleutels met de storage-account van de werkruimte. Als het opslagaccount sleutels wijzigt, en vervolgens klikt u op **sleutels opnieuw synchroniseren** om te synchroniseren van de sleutels met de werkruimte.

Gebruik voor het beheren van de webservices die zijn gekoppeld aan deze Studio-werkruimte, Machine Learning-webserviceportal. Zie [beheren van een webservice met behulp van de Azure Machine Learning-webserviceportal](manage-new-webservice.md) voor volledige informatie.

> [!NOTE]
> Als u wilt implementeren of beheren van nieuwe webservices als u een rol Bijdrager of beheerder zijn op het abonnement waaraan de web-service is geïmplementeerd zijn toegewezen. Als u een andere gebruiker aan een machine learning Studio-werkruimte uitnodigen, moet u deze toewijzen aan een rol Bijdrager of beheerder zijn op het abonnement voordat u ze kunnen implementeren of beheren van webservices. 
> 
>Zie voor meer informatie over toegangsmachtigingen instellen [toegang met RBAC en de Azure-portal beheren](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Machine Learning met Azure Resource Manager-sjablonen implementeren](deploy-with-resource-manager-template.md). 
