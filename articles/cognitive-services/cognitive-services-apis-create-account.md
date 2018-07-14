---
title: Een account voor Cognitive Services API's maken in Azure portal | Microsoft Docs
description: Het maken van een account met Microsoft Cognitive Services API's in Azure portal.
services: cognitive-services
documentationcenter: ''
author: garyericson
manager: cgronlun
editor: ''
ms.assetid: b6176bb2-3bb6-4ebf-84d1-3598ee6e01c6
ms.service: cognitive-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2018
ms.author: garye
ms.reviewer: gibattag
ms.openlocfilehash: ed5f19b23375ecb83e19274c7405e9a1208a7985
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39036150"
---
# <a name="create-a-cognitive-services-apis-account-in-the-azure-portal"></a>Een account voor Cognitive Services API's maken in Azure portal

Voor het gebruik van Microsoft Cognitive Service API's, moet u eerst een account maken in Azure portal.

1. Meld u aan bij [Azure Portal](http://portal.azure.com).

2. Klik op **+ een resource maken**.

3. Selecteer onder Azure Marketplace, **AI en Cognitive Services** en de lijst met beschikbare API's te detecteren. Klik op **alle** om te zien van de volledige lijst van Cognitive Services API's. Klik op de API van uw keuze om door te gaan.

    ![Cognitive Services API's selecteren](media/cognitive-services-apis-create-account/select-cognitive-services-apis.png)

4. Op de **maken** pagina, geef de volgende informatie:

   - **Accountnaam:** naam van het account. Wordt u aangeraden een beschrijvende naam, bijvoorbeeld *AFaceAPIAccount*.

   - **Abonnement:** Selecteer een van de beschikbare Azure-abonnementen waarmee u minimaal hebt Inzender-rechten.

   - **API-Type:** kiest u de Cognitive Services-API die u wilt gebruiken. Voor meer informatie over de verschillende Cognitive Services API's beschikbaar, gaat u naar de [Cognitive Services](https://azure.microsoft.com/services/cognitive-services/) site.

   - **Prijscategorie:** de kosten van uw Cognitive Services-account is afhankelijk van het werkelijke gebruik en de opties die u kiest. Raadpleeg voor meer informatie over prijzen voor elke API, de [prijspagina's](https://azure.microsoft.com/pricing/details/cognitive-services/).

   - **Resourcegroep:** een resourcegroep is een verzameling resources met dezelfde levenscyclus, machtigingen en beleidsregels. Zie voor meer informatie over resourcegroepen, [Azure-resources beheren via portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

   - **Locatie van de resourcegroep:** dit is alleen vereist als de API die geselecteerd algemene is (niet gebonden aan een locatie). Als de API globaal en is niet gebonden aan een locatie is, maar moet u een locatie voor de resourcegroep waarin de metagegevens die zijn gekoppeld aan de Cognitive Services-API-account zich bevindt. Deze locatie heeft geen invloed op de runtime-beschikbaarheid van uw account. Zie voor meer informatie over de resourcegroep, [Azure-resources beheren via portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

   - **Expliciete bevestiging van de voorwaarden voor Online-Service:** om te kunnen maken van een account, eigenaars van abonnementen of bijdragers (zoals gedefinieerd door [toegangsbeheer](https://docs.microsoft.com/azure/role-based-access-control/overview)) moet expliciet erkent de voorwaarden die van toepassing op Cognitive Services in [voorwaarden voor Online-Service](https://www.microsoft.com/en-us/Licensing/product-licensing/products.aspx). 

     Het abonnement de eigenaar van het maken van Cognitive Services-account voor een specifieke resourcegroep of abonnement via kunt uitschakelen [Azure beleid](../azure-policy/azure-policy-introduction.md) door het artikel te volgen [met behulp van Azure portal voor het toewijzen en beheren resourcebeleid](../azure-policy/assign-policy-definition.md) en een "niet toegestane resourcetypen" beleidsdefinitie toe te wijzen en op te geven **Microsoft.CognitiveServices/accounts** als het doeltype van de resource.

     Als het maken van accounts is uitgeschakeld, zou u de volgende fout weergegeven op het moment van het maken van accounts:

     ![Fout bij het maken van account](media/cognitive-services-apis-create-account/error-message.png)

5. Als u wilt het account naar de Azure portal-dashboard vastmaken, klikt u op **vastmaken aan Dashboard**.

6. Klik op **Maken** om het account te maken.

Nadat de Cognitive Services-account is geïmplementeerd, klikt u op de tegel in het dashboard om de gegevens weer te geven.

U kunt de **eindpunt-URL** in de **overzicht** sectie en sleutels in de **sleutels** sectie om te beginnen met het maken van API-aanroepen in uw toepassingen.

![Accountgegevens weergeven](media/cognitive-services-apis-create-account/display-account.png)

![Accountsleutels weergeven](media/cognitive-services-apis-create-account/account-keys.png)

### <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over alle Microsoft Cognitive Services beschikbaar [Cognitive Services](https://azure.microsoft.com/services/cognitive-services/).

Voor de snelstartgidsen voor het gebruik van een voorbeeld van de Cognitive Services API's:

 - [Snelstartgidsen voor computer Vision-C#](computer-vision/quickstarts/csharp.md)
 - [Text Analytics met Python](text-analytics/quickstarts/python.md)
 - [Face-API met JavaScript](face/quickstarts/javascript.md)
