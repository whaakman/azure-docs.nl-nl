---
title: Een beperkte proefversie project naar Azure verplaatsen
titlesuffix: Azure Cognitive Services
description: Leer hoe u een beperkte proefversie-project naar Azure te verplaatsen.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: anroth
ms.openlocfilehash: 6fac6531ea0a39796de13f95aee33b30dc91f131
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60816513"
---
# <a name="how-to-move-your-limited-trial-project-to-azure"></a>Uw project beperkte proefversie verplaatsen naar Azure

Omdat Custom Vision Service is voltooid de overgang naar Azure, beëindigt de ondersteuning voor projecten met een beperkte proefversie buiten Azure. Dit document wordt beschreven hoe de Custom Vision-API's gebruiken om te kopiëren van uw project beperkte proefversie naar een Azure-resource.

Ondersteuning voor beperkte proefversie projecten weergeven op de [Custom Vision website](https://customvision.ai) is gestopt op 25 maart 2019. Dit document ziet u nu het gebruik van de aangepaste Vision-API's met een [migratie python-script](https://github.com/Azure-Samples/custom-vision-move-project) op GitHub) voor het dupliceren van uw project naar een Azure-resource.

Voor meer informatie, zoals de sleutel deadlines in het proces beperkte proefversie afschaffing verwijzen wij u naar de [opmerkingen bij de release](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/release-notes#february-25-2019) of e-mailberichten verzonden voor eigenaars van beperkte proefversie projecten.

De [migratiescript](https://github.com/Azure-Samples/custom-vision-move-project) kunt u een project opnieuw door te downloaden en vervolgens geüpload alle labels, regio's en afbeeldingen in uw huidige iteratie. Dit laat u met een nieuw project in uw nieuwe abonnement dat u vervolgens kunt trainen.

## <a name="prerequisites"></a>Vereisten

- U moet een geldig Azure-abonnement is gekoppeld aan de Microsoft-account of Azure Active Directory (AAD)-account die u gebruiken wilt voor aanmelding bij de [Custom Vision website](https://customvision.ai). 
    - Als u een Azure-account niet hebt [maken van een account](https://azure.microsoft.com/free/) gratis.
    - Voor een inleiding tot de Azure-concepten van abonnementen en resources, raadpleegt u de [ontwikkelaarshandleiding van Azure.](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#manage-your-subscriptions).
-  [Python](https://www.python.org/downloads/)
- [PIP](https://pip.pypa.io/en/stable/installing/)

## <a name="create-custom-vision-resources-in-the-azure-portal"></a>Custom Vision resources maken in Azure portal

Voor het gebruik van Custom Vision Service met Azure, moet u maken van aangepaste Vision trainen en voorspellen resources in de [Azure-portal](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision). 

Meerdere projecten kunnen worden gekoppeld aan één resource zijn. Meer informatie over [prijzen en beperkingen](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/limits-and-quotas) beschikbaar is. Als u wilt doorgaan Custom Vision Service gratis te gebruiken, kunt u de laag F0 in Azure portal. 

> [!NOTE]
> Wanneer u uw aangepaste Vision-project naar een Azure-resource verplaatst, neemt de onderliggende [machtigingen]( https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) van deze Azure-resource. Als andere gebruikers in uw organisatie eigenaren van de Azure-resource uw project is zijn, deze zich toegang tot uw project op de [Custom Vision website](https://customvision.ai). Op deze manier worden uw resources verwijdert, uw projecten.  

## <a name="find-your-limited-trial-project-information"></a>Informatie over uw beperkte proefversie project

Voor het verplaatsen van uw project, moet u de _project ID_ en _training sleutel_ voor het project dat u probeert te migreren. Als u deze informatie niet hebt, gaat u naar [ https://limitedtrial.customvision.ai/projects ](https://limitedtrial.customvision.ai/projects) verkrijgen de ID en sleutel voor elk van uw projecten. 

## <a name="use-the-python-sample-code-to-copy-your-project-to-azure"></a>De voorbeeldcode voor Python gebruiken om te kopiëren van uw project naar Azure

Ga als volgt de [sample code instructies](https://github.com/Azure-Samples/custom-vision-move-project), met behulp van de sleutel van uw beperkte proefversie en ID als het materiaal 'bron' en de sleutel van de nieuwe Azure resource die u hebt gemaakt als het 'doel'-project.

Standaard zijn alle beperkte proefversie projecten in Zuid-centraal VS Azure regio wordt gehost.

## <a name="next-steps"></a>Volgende stappen

Uw project is nu verplaatst naar een Azure-resource. U moet bijwerken van uw sleutels trainen en voorspellen in alle toepassingen die u hebt geschreven.

Om weer te geven van uw project op de [Custom Vision website](https://customvision.ai), meld u aan met hetzelfde account waarmee u zich bij de Azure-portal. Als u uw project niet ziet, Controleer of dat u zich in dezelfde map in de [Custom Vision website](https://customvision.ai) als de map waar de resources bevinden zich in de Azure-portal. In de Azure-portal en CustomVision.ai, kunt u uw directory in het menu van de gebruiker-omlaag in de rechterbovenhoek van het scherm.