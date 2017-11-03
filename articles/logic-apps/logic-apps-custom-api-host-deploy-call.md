---
title: Implementeren en web-API's & REST-API's aanroepen vanuit Azure Logic Apps | Microsoft Docs
description: Implementeren en uw web-API's & REST-API's voor systeem integratie werkstromen in Azure Logic Apps aanroepen
keywords: "Web-API's, REST-API's, connectors, werkstromen, systeemintegraties, verifiëren"
services: logic-apps
author: stepsic-microsoft-com
manager: anneta
editor: 
documentationcenter: 
ms.assetid: f113005d-0ba6-496b-8230-c1eadbd6dbb9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: LADocs; stepsic
ms.openlocfilehash: 3df59ec172b037daaeed9e3eb69ffb990d70d8a5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-and-call-custom-apis-from-logic-app-workflows"></a>Implementeren en het aanroepen van aangepaste API's vanuit logic app-werkstromen

Nadat u [maken van aangepaste API's](./logic-apps-create-api-app.md) voor gebruik in logic app werkstromen, moet u uw API's implementeren voordat u ze kunt aanroepen. U kunt uw API's als implementeren [web-apps](../app-service/app-service-web-overview.md), maar overweeg het implementeren van uw API's als [API-apps](../app-service/app-service-web-tutorial-rest-api.md), die efficiënter werken wanneer u ontwikkelen, hosten en API's in de cloud en on-premises gebruiken. U hoeft niet te wijzigen van de code in de API's: uw code hoeft te implementeren naar een API-app. U kunt uw API's hosten op [Azure App Service](../app-service/app-service-web-overview.md), een platform-as-a-service (PaaS) en biedt in hoge mate schaalbaar, eenvoudige API die als host fungeert van de aanbieding.

Hoewel voor de beste ervaring u API vanuit een logische app aanroepen kunt, Voeg [OpenAPI (eerder Swagger) metagegevens](http://swagger.io/specification/) die uw API bewerkingen en parameters beschrijft. Dit bestand OpenAPI helpt uw API eenvoudiger integreren en beter werkt met logic apps.

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a>Uw API als een web-app of API-app implementeren

Voordat u uw aangepaste API vanuit een logische app aanroepen kunt, moet u uw API als een web-app of API-app implementeren in Azure App Service. Ook als u uw bestand kan worden gelezen door de ontwerper van Logic Apps OpenAPI, de API-definitie-eigenschappen instellen en inschakelen [cross-origin-resource delen (CORS)](../app-service/app-service-web-overview.md) voor uw web-app of API-app.

1. In de [Azure-portal](https://portal.azure.com), selecteer uw web-app of API-app.

2. In het app-menu dat verschijnt, klikt u onder **API**, kies **API-definitie**. Stel de **locatie voor API-definitie** naar de URL voor uw OpenAPI swagger.json-bestand.

   Normaal gesproken de URL wordt weergegeven in deze indeling:`https://{name}.azurewebsites.net/swagger/docs/v1)`

   ![Koppeling naar OpenAPI-bestand voor uw aangepaste API](./media/logic-apps-custom-api-deploy-call/custom-api-swagger-url.png)

3. Onder **API**, kies **CORS**. Stel het CORS-beleid voor **toegestane oorsprongen** naar  **'*'** (alle toestaan).

   Deze instelling kan aanvragen van Logic App-ontwerper.

   ![Aanvragen van Logic App-ontwerper verlenen aan uw aangepaste API gebruiken](./media/logic-apps-custom-api-deploy-call/custom-api-cors.png)

Zie voor meer informatie [bouwen van een Node.js-RESTful-API](../app-service/app-service-web-tutorial-rest-api.md).

## <a name="call-your-custom-api-from-logic-app-workflows"></a>Uw aangepaste API aanroepen vanuit logic app-werkstromen

Nadat u de eigenschappen van de API-definitie en CORS hebt ingesteld, uw aangepaste API triggers en acties moeten beschikbaar zijn voor u in uw logische app-werkstroom op te nemen. 

*  Als u wilt weergeven van websites die OpenAPI URL's hebben, kunt u de websites van uw abonnement in de ontwerpfunctie voor Logic Apps bladeren.

*  U kunt beschikbare acties en invoer weergeven door aan te wijzen aan een document OpenAPI, met de [HTTP + Swagger actie](../connectors/connectors-native-http-swagger.md).

*  Om aan te roepen API, met inbegrip van API's die niet zijn of een document OpenAPI weergeven kunt u altijd een aanvraag met maken de [HTTP-actie](../connectors/connectors-native-http.md).

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van aangepaste connector](../logic-apps/custom-connector-overview.md)