---
title: Implementeren en web-API's en REST API's aanroepen vanuit Azure Logic Apps | Microsoft Docs
description: Implementeren en aanroepen van web-API's en REST API's voor systeem integratio werkstromen in Azure Logic Apps
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, stepsic, LADocs
ms.topic: article
ms.assetid: f113005d-0ba6-496b-8230-c1eadbd6dbb9
ms.date: 05/26/2017
ms.openlocfilehash: a9049ba1fbd7d3bdce061d277f6a7a02d9b1e4b7
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57880061"
---
# <a name="deploy-and-call-custom-apis-from-workflows-in-azure-logic-apps"></a>Implementeren en aanroepen van aangepaste API's van werkstromen in Azure Logic Apps

Nadat u [aangepaste API's maken](./logic-apps-create-api-app.md) voor gebruik in logic app workflows, moet u uw API's implementeren voordat u ze kunt aanroepen. U kunt uw API's als implementeren [web-apps](../app-service/overview.md), maar Overweeg de implementatie van uw API's als [API apps](../app-service/app-service-web-tutorial-rest-api.md), die eenvoudiger uw taak bij het bouwen, hosten en gebruikmaken van API's in de cloud en on-premises. U hoeft niet te wijzigen van de code in uw API's - net implementeert uw code in een API-app. U kunt uw API's hosten op [Azure App Service](../app-service/overview.md), een platform-as-a-serviceoplossing (PaaS) waarmee de uiterst schaalbare webhostingservice met eenvoudige API die als host fungeert.

Hoewel voor de beste ervaring, u een API vanuit een logische app aanroepen kunt, Voeg [OpenAPI (voorheen Swagger) metagegevens](https://swagger.io/specification/) met de beschrijving van uw API bewerkingen en parameters. Deze OpenAPI-bestand kunt u uw API eenvoudiger integreren en beter werken met logic apps.

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a>Uw API als een web-app of API-app implementeren

Voordat u uw aangepaste API vanuit een logische app aanroepen kunt, moet u uw API als een web-app of API-app implementeren in Azure App Service. Ook als u uw OpenAPI-bestand gelezen door de ontwerper van logische Apps, stelt u de eigenschappen van API-definitie en schakel [cross-origin resource sharing (CORS)](../app-service/overview.md) voor uw web-app of API-app.

1. In de [Azure-portal](https://portal.azure.com), selecteert u uw web-app of API-app.

2. In het menu van de app die wordt geopend, onder **API**, kiest u **API-definitie**. Stel de **locatie van de API-definitie** aan de URL voor uw OpenAPI swagger.json-bestand.

   Normaal gesproken de URL wordt weergegeven in deze indeling: `https://{name}.azurewebsites.net/swagger/docs/v1)`

   ![Koppeling naar de OpenAPI-bestand voor uw aangepaste API](./media/logic-apps-custom-api-deploy-call/custom-api-swagger-url.png)

3. Onder **API**, kiest u **CORS**. Stel het CORS-beleid voor **oorsprongen toegestaan** naar **' *'** (alle toestaan).

   Deze instelling kan aanvragen vanuit Logic App Designer.

   ![Toestaan van aanvragen van Logic App Designer voor uw aangepaste API](./media/logic-apps-custom-api-deploy-call/custom-api-cors.png)

Zie voor meer informatie, [een RESTful API hosten met CORS in Azure App Service](../app-service/app-service-web-tutorial-rest-api.md).

## <a name="call-your-custom-api-from-logic-app-workflows"></a>Uw aangepaste API oproept vanuit logic app workflows

Na het instellen van de eigenschappen van de API-definitie en CORS, moeten triggers en acties van uw aangepaste API's beschikbaar zijn voor u om op te nemen in uw werkstroom voor logische Apps. 

*  Als u wilt weergeven van websites met OpenAPI-URL's, kunt u uw abonnement-websites in de ontwerper van logische Apps bladeren.

*  U kunt beschikbare acties en invoer weergeven door aan te wijzen op een OpenAPI-document, met de [HTTP + Swagger actie](../connectors/connectors-native-http-swagger.md).

*  Voor het aanroepen van een API, met inbegrip van API's die niet zijn of beschikbaar maken van een OpenAPI-document, kunt u altijd een aanvraag met maken de [HTTP-actie](../connectors/connectors-native-http.md).

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van aangepaste connectors](../logic-apps/custom-connector-overview.md)