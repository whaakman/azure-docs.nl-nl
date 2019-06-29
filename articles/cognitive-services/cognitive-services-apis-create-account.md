---
title: Een Cognitive Services-account maken in Azure portal
titlesuffix: Azure Cognitive Services
description: Het maken van een account voor Azure Cognitive Services API's in Azure portal.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 06/11/2019
ms.author: aahi
ms.openlocfilehash: b857ee0395c447c8699b8f6a812853528812a7bd
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445860"
---
# <a name="create-a-cognitive-services-account-using-the-azure-portal"></a>Een Cognitive Services-account maken met de Azure-portal

In deze snelstartgids leert u hoe u zich aanmelden voor Azure Cognitive Services en maak een account dat een single-service of meerdere service-abonnement heeft. Deze services worden vertegenwoordigd door Azure [resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal), waarmee u verbinding maken met een of meer van de Azure Cognitive Services-API's.

## <a name="prerequisites"></a>Vereisten

* Een geldig Azure-abonnement. [Maak een account](https://azure.microsoft.com/free/) gratis.

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="create-a-new-azure-cognitive-services-resource"></a>Maak een nieuwe Azure Cognitive Services-resource

Voordat u een resource maakt, moet u een Azure-resourcegroep hebben. Alle Cognitive Services-account (en de bijbehorende Azure-resource) moeten behoren tot een Azure-resourcegroep. Wanneer u een account maakt, hebt u de optie voor het maken van een nieuwe resourcegroep of gebruik een bestaande resourcegroep. In dit artikel laat zien hoe een nieuwe resourcegroep maken.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en klikt u op **+ een resource maken**.

    ![Cognitive Services API's selecteren](media/cognitive-services-apis-create-account/azurePortalScreenMulti.png)

2. U kunt beschikbare Cognitive Services met vinden in de volgende manieren:
    * Gebruik de zoekbalk en voer de naam van de service dat u zich wilt abonneren.
        * Voer voor het maken van een resource voor een abonnement op meerdere services, **Cognitive Services** in het zoekvak, en selecteer de **Cognitive Services** resource.

        ![Zoeken naar Cognitive Services](media/cognitive-services-apis-create-account/azureCogServSearchMulti.png)

    * Als u wilt zien van alle beschikbare cognitive services, selecteer **AI + Machine Learning**onder **Azure Marketplace**. Als u de service u geïnteresseerd bent in niet ziet, klikt u op **alle** en schuif naar **Cognitive Services**. Klik op **meer** om de volledige catalogus van Cognitive Services API's weer te geven.
    
        ![Cognitive Services API's selecteren](media/cognitive-services-apis-create-account/azureMarketplace.png)

3. Op de **maken** pagina, geef de volgende informatie:

    > [!IMPORTANT]
    > Vergeet niet uw Azure-locatie, als u dit mogelijk nodig bij het aanroepen van de Azure Cognitive Services.

    |    |    |
    |--|--|
    | **Naam** | Een beschrijvende naam voor uw cognitive services-resource. Wordt u aangeraden een beschrijvende naam, bijvoorbeeld *MyCognitiveServicesAccount*. |
    | **Abonnement** | Selecteer een van uw beschikbare Azure-abonnementen. |
    | **Location** | De locatie van uw cognitive service-exemplaar. Verschillende locaties kunnen introduceren latentie, maar hebben geen invloed op de runtime-beschikbaarheid van uw resource. |
    | **Prijscategorie** | De kosten van uw Cognitive Services-account, is afhankelijk van de opties die u kiest en uw gebruik. Zie voor meer informatie, de API [prijsinformatie](https://azure.microsoft.com/pricing/details/cognitive-services/).
    | **Resourcegroep** | De [Azure-resourcegroep](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group) waarin uw Cognitive Services-resource. U kunt een nieuwe groep maken of toe te voegen aan een bestaande groep. |

    ![Maken van het scherm](media/cognitive-services-apis-create-account/resource_create_screen.png)


## <a name="get-the-keys-for-your-subscription"></a>De sleutels voor uw abonnement ophalen

Nadat de resource is gemaakt, kunt u openen vanuit het Azure-dashboard als u deze hebt vastgemaakt. Anders kunt u deze vinden in **resourcegroepen**. Na het selecteren van de bron, kunt u de sleutels krijgen door het selecteren van **sleutels** onder **resourcebeheer**.

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt wissen en verwijderen van een Cognitive Services-abonnement, kunt u de resource of resourcegroep verwijderen. Als u de resourcegroep verwijdert, verwijdert u ook alle andere resources die zijn gekoppeld aan de resourcegroep.

Ga als volgt te werk om een resourcegroep te verwijderen in Azure Portal:

1. Vouw het menu aan de linkerkant in Azure Portal uit om het menu met services te openen en kies **Resourcegroepen** om de lijst met resourcegroepen weer te geven.
2. Zoek de resourcegroep te verwijderen, en met de rechtermuisknop op de knop meer (...) aan de rechterkant van de aanbieding.
3. Selecteer **Resourcegroep verwijderen** en bevestig dit.

## <a name="see-also"></a>Zie ook

* [Verifiëren van aanvragen voor Azure Cognitive Services](authentication.md)
* [Wat is Azure Cognitive Services?](Welcome.md)
* [Ondersteuning van natuurlijke taal](language-support.md)
* [Ondersteuning voor docker-containers](cognitive-services-container-support.md)
