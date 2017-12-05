---
title: Een OpenAPI importspecificatie die met de Azure portal | Microsoft Docs
description: Informatie over het importeren van een specificatie OpenAPI met API Management.
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/22/2017
ms.author: apimpm
ms.openlocfilehash: f0c77c6e959ca99698b3ea704756a6abf36147f3
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="import-an-openapi-specification"></a>Een aanduiding OpenAPI importeren

In dit artikel laat zien hoe een 'OpenAPI specificatie' back-end-API die zich op http://conferenceapi.azurewebsites.net?format=json importeren. Deze back-end-API is geleverd door Microsoft en wordt gehost op Azure. Ook wordt uitgelegd hoe u kunt de API APIM testen.

In dit artikel leert u hoe:

> [!div class="checklist"]
> * Een back-end 'OpenAPI specificatie' API importeren
> * De API testen in de Azure-portal
> * De API testen in de portal voor ontwikkelaars

## <a name="prerequisites"></a>Vereisten

Voltooi de volgende Snelstartgids: [Azure API Management-exemplaar maken](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Importeren en publiceren van een back-end-API

1. Selecteer **API's** uit onder **API MANAGEMENT**.
2. Selecteer **OpenAPI specificatie** van de **een nieuwe API toevoegen** lijst.
    ![OpenAPI specificatie](./media/import-api-from-oas/oas-api.png)
3. Voer de juiste instellingen. Tijdens het maken, kunt u alle API-waarden instellen. U kunt ook u kunt ze later instellen door te gaan naar de **instellingen** tabblad. <br/> Als u op **tabblad** sommige of alle van de velden ophalen gevuld met de gegevens van de opgegeven back-end-service.

    ![Een API maken](./media/api-management-get-started/create-api.png)

    |Instelling|Waarde|Beschrijving|
    |---|---|---|
    |**OpenAPI specificatie**|http://conferenceapi.azurewebsites.NET?Format=JSON|Verwijst naar de implementatie van de API-service. API management verzendt aanvragen naar dit adres.|
    |**Weergavenaam**|*Demo conferentie API*|Als u na het invoeren van de service-URL op tab drukt, APIM dit veld ingevuld op basis van wat is er in de json. <br/>Deze naam wordt weergegeven in de portal voor ontwikkelaars.|
    |**Naam**|*demo-conferentie-api*|Biedt een unieke naam voor de API. <br/>Als u na het invoeren van de service-URL op tab drukt, APIM dit veld ingevuld op basis van wat is er in de json.|
    |**Beschrijving**|Geef een optionele beschrijving van de API.|Als u na het invoeren van de service-URL op tab drukt, APIM dit veld ingevuld op basis van wat is er in de json.|
    |**Achtervoegsel URL-API**|*telefonische vergaderingen*|Het achtervoegsel wordt toegevoegd aan de basis-URL voor de API management-service. API Management API's onderscheidt door hun achtervoegsel en daarom het achtervoegsel moet uniek zijn voor elke API voor een opgegeven uitgever.|
    |**URL-schema**|*HTTPS*|Hiermee wordt bepaald welke protocollen kunnen worden gebruikt voor toegang tot de API. |
    |**Producten**|*Onbeperkt*| De API door de API koppelen aan een product publiceren. Typ om eventueel deze nieuwe API toevoegen aan een product, de productnaam. Deze stap kan meerdere keren toevoegen van de API aan meerdere producten worden herhaald.<br/>Producten zijn de koppelingen van een of meer API's. U kunt een aantal API's en ze bieden voor ontwikkelaars via de portal voor ontwikkelaars. Ontwikkelaars zich eerst abonneren op een product ze toegang krijgen tot de API. Wanneer ze zich abonneert, krijgen ze een abonnementssleutel die geschikt is voor API in dat product. Als u de APIM exemplaar gemaakt, bent u een beheerder al, zodat u bent geabonneerd op elk product standaard.<br/> Elk exemplaar van API Management wordt standaard geleverd met twee voorbeeldproducten: **Starter** en **onbeperkt**. |

4. Selecteer **Maken**.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>De nieuwe APIM API testen in de Azure-portal

Bewerkingen kunnen rechtstreeks vanuit de Azure portal, die een handige manier om te bekijken en te testen van de bewerkingen van een API worden aangeroepen.  

1. Selecteer de API die u in de vorige stap hebt gemaakt.
2. Druk op de **Test** tabblad.

    ![API testen](./media/api-management-get-started/test-api.png)
1. Klik op **GetSpeakers**.

    Velden voor queryparameters op de pagina weergegeven, maar in dit geval zijn er een. Velden voor de headers van de pagina worden ook weergegeven. Een van de headers is 'Ocp-Apim-Subscription-Key' voor de abonnementssleutel van het product dat is gekoppeld aan deze API. Als u het exemplaar APIM hebt gemaakt, bent u beheerder al, zodat de sleutel wordt automatisch ingevuld. 
4. Druk op **verzenden**.

    Back-end reageert met **200 OK** en bepaalde gegevens.

## <a name="call-operation"></a>Een bewerking aanroepen vanuit de portal voor ontwikkelaars

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
7. Druk op **verzenden**.
    
    Nadat een bewerking is aangeroepen, worden in de ontwikkelaarsportal de **antwoordstatus**, de **antwoordheaders** en eventuele **antwoordinhoud** weergegeven.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Transformeren en een gepubliceerde API beveiligen](transform-api.md)
