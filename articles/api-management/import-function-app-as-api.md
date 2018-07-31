---
title: Een Azure Functions-app als API importeren in Azure Portal | Microsoft Docs
description: Deze zelfstudie laat zien hoe u Azure API Management moet gebruiken om een Azure Functions-app als API te importeren.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/15/2018
ms.author: apimpm
ms.openlocfilehash: 670fa58de7155028b0f72f1f819b9f269e07b9eb
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39239049"
---
# <a name="import-an-azure-functions-app-as-an-api"></a>Een Azure Functions-app als API importeren

In dit artikel wordt uitgelegd hoe u een Azure Functions-app als API importeert. In het artikel wordt ook uitgelegd hoe u de Azure API Management-API kunt testen.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een Functions-app als API importeren
> * De API testen in Azure Portal
> * De API testen in de ontwikkelaarsportal

## <a name="prerequisites"></a>Vereisten

+ Lees de snelstart [Een Azure API Management-exemplaar maken](get-started-create-service-instance.md).
+ Controleer of er een Azure Functions-app in uw abonnement aanwezig is. Zie [Een functie-app maken](../azure-functions/functions-create-first-azure-function.md#create-a-function-app) voor meer informatie.
+ [Maak een OpenAPI-definitie](../azure-functions/functions-openapi-definition.md) van uw Azure Functions-app.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"></a>Een back-end-API importeren en publiceren

1. Selecteer **API's** onder **API MANAGEMENT**.
2. Selecteer **Functions App** in de lijst **Add a new API**.

    ![Functions-app](./media/import-function-app-as-api/function-app-api.png)
3. Selecteer **Bladeren** voor een overzicht van de Functions-apps in uw abonnement.
4. Selecteer de app. API Management zoekt de swagger die is gekoppeld aan de geselecteerde app, haalt deze op en importeert deze. 
5. Voeg een achtervoegsel toe van de URL voor de API. Het achtervoegsel is een naam die deze specifieke API in dit API Management-exemplaar identificeert. Het achtervoegsel moet uniek zijn in dit exemplaar van API Management.
6. Publiceer de API door deze aan een product te koppelen. In dit geval wordt het product **Onbeperkt** gebruikt. Als u wilt dat de API wordt gepubliceerd en beschikbaar komt voor ontwikkelaars, voegt u de API aan een product toe. U kunt de API aan een product toevoegen wanneer u de API maakt. U kunt de API ook later toevoegen.

    Producten zijn koppelingen van een of meer API's. U kunt meerdere API's opnemen en deze beschikbaar stellen voor ontwikkelaars via de ontwikkelaarsportal. Ontwikkelaars moeten zich eerst abonneren op een product om toegang tot de API te krijgen. Wanneer een ontwikkelaar zich abonneert, ontvangt hij of zij een abonnementssleutel die toegang biedt tot elke API in het betreffende product. Als u het API Management-exemplaar hebt gemaakt, bent u een beheerder. Beheerders zijn standaard geabonneerd op elk product.

    Standaard wordt elk API Management-exemplaar geleverd met twee voorbeeldproducten:

    * **Starter**
    * **Onbeperkt**   
7. Selecteer **Maken**.

## <a name="populate-azure-functions-keys-in-azure-api-management"></a>Azure Function-sleutels invullen in Azure API Management

Als de geïmporteerde Azure Functions-apps door sleutels worden beveiligd, worden er automatisch *benoemde waarden* voor de sleutels gemaakt. De vermeldingen worden niet door API Management met geheimen gevuld. Voltooi voor elke vermelding de volgende stappen:  

1. Selecteer het tabblad **Benoemde waarden** in het API Management-exemplaar.
2. Selecteer een vermelding en selecteer vervolgens **Waarde weergeven** in de zijbalk.

    ![Benoemde waarden](./media/import-function-app-as-api/apim-named-values.png)

3. Als de tekst in het vak **Waarde** gelijk is aan **code for \<Azure Functions-naam\>**, gaat u naar **Functions Apps** en vervolgens naar **Functions**.
4. Selecteer **Beheren** en kopieer de relevante sleutel, dus de sleutel die is gebaseerd op de verificatiemethode van uw app.

    ![Functions-app - Sleutels kopiëren](./media/import-function-app-as-api/azure-functions-app-keys.png)

5. Plak de sleutel in het vak **Waarde** en selecteer **Opslaan**.

    ![Functions-app - Sleutelwaarden plakken](./media/import-function-app-as-api/apim-named-values-2.png)

## <a name="test-the-new-api-management-api-in-the-azure-portal"></a>De nieuwe API Management-API testen in Azure Portal

U kunt bewerkingen rechtstreeks vanuit Azure Portal aanroepen. In Azure Portal kunt u de bewerkingen van een API gemakkelijk bekijken en testen.  

1. Selecteer de API die u in de voorgaande sectie hebt gemaakt.
2. Selecteer het tabblad **Testen**.
3. Selecteer een bewerking.

    De pagina geeft velden weer voor queryparameters en velden voor de headers. Een van de headers is **Ocp-Apim-Subscription-Key** voor de abonnementssleutel van het product dat is gekoppeld aan deze API. Als u het API Management-exemplaar hebt gemaakt, bent u al een beheerder en wordt de sleutel dus automatisch ingevuld. 
4. Selecteer **Verzenden**.

    Back-end reageert met **200 OK** en enkele gegevens.

## <a name="call-operation"></a>Een bewerking aanroepen vanuit de ontwikkelaarsportal

U kunt ook bewerkingen aanroepen vanuit de ontwikkelaarsportal om API's te testen. 

1. Selecteer de API die u hebt gemaakt in [Een back-end-API importeren en publiceren](#create-api).
2. Selecteer **Ontwikkelaarsportal**.

    De site voor de ontwikkelaarsportal wordt geopend.
3. Selecteer de **API** die u hebt gemaakt.
4. Selecteer de bewerking die u wilt testen.
5. Selecteer **Probeer het nu**.
6. Selecteer **Verzenden**.
    
    Nadat een bewerking is aangeroepen, worden in de ontwikkelaarsportal de **antwoordstatus**, de **antwoordheaders** en eventuele **antwoordinhoud** weergegeven.

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een gepubliceerde API transformeren en beveiligen](transform-api.md)