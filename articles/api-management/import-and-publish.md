---
title: Uw eerste API importeren en publiceren in Azure API Management | Microsoft Docs
description: Informatie over het importeren en publiceren van uw eerste API met API Management.
services: api-management
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/15/2017
ms.author: apimpm
ms.openlocfilehash: 26a5ab93c82b523063632c4899daadbde70dd7d8
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="import-and-publish-your-first-api"></a>Uw eerste API importeren en publiceren 

In deze zelfstudie ziet u hoe u een back-end-API met de naam OpenAPI-specificatie kunt importeren die zich bevindt in http://conferenceapi.azurewebsites.net?format=json. Deze back-end-API wordt geleverd met Microsoft en gehost in Azure. 

Zodra de back-end-API is geïmporteerd in APIM (API Management), wordt de APIM API een façade voor de back-end-API. Op het moment dat u de back-end-API importeert, zijn de bron-API en de APIM API identiek. Met APIM kunt u de façade aanpassen aan uw behoeften zonder dat de back-end-API wordt gewijzigd. Zie [Transform and protect your API](transform-api.md) (Uw API transformeren en beveiligen) voor meer informatie. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Uw eerste API importeren
> * De API testen in Azure Portal
> * De API testen in de ontwikkelaarsportal

![Nieuwe API](./media/api-management-get-started/created-api.png)

## <a name="prerequisites"></a>Vereisten

Lees de volgende snelstartgids: [Een Azure API Management-exemplaar maken](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-api"> </a>Een API importeren en publiceren

In deze sectie ziet u hoe u een back-end-API met de naam OpenAPI-specificatie kunt importeren en publiceren.
 
1. Selecteer **API's** bij **API MANAGEMENT**.
2. Selecteer **OpenAPI-specificatie** in de lijst.

    ![Een API maken](./media/api-management-get-started/create-api.png)

    U kunt de API-waarden instellen tijdens het maken of later door naar het tabblad **Instellingen** te gaan. De rode ster naast een veld geeft aan dat het veld verplicht is.

    |Instelling|Waarde|Beschrijving|
    |---|---|---|
    |**OpenAPI-specificatie**|http://conferenceapi.azurewebsites.net?format=json|Verwijst naar de service waarmee de API wordt geïmplementeerd. API Management stuurt aanvragen door naar dit adres.|
    |**Weergavenaam**|*Demo Conference API*|Als u op Tab drukt nadat u de service-URL hebt ingevoerd, wordt dit veld ingevuld met APIM op basis van de inhoud van het JSON-bestand. <br/>Deze naam wordt weergegeven in de ontwikkelaarsportal.|
    |**Naam**|*demo-conference-api*|Biedt een unieke naam voor de API. <br/>Als u op Tab drukt nadat u de service-URL hebt ingevoerd, wordt dit veld ingevuld met APIM op basis van de inhoud van het JSON-bestand.|
    |**Beschrijving**|Geef een optionele beschrijving van de API.|Als u op Tab drukt nadat u de service-URL hebt ingevoerd, wordt dit veld ingevuld met APIM op basis van de inhoud van het JSON-bestand.|
    |**URL-schema**|*HTTPS*|Bepaalt welke protocollen kunnen worden gebruikt om toegang te krijgen tot de API. |
    |**API-URL-achtervoegsel**|*conference*|Het achtervoegsel wordt toegevoegd aan de basis-URL voor de API Management-service. In API Management worden API's herkend aan hun achtervoegsel en daarom moet het achtervoegsel uniek zijn voor elke API voor een bepaalde uitgever.|
    |**Producten**|*Onbeperkt*|Producten zijn koppelingen van een of meer API's. U kunt een aantal API's opnemen in een product en deze beschikbaar stellen voor ontwikkelaars via de ontwikkelaarsportal. <br/>U publiceert de API door deze te koppelen aan een product (in dit voorbeeld *Onbeperkt*). Als u deze nieuwe API wilt toevoegen aan een product, typt u de productnaam (u kunt dit ook later doen op de pagina **Instellingen**). Deze stap kan meerdere keren worden herhaald om de API toe te voegen aan meerdere producten.<br/>Ontwikkelaars moeten zich eerst abonneren op een product om toegang tot de API te krijgen. Wanneer ontwikkelaars zich abonneren, ontvangen ze een abonnementssleutel die toegang biedt tot elke API in het betreffende product. <br/> Als u de APIM-instantie hebt gemaakt, bent u al een beheerder en bent u geabonneerd op elk product.<br/> Standaard wordt elke API Management-instantie geleverd met twee voorbeeldproducten: **Starter** en **Onbeperkt**. |
    |Versie van deze API?||Zie [Publish multiple versions of your API](api-management-get-started-publish-versions.md) (Meerdere versies van uw API publiceren) voor meer informatie over versiebeheer|
    
    >[!NOTE]
    > Als u de API wilt publiceren, moet u deze koppelen aan een product. U kunt dit doen op de pagina **Instellingen**.
    
3. Selecteer **Maken**.

## <a name="test-the-new-apim-api-in-the-azure-portal"></a>De nieuwe APIM API testen in Azure Portal

Bewerkingen kunnen rechtstreeks vanuit Azure Portal worden aangeroepen. Dit is een handige manier om de bewerkingen van een API te bekijken en te testen.  
1. Selecteer de API die u in de vorige stap hebt gemaakt (op het tabblad **API's**).
2. Druk op het tabblad **Testen**.  ![API testen](./media/api-management-get-started/test-api.png)
3. Klik op **GetSpeakers**.
    Op de pagina worden velden weergegeven voor queryparameters, maar in dit geval zijn er geen queryparameters. Op de pagina worden ook velden weergegeven voor de headers. Een van de headers is Ocp-Apim-Subscription-Key voor de abonnementssleutel van het product dat is gekoppeld aan deze API. De sleutel wordt automatisch ingevuld.
4. Druk op **Verzenden**.

    Back-end reageert met **200 OK** en enkele gegevens.

## <a name="call-operation"> </a>Een bewerking aanroepen vanuit de ontwikkelaarsportal

Bewerkingen kunnen ook worden aangeroepen vanuit de **ontwikkelaarsportal** om de API's te testen. 

1. Selecteer **Demo Conference API**.
2. Klik op **GetSpeakers**.
    
    Op de pagina worden velden weergegeven voor queryparameters, maar in dit geval zijn er geen queryparameters. Op de pagina worden ook velden weergegeven voor de headers. Een van de headers is Ocp-Apim-Subscription-Key voor de abonnementssleutel van het product dat is gekoppeld aan deze API. Als u de APIM-instantie hebt gemaakt, bent u al een beheerder en wordt de sleutel automatisch ingevoerd.
3. Druk op **Proberen**.
4. Druk op **Verzenden**.
    
    Nadat een bewerking is aangeroepen, worden de antwoorden weergegeven in de ontwikkelaarsportal.  

## <a name="next-steps"> </a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Uw eerste API importeren
> * De API testen in Azure Portal
> * De API testen in de ontwikkelaarsportal

Ga door naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Een product maken en publiceren](api-management-howto-add-products.md)
