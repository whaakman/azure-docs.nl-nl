---
title: Een Cognitive Services resource maken in de Azure Portal
titlesuffix: Azure Cognitive Services
description: Ga aan de slag met Azure Cognitive Services door een resource te maken en te abonneren in de Azure Portal.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 07/16/2019
ms.author: aahi
ms.openlocfilehash: e9616f3014288e4b2580f474c49c646928db5a08
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68334240"
---
# <a name="create-a-cognitive-services-resource-using-the-azure-portal"></a>Een Cognitive Services resource maken met behulp van de Azure Portal

Gebruik deze Quick Start om aan de slag te gaan met Azure Cognitive Services met behulp van de Azure Portal. Cognitive Services worden vertegenwoordigd door Azure- [resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal) die u in uw Azure-abonnement hebt gemaakt. Nadat u de resource hebt gemaakt, gebruikt u de sleutels en het eind punt dat u hebt gegenereerd voor het verifiëren van uw toepassingen. 

## <a name="prerequisites"></a>Vereisten

* Een geldig Azure-abonnement: [Maak er gratis een](https://azure.microsoft.com/free/) .

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="create-a-new-azure-cognitive-services-resource"></a>Een nieuwe Azure Cognitive Services-resource maken

Voordat u een Cognitive Services resource maakt, moet u een Azure-resource groep hebben om de resource te kunnen bevatten. Wanneer u een nieuwe resource maakt, hebt u de optie om een nieuwe resource groep te maken of een bestaande te gebruiken. In dit artikel wordt beschreven hoe u een nieuwe resource groep maakt.

1. Meld u aan bij de [Azure Portal](https://portal.azure.com)en klik op **+ een resource maken**.

    ![Cognitive Services-API's selecteren](media/cognitive-services-apis-create-account/azurePortalScreenMulti.png)

2. U kunt op de volgende manieren beschik bare Cognitive Services vinden:
    * Gebruik de zoek balk en voer de naam in van de service waarop u zich wilt abonneren.
        * Als u een resource voor meerdere services wilt maken, voert u **Cognitive Services** in de zoek balk in en selecteert u de **Cognitive Services** resource.

        ![Zoeken naar Cognitive Services](media/cognitive-services-apis-create-account/azureCogServSearchMulti.png)

    * Als u alle beschik bare cognitieve Services wilt weer geven, selecteert u **AI + machine learning**onder **Azure Marketplace**. Als u de service waarin u bent geïnteresseerd, niet ziet, klikt u op **alles weer geven** en bladert u naar **Cognitive Services**. Klik op **meer** om de volledige catalogus van Cognitive Services-API's weer te geven.
    
        ![Cognitive Services-API's selecteren](media/cognitive-services-apis-create-account/azureMarketplace.png)

3. Geef op de pagina **maken** de volgende informatie op:

    > [!IMPORTANT]
    > Onthoud uw Azure-locatie, omdat u deze mogelijk nodig hebt bij het aanroepen van de Azure Cognitive Services.

    |    |    |
    |--|--|
    | **Name** | Een beschrijvende naam voor uw cognitieve Services-resource. Bijvoorbeeld *MyCognitiveServicesAccount*. |
    | **Abonnement** | Selecteer een van de beschik bare Azure-abonnementen. |
    | **Location** | De locatie van uw cognitieve service-exemplaar. Verschillende locaties kunnen latentie introduceren, maar hebben geen invloed op de runtime-Beschik baarheid van uw resource. |
    | **Prijscategorie** | De kosten van uw Cognitive Services-account zijn afhankelijk van de opties die u kiest en uw gebruik. Zie de [prijs informatie](https://azure.microsoft.com/pricing/details/cognitive-services/)voor de API voor meer informatie.
    | **Resourcegroep** | De [Azure-resource groep](https://docs.microsoft.com/azure/architecture/cloud-adoption/governance/resource-consistency/azure-resource-access#what-is-an-azure-resource-group) die uw Cognitive Services-resource bevat. U kunt een nieuwe groep maken of toevoegen aan een bestaande groep. |

    ![Scherm voor het maken van resources](media/cognitive-services-apis-create-account/resource_create_screen.png)


## <a name="get-the-keys-for-your-resource"></a>De sleutels voor uw resource ophalen

Nadat u de resource hebt gemaakt, kunt u deze openen vanuit het Azure-dash board als u deze hebt vastgemaakt. Als dat niet het geval is, kunt u het vinden in **resource groepen**. Nadat u uw resource hebt geselecteerd, kunt u de sleutels ophalen door **sleutels** te selecteren onder **resource beheer**.

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Als u een Cognitive Services-abonnement wilt opschonen en verwijderen, kunt u de resource of resource groep verwijderen. Als u de resource groep verwijdert, worden ook alle resources in de groep verwijderd.

Ga als volgt te werk om een resourcegroep te verwijderen in Azure Portal:

1. Vouw het menu aan de linkerkant in Azure Portal uit om het menu met services te openen en kies **Resourcegroepen** om de lijst met resourcegroepen weer te geven.
2. Zoek de resource groep die u wilt verwijderen en klik met de rechter muisknop op de knop meer (...) aan de rechter kant van de vermelding.
3. Selecteer **Resourcegroep verwijderen** en bevestig dit.

## <a name="see-also"></a>Zie ook

* [Aanvragen verifiëren voor Azure Cognitive Services](authentication.md)
* [Wat zijn Azure Cognitive Services?](Welcome.md)
* [Ondersteuning voor natuurlijke taal](language-support.md)
* [Ondersteuning voor docker-container](cognitive-services-container-support.md)
