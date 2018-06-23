---
title: Een cognitieve API's voor Services-account maken in de Azure portal | Microsoft Docs
description: Het maken van een account Microsoft cognitieve Services API's in de Azure portal.
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
ms.openlocfilehash: 3697dd0628f0028cb486139e92c032f0d757c6ed
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344845"
---
# <a name="create-a-cognitive-services-apis-account-in-the-azure-portal"></a>Een cognitieve API's voor Services-account maken in de Azure portal

Als u wilt Microsoft cognitieve Service API's gebruiken, moet u eerst een account maken in de Azure-portal.

1. Meld u aan bij [Azure Portal](http://portal.azure.com).

2. Klik op **+ maken van een resource**.

3. Selecteer onder Azure Marketplace **AI + cognitieve Services** en de lijst met beschikbare API's te detecteren. Klik op **alle** om te zien van de volledige lijst met cognitieve Services-API's. Klik op de API van uw keuze om door te gaan.

    ![Selecteer cognitieve Services API 's](media/cognitive-services-apis-create-account/select-cognitive-services-apis.png)

4. Op de **maken** pagina, geef de volgende informatie:

   - **Accountnaam:** naam van het account. Het is raadzaam een beschrijvende naam, bijvoorbeeld met *AFaceAPIAccount*.

   - **Abonnement:** Selecteer een van de beschikbare Azure-abonnementen u ten minste hebt Inzender-rechten.

   - **API-Type:** kiezen cognitieve Services-API die u wilt gebruiken. Voor meer informatie over de verschillende cognitieve Services beschikbare API's, gaat u naar de [cognitieve Services](https://azure.microsoft.com/services/cognitive-services/) site.

   - **Prijscategorie:** de kosten van uw cognitieve Services-account is afhankelijk van het werkelijke gebruik en de opties die u kiest. Raadpleeg voor meer informatie over prijzen voor elke API, de [prijzen van pagina's](https://azure.microsoft.com/pricing/details/cognitive-services/).

   - **Resourcegroep:** een resourcegroep is een verzameling resources met dezelfde levenscyclus, machtigingen en beleidsregels. Zie voor meer informatie over resourcegroepen, [beheren Azure-resources via de portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

   - **Locatie van resourcegroep:** dit is alleen vereist als de API die geselecteerd algemene is (niet gebonden aan een locatie). Als de API globale en is niet gebonden aan een locatie is, echter, moet u een locatie voor de resourcegroep waar de metagegevens gekoppeld aan de API voor cognitieve Services-account zich bevindt. Deze locatie heeft geen invloed op de runtime-beschikbaarheid van uw account. Zie voor meer informatie over de resourcegroep, [beheren Azure-resources via de portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

   - **Expliciete bevestiging van Online servicevoorwaarden:** om te kunnen maken van een account, Abonnementseigenaren of inzenders (zoals gedefinieerd door [rollen gebaseerd toegangsbeheer](https://docs.microsoft.com/azure/role-based-access-control/overview)) moet erkent expliciet de voorwaarden die van toepassing op cognitieve Services in [Online servicevoorwaarden](https://www.microsoft.com/en-us/Licensing/product-licensing/products.aspx). 

     Het maken van cognitieve Services-account voor een specifieke resourcegroep of abonnement via kunt uitschakelen door de eigenaar van het abonnement [Azure beleid](../azure-policy/azure-policy-introduction.md) aan de hand van het artikel [toewijzen en beheren met behulp van Azure-portal bronbeleid](../azure-policy/assign-policy-definition.md) en toewijzen van een beleidsdefinitie 'niet-toegestane brontypen' en het opgeven van **Microsoft.CognitiveServices/accounts** als het doeltype van de resource.

     Als het maken van het account is uitgeschakeld, zou u de volgende fout weergegeven op het moment van account maken:

     ![Fout bij het maken van account](media/cognitive-services-apis-create-account/error-message.png)

5. Als u wilt het account naar de Azure portal-dashboard vastmaken, klikt u op **vastmaken aan Dashboard**.

6. Klik op **Maken** om het account te maken.

Nadat het cognitieve Services-account met succes is geïmplementeerd, klikt u op de tegel in het dashboard om de accountgegevens weer te geven.

U kunt de **eindpunt-URL** in de **overzicht** sectie en sleutels in de **sleutels** sectie om te beginnen met het maken van API-aanroepen in uw toepassingen.

![Accountgegevens weergeven](media/cognitive-services-apis-create-account/display-account.png)

![Toegangscodes weergeven](media/cognitive-services-apis-create-account/account-keys.png)

### <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over alle Microsoft cognitieve Services beschikbaar [cognitieve Services](https://azure.microsoft.com/services/cognitive-services/).

Voor de Quick Start-handleidingen voor het gebruik van een aantal voorbeelden cognitieve Services-API's:

 - [Computer Vision C# snelstartgidsen](/computer-vision/quickstarts/csharp.md)
 - [Tekstanalyse met behulp van Python](/text-analytics/quickstarts/python.md)
 - [Face-API met JavaScript](/face/quickstarts/javascript.md)
