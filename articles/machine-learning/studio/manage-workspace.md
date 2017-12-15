---
title: Beheren van een Machine Learning-werkruimte | Microsoft Docs
description: Toegang tot Azure Machine Learning-werkruimten, beheren en implementeren en beheren van ML API-webservices
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: daf3d413-7a77-4beb-9a7a-6b4bdf717719
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye
ms.openlocfilehash: 2e4b2869b6eac9670853832d58bc37f1cb0ed001
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="manage-an-azure-machine-learning-workspace"></a>Een Azure Machine Learning-werkruimte beheren

> [!NOTE]
> Zie voor meer informatie over het beheren van Web-services in de portal voor Machine Learning-webservices [beheren van een webservice via de portal voor Azure Machine Learning-webservices](manage-new-webservice.md).
> 
> 

U kunt de werkruimten Machine Learning in Azure portal beheren.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Voor het beheren van een werkruimte in de Azure-portal:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) met een administrator-account Azure-abonnement.
2. Voer in het zoekvak boven aan de pagina 'machine learning werkruimten' en selecteer vervolgens **Machine Learning-werkruimten**.
3. Klik op de werkruimte die u wilt beheren.

Naast de standaard resourcegegevens voor beheer en de opties die beschikbaar zijn, kunt u het volgende doen:

- Weergave **eigenschappen** : deze pagina bevat de informatie van de werkruimte en resource en kunt u het abonnement en de resource-groep die deze werkruimte is verbonden met wijzigen.
- **Opslagsleutels Resync** -de werkruimte onderhoudt sleutels naar het opslagaccount. Als het opslagaccount sleutels wijzigt vervolgens kunt u op **sleutels synchroniseren** de sleutels synchroniseren met de werkruimte.

Voor het beheren van de webservices die zijn gekoppeld aan deze werkruimte kunt u de Machine Learning-webservices-portal gebruiken. Zie [beheren van een webservice via de portal voor Azure Machine Learning-webservices](manage-new-webservice.md) voor volledige informatie.

> [!NOTE]
> Als u wilt implementeren of beheren van nieuwe webservices als u een rol Inzender of -beheerder voor het abonnement waarop de webservice is geïmplementeerd zijn toegewezen. Als u een andere gebruiker voor een machine learning-werkruimte uitnodigen, moet u ze toewijzen aan een rol Inzender of -beheerder voor het abonnement voordat ze kunnen implementeren of beheren van webservices. 
> 
>Zie voor meer informatie over toegangsmachtigingen instellen [toegangstoewijzingen weergeven voor gebruikers en groepen in de Azure portal](../../active-directory/role-based-access-control-manage-assignments.md).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [implementeren Machine Learning met Azure Resource Manager-sjablonen](deploy-with-resource-manager-template.md). 