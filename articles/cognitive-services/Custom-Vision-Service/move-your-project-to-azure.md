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
ms.openlocfilehash: a9f49af54f391b159f8b3d626fffc36635f5e51f
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56821299"
---
# <a name="how-to-move-your-limited-trial-project-to-azure-using-the-customvisionai-site"></a>Uw project beperkte proefversie verplaatsen naar Azure met behulp van de site CustomVision.ai


Omdat Custom Vision Service is nu in [Azure Preview](https://azure.microsoft.com/services/preview/), beëindigt de ondersteuning voor projecten met een beperkte proefversie buiten Azure. Dit document leert u hoe u de [Custom Vision website](https://customvision.ai) te verplaatsen van uw project op beperkte proefversie te worden gekoppeld aan een Azure-resource. 

> [!NOTE]
> Wanneer u uw aangepaste Vision-projecten naar een Azure-resource verplaatsen, zij het overnemen van onderliggende [machtigingen]( https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) van deze Azure-resource. Als andere gebruikers in uw organisatie eigenaren van de Azure-resource uw project is zijn, deze zich toegang tot uw project op de [Custom Vision website](https://customvision.ai). Op deze manier worden uw resources verwijdert, uw projecten.  


Voor een inleiding tot de Azure-concepten van abonnementen en resources, raadpleegt u de [ontwikkelaarshandleiding van Azure.](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#manage-your-subscriptions)


## <a name="prerequisites"></a>Vereisten

U moet een geldig Azure-abonnement is gekoppeld aan de hetzelfde Microsoft-account of Azure Active Directory (AAD) waarmee u zich aanmelden bij de [Custom Vision website](https://customvision.ai). 

Als u een Azure-account niet hebt [maken van een account](https://azure.microsoft.com/free/) gratis.


## <a name="create-custom-vision-resources-in-the-azure-portal"></a>Custom Vision resources maken in Azure portal
Voor het gebruik van Custom Vision Service met Azure, moet u maken van aangepaste Vision trainen en voorspellen resources in de [Azure-portal](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision). 

 Verplaatsen van uw project met behulp van dit [Custom Vision website](https://customvision.ai) ervaring, moet u uw resources in de regio Zuid-centraal VS, omdat alle beperkte proefversie projecten worden gehost in Zuid-centraal VS. 

Meerdere projecten kunnen worden gekoppeld aan één resource zijn. Meer informatie over [prijzen en beperkingen](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/limits-and-quotas) beschikbaar is. Als u wilt doorgaan Custom Vision Service gratis te gebruiken, kunt u de laag F0 in Azure portal. 


## <a name="move-your-limited-trial-project-to-an-azure-resource"></a>Uw project beperkte proefversie verplaatsen naar een Azure-resource

1.  Navigeer in uw webbrowser naar de [Custom Vision website](https://customvision.ai) en selecteer __aanmelden__. Open het project dat u wilt migreren naar een Azure-account. 
2.  Open de pagina instellingen voor uw project door te klikken op het tandwielpictogram in de hoek boven rechterzijde van het scherm. 

    ![Instellingen voor het project is het tandwielpictogram in de rechterbovenhoek van de projectpagina.](./media/move-your-project-to-azure/settings-icon.png)


3. Klik op __verplaatsen naar Azure__.

    ![Verplaatsen naar Azure knop is in de linkerbenedenhoek van de pagina instellingen voor het Project.](./media/move-your-project-to-azure/move-to-azure.jpg)


4. In de vervolgkeuzelijst op de __verplaatsen naar Azure__ knop, selecteert u de Azure resource die u wilt verplaatsen van uw project. Klik op __verplaatsen__. 

5. Als u de Azure resource die u eerder hebt gemaakt voor Custom Vision Service niet ziet, kan het zijn in een andere directory. Volg de onderstaande instructies om uw project naar een resource in een andere map verplaatsen. 

    ![Project migratieperiode.](./media/move-your-project-to-azure/Project_Migration_Window.jpg)


## <a name="move-project-to-another-azure-directory"></a>Project verplaatsen naar een andere Azure-map 

> [!NOTE]
> In de Azure-portal en CustomVision.ai, kunt u uw directory in het menu van de gebruiker-omlaag in de rechterbovenhoek van het scherm.   


1. Identificeren welke directory uw Azure-resource wordt. Hier vindt u de map die wordt vermeld onder uw gebruikersnaam in de rechterbovenhoek van de Azure portal in de menubalk. 

    ![Uw directory wordt vermeld onder uw gebruikersnaam in de rechterbovenhoek van de Azure portal in de menubalk. .](./media/move-your-project-to-azure/identify_directory.jpg)

2. De Resource-ID van uw aangepaste Vision Training-resource niet vinden. U kunt dit vinden in Azure portal door uw aangepaste Vision Training-resource openen en selecteert u 'Eigenschappen' onder de sectie ' Resource '. De Resource-ID aanwezig zal zijn. 

    ![Uw Resource-ID vinden in Azure portal door uw aangepaste Vision Training-resource openen en selecteert u 'Eigenschappen' onder de sectie ' Resource '.](./media/move-your-project-to-azure/resource_ID_azure_portal.jpg)


3. U kunt ook u vindt de Resource-ID van uw aangepaste Vision Resource rechtstreeks in de website van Custom Vision [instellingenpagina]( https://www.customvision.ai/projects#/settings). U moet overschakelen naar uw Azure-resource is in dezelfde map.

    ![De Resource-ID wordt weergegeven voor elke resource op de instellingenpagina op de website van Custom Vision.](./media/move-your-project-to-azure/resource_ID_CVS_portal.jpg)

4. Nu dat u de resource-ID hebt, kunt u teruggaan naar het aangepaste Vision-project dat u probeert te verplaatsen van een beperkte proefversie naar een Azure-resource. Herinnering, wellicht u wilt terugkeren naar de oorspronkelijke map te vinden. Volg de instructies [hierboven](#move-your-limited-trial-project-to-an-azure-resource) naar de instellingenpagina van uw project openen en selecteer __verplaatsen naar Azure__. 


5. Schakel het selectievakje voor 'Verplaatsen naar een andere Azure-adreslijst?' in de overstap naar Azure-venster. Selecteer de map die u wilt verplaatsen van uw project en voert u de Resource-ID van de resource die u uw project verplaatst. Klik op __verplaatsen__. 



5. Let op: uw project is nu in een andere map. Als u wilt zoeken in uw project, moet u overschakelen naar dezelfde map op de webportal van Custom Vision dat uw project in. In de Azure portal en de [Custom Vision website](https://customvision.ai), kunt u uw directory in het menu account van de vervolgkeuzelijst in de rechterbovenhoek van het scherm. 

## <a name="next-steps"></a>Volgende stappen

Uw project is nu verplaatst naar een Azure-resource. U moet bijwerken van uw sleutels trainen en voorspellen in alle toepassingen die u hebt geschreven.
