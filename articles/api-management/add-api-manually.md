---
title: Een API handmatig met de Azure-portal toevoegen | Microsoft Docs
description: Deze zelfstudie ziet u het gebruik van API Management (APIM) handmatig toevoegen van een API.
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
ms.openlocfilehash: 9426839f88daece1bb688a2079b7854ccaebdc57
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="add-an-api-manually"></a>Een API handmatig toevoegen 

In dit artikel wordt uitgelegd hoe u handmatig een API toevoegen aan het exemplaar van API Management (APIM) met de Azure-portal. Een veelvoorkomend scenario wanneer u wilt maken van een lege API en handmatig definiëren is wanneer u een model van de API wilt maken. Zie voor meer informatie over een API mocking [antwoorden van een model API](mock-api-responses.md).

Als u importeren van een bestaande API wilt, Zie [Verwante onderwerpen](#related-topics) sectie.

In dit artikel wordt een lege API maken en geef [httpbin.org](http://httpbin.org) (een openbare testen service) als een back-end-API.

## <a name="prerequisites"></a>Vereisten

Voltooi de volgende Snelstartgids: [Azure API Management-exemplaar maken](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-an-api"></a>Een API maken

1. Selecteer **API's** uit onder **API MANAGEMENT**.
2. Selecteer in het menu links **+ API toevoegen**.
3. Selecteer **leeg API** uit de lijst.

    ![Lege API](media/add-api-manually/blank-api.png)
4. Voer de instellingen voor de API.

    ![Instellingen](media/add-api-manually/settings.png)

    |**Naam**|**Waarde**|**Beschrijving**|
    |---|---|---|
    |**Weergavenaam**|'*API leeg*' |Deze naam wordt weergegeven in de portal voor ontwikkelaars.|
    |**Webservice-URL** (optioneel)| '*http://httpbin.org*'| Als u een API model wilt, kunt u alles niet invoeren. <br/>In dit geval we invoeren [http://httpbin.org](http://httpbin.org). Dit is een openbare testen service. <br/>Als u importeren van een API die is toegewezen aan een back-end automatisch wilt, ziet u een van de onderwerpen in de [Verwante onderwerpen](#related-topics) sectie.|
    |**URL-schema**|'*HTTPS*'|In dit geval ondanks dat de back-end niet-beveiligde HTTP-toegang heeft, geven we een beveiligde HTTPS APIM toegang tot de back-end. <br/>Dit soort scenario (HTTPS naar HTTP) wordt de HTTPS-beëindiging genoemd. U kunt dit doen als uw API binnen een virtueel netwerk (waarvan u weet dat de toegang is beveiligd, zelfs als HTTPS wordt niet gebruikt) bestaat. <br/>U kunt gebruiken 'HTTPS-beëindiging' op te slaan op een aantal CPU-cycli.|
    |**URL-achtervoegsel**|'*hbin*'| Het achtervoegsel is een naam op die deze specifieke API in dit exemplaar APIM identificeert. Er moet uniek zijn in dit exemplaar APIM.|
    |**Producten**|'*Onbeperkte*' |De API door de API koppelen aan een product publiceren. Als u wilt dat voor de API worden gepubliceerd en beschikbaar voor ontwikkelaars, kunt u deze toevoegen aan een product. U kunt dit doen tijdens het maken van de API of later instellen.<br/><br/>Producten zijn de koppelingen van een of meer API's. U kunt een aantal API's en ze bieden voor ontwikkelaars via de portal voor ontwikkelaars. <br/>Ontwikkelaars zich eerst abonneren op een product ze toegang krijgen tot de API. Wanneer ze zich abonneert, krijgen ze een abonnementssleutel die geschikt is voor API in dat product. Als u de APIM exemplaar gemaakt, bent u een beheerder al, zodat u bent geabonneerd op elk product standaard.<br/><br/> Elk exemplaar van API Management wordt standaard geleverd met twee voorbeeldproducten: **Starter** en **onbeperkt**.| 
5. Selecteer **Maken**.

U hebt op dit moment geen bewerkingen in APIM die zijn toegewezen aan de bewerkingen in uw back-end-API. Als u een bewerking die beschikbaar is gesteld via de back-end maar niet via de APIM aanroepen, krijgt u een **404**. 

>[!NOTE] 
> Standaard wanneer u een API toevoegen, zelfs als deze is verbonden met sommige back-endservice APIM zal niet worden blootgesteld bewerkingen totdat u geaccepteerde ze. Goedgekeurde IP-adressen een bewerking van uw back-end-service, maakt u een APIM bewerking die is toegewezen aan de back-end-bewerking.
>

## <a name="add-and-test-an-operation"></a>Toevoegen en testen van een bewerking

Deze sectie wordt beschreven hoe een bewerking '/ get' toevoegen aan de back-end 'http://httpbin.org/get'-bewerking worden toegewezen.

### <a name="add-the-operation"></a>De bewerking toevoegen

1. Selecteer de API die u in de vorige stap hebt gemaakt.
2. Klik op **+ toevoegbewerking**.
3. In de **URL**, selecteer **ophalen** en voer '*/get*' in de bron.
4. Voer '*FetchData*' voor **weergavenaam**.
5. Selecteer **Opslaan**.

### <a name="test-the-operation"></a>De bewerking testen

De bewerking te testen in de Azure-portal. U kunt ook kunt u testen in de **ontwikkelaarsportal**.

1. Selecteer de **Test** tabblad.
2. Selecteer **FetchData**.
3. Druk op **verzenden**.

Het antwoord dat de bewerking 'http://httpbin.org/get' genereert wordt weergegeven. Als u wilt voor het transformeren van uw bewerkingen, Zie [transformeren en uw API beveiligen](transform-api.md).

## <a name="add-and-test-a-parameterized-operation"></a>Toevoegen en testen van een bewerking met parameters

Deze sectie wordt beschreven hoe een bewerking die een parameter toevoegen. In dit geval, wijst u de bewerking 'http://httpbin.org/status/200'.

### <a name="add-the-operation"></a>De bewerking toevoegen

1. Selecteer de API die u in de vorige stap hebt gemaakt.
2. Klik op **+ toevoegbewerking**.
3. In de **URL**, selecteer **ophalen** en voer '*/status/ {code}*' in de bron. Desgewenst kunt u sommige gegevens zijn gekoppeld aan deze parameter opgeven. Voer bijvoorbeeld "*getal*' voor **TYPE**, '*200*' (standaard) voor **waarden**.
4. Voer 'GetStatus' voor **weergavenaam**.
5. Selecteer **Opslaan**.

### <a name="test-the-operation"></a>De bewerking testen 

De bewerking te testen in de Azure-portal.  U kunt ook kunt u testen in de **ontwikkelaarsportal**.

1. Selecteer de **Test** tabblad.
2. Selecteer **GetStatus**. De waarde is standaard ingesteld op '*200*'. U kunt deze als u wilt testen, andere waarden wijzigen. Typ bijvoorbeeld "*418*'.
3. Druk op **verzenden**.

    Het antwoord dat de bewerking 'http://httpbin.org/status/200' genereert wordt weergegeven. Als u wilt voor het transformeren van uw bewerkingen, Zie [transformeren en uw API beveiligen](transform-api.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Transformeren en een gepubliceerde API beveiligen](transform-api.md)
