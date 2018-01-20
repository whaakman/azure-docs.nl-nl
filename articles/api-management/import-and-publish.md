---
title: Importeren en publiceren van uw eerste API in Azure API Management | Microsoft Docs
description: Informatie over het importeren en publiceren van uw eerste API met API Management.
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: apimpm
ms.openlocfilehash: ffe5ee95c66eee7dccd25a1afd2fe639cbc273f5
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="import-and-publish-your-first-api"></a>Importeren en publiceren van uw eerste API 

Deze zelfstudie laat zien hoe een 'OpenAPI specificatie' back-end-API die zich op http://conferenceapi.azurewebsites.net?format=json importeren. Deze back-end-API is geleverd door Microsoft en wordt gehost op Azure. 

Nadat de back-end API is geïmporteerd in API Management (APIM), wordt de API APIM een façade voor de end-API. Op het moment dat u de back-end API importeren, zijn zowel de bron-API en de API APIM identiek. APIM kunt u de façade aan uw wensen zonder de end-API aan te passen. Zie voor meer informatie [transformeren en uw API beveiligen](transform-api.md). 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Uw eerste API importeren
> * De API testen in de Azure-portal
> * De API testen in de portal voor ontwikkelaars

![Nieuwe API](./media/api-management-get-started/created-api.png)

## <a name="prerequisites"></a>Vereisten

Voltooi de volgende Snelstartgids: [Azure API Management-exemplaar maken](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Importeren en publiceren van een back-end API

Deze sectie wordt beschreven hoe importeren en publiceren van een OpenAPI specificatie van het back-end API.
 
1. Selecteer **API's** uit onder **API MANAGEMENT**.
2. Selecteer **OpenAPI specificatie** uit de lijst.

    ![Een API maken](./media/api-management-get-started/create-api.png)

    U kunt de API-waarden instellen tijdens het maken of later door te gaan naar de **instellingen** tabblad.  

    |Instelling|Waarde|Beschrijving|
    |---|---|---|
    |**OpenAPI specificatie**|http://conferenceapi.azurewebsites.net?format=json|Verwijst naar de implementatie van de API-service. API management verzendt aanvragen naar dit adres.|
    |**Weergavenaam**|*Demo conferentie API*|Als u na het invoeren van de service-URL op tab drukt, APIM dit veld ingevuld op basis van wat is er in de json. <br/>Deze naam wordt weergegeven in de portal voor ontwikkelaars.|
    |**Naam**|*demo-conference-api*|Biedt een unieke naam voor de API. <br/>Als u na het invoeren van de service-URL op tab drukt, APIM dit veld ingevuld op basis van wat is er in de json.|
    |**Beschrijving**|Geef een optionele beschrijving van de API.|Als u na het invoeren van de service-URL op tab drukt, APIM dit veld ingevuld op basis van wat is er in de json.|
    |**Achtervoegsel URL-API**|*telefonische vergaderingen*|Het achtervoegsel wordt toegevoegd aan de basis-URL voor de API management-service. API Management API's onderscheidt door hun achtervoegsel en daarom het achtervoegsel moet uniek zijn voor elke API voor een opgegeven uitgever.|
    |**URL-schema**|*HTTPS*|Hiermee wordt bepaald welke protocollen kunnen worden gebruikt voor toegang tot de API. |
    |**Producten**|*Onbeperkt*| De API door de API koppelen aan een product publiceren. Typ om eventueel deze nieuwe API toevoegen aan een product, de productnaam. Deze stap kan meerdere keren toevoegen van de API aan meerdere producten worden herhaald.<br/>Producten zijn de koppelingen van een of meer API's. U kunt een aantal API's en ze bieden voor ontwikkelaars via de portal voor ontwikkelaars. Ontwikkelaars zich eerst abonneren op een product ze toegang krijgen tot de API. Wanneer ze zich abonneert, krijgen ze een abonnementssleutel die geschikt is voor API in dat product. Als u de APIM exemplaar gemaakt, bent u een beheerder al, zodat u bent geabonneerd op elk product standaard.<br/> Elk exemplaar van API Management wordt standaard geleverd met twee voorbeeldproducten: **Starter** en **onbeperkt**. |
3. Selecteer **Maken**.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>De nieuwe APIM API testen in de Azure-portal

Bewerkingen kunnen rechtstreeks vanuit de Azure portal, die een handige manier om te bekijken en te testen van de bewerkingen van een API worden aangeroepen.  
1. Selecteer de API die u in de vorige stap hebt gemaakt.
2. Druk op de **Test** tabblad.  ![API testen](./media/api-management-get-started/test-api.png)
3. Klik op **GetSpeakers**.
    Velden voor queryparameters op de pagina weergegeven, maar in dit geval zijn er een. Velden voor de headers van de pagina worden ook weergegeven. Een van de headers is 'Ocp-Apim-Subscription-Key' voor de abonnementssleutel van het product dat is gekoppeld aan deze API. De sleutel wordt automatisch ingevuld.
4. Druk op **Verzenden**.

    Back-end reageert met **200 OK** en bepaalde gegevens.

## <a name="call-operation"> </a>Een bewerking aanroepen vanuit de ontwikkelaarsportal

Bewerkingen kunnen ook worden aangeroepen **ontwikkelaarsportal** voor het testen van API's. 

1. Selecteer de API die u hebt gemaakt in de ' importeren en publiceren van een back-end-API ' stap.
2. Druk op **ontwikkelaarsportal**.

    ![Testen in de portal voor ontwikkelaars](./media/api-management-get-started/developer-portal.png)

    De site 'Ontwikkelaarsportal' wordt geopend.
3. Selecteer **API**.
4. Selecteer **Demo conferentie API**.
5. Klik op **GetSpeakers**.
    
    Velden voor queryparameters op de pagina weergegeven, maar in dit geval zijn er een. Velden voor de headers van de pagina worden ook weergegeven. Een van de headers is 'Ocp-Apim-Subscription-Key' voor de abonnementssleutel van het product dat is gekoppeld aan deze API. Als u het exemplaar APIM hebt gemaakt, bent u beheerder al, zodat de sleutel wordt automatisch ingevuld.
6. Druk op **Try it**.
7. Druk op **Verzenden**.
    
    Nadat een bewerking is aangeroepen, worden in de ontwikkelaarsportal de **antwoordstatus**, de **antwoordheaders** en eventuele **antwoordinhoud** weergegeven.

## <a name="next-steps"> </a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Uw eerste API importeren
> * De API testen in de Azure-portal
> * De API testen in de portal voor ontwikkelaars

Ga naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Een product maken en publiceren](api-management-howto-add-products.md)
