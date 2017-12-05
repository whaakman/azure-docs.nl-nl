---
title: Transformeren en uw API beveiligen met Azure API Management | Microsoft Docs
description: Informatie over het beveiligen van uw API met beleidsregels voor quota en (frequentie)beperking.
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
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 772f3828d85c54e7b8bb44c857e555175b7444cc
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="transform-and-protect-your-api"></a>Transformeren en uw API beveiligen 

De zelfstudie laat zien hoe uw API transformeren zodat deze een persoonlijke back-end-info niet vrijgegeven. U wilt verbergen, de informatie over technologiestack die wordt uitgevoerd op de back-end. U kunt ook de oorspronkelijke URL's die worden weergegeven in de hoofdtekst van de HTTP-antwoord van de API en in plaats daarvan hen omleidt naar de gateway APIM verbergen.

Deze zelfstudie ook ziet u hoe eenvoudig het is voor het toevoegen van beveiliging voor uw back-end API door frequentielimiet configureren met Azure API Management. U wilt bijvoorbeeld een aantal van de API wordt aangeroepen, zodat deze niet wordt gebruikt door ontwikkelaars aanroepen beperken. Zie voor meer informatie [API Management-beleidsregels](api-management-policies.md)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een API om antwoordheaders van strook / transformeren
> * Oorspronkelijke URL's in de hoofdtekst van de API-reactie vervangen door APIM gateway-URL 's
> * Een API beveiligen door beleid voor Frequentielimiet (beperking) toe te voegen
> * De transformaties testen

![Beleidsregels](./media/transform-api/api-management-management-console.png)

## <a name="prerequisites"></a>Vereisten

+ Voltooi de volgende Snelstartgids: [Azure API Management-exemplaar maken](get-started-create-service-instance.md).
+ Ook de volgende zelfstudie te voltooien: [importeren en publiceren van uw eerste API](import-and-publish.md).
 
[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="transform-an-api-to-strip-response-headers"></a>Een API om antwoordheaders van strook / transformeren

Deze sectie wordt beschreven hoe de HTTP-headers die u niet wilt weergeven voor uw gebruikers te verbergen. In dit voorbeeld wordt de volgende headers verwijderd in het HTTP-antwoord:

* **X aangedreven door**
* **X-AspNet-versie**

### <a name="test-the-original-response"></a>Het oorspronkelijke antwoord testen

Naar de oorspronkelijke antwoord weergegeven:

1. Selecteer de **API** tabblad.
2. Klik op **Demo conferentie API** uit de lijst met API.
3. Selecteer de **GetSpeakers** bewerking.
4. Klik op de **Test** tabblad boven aan het scherm.
5. Druk op de **verzenden** knop aan de onderkant van het scherm. 

    Zoals u dat het oorspronkelijke antwoord ziet er als volgt ziet:

    ![Beleidsregels](./media/transform-api/original-response.png)

### <a name="set-the-transformation-policy"></a>Stel het beleid voor transformatie

1. Blader naar uw APIM-exemplaar.
2. Selecteer de **API** tabblad.
3. Klik op **Demo conferentie API** uit de lijst met API.
4. Selecteer **alle bewerkingen**.
5. Selecteer boven aan het scherm **ontwerp** tabblad.
6. In de **uitgaande verwerking** venster, klikt u op de driehoek (naast de pen).
7. Selecteer **Code-editor**.
    
     ![Beleid bewerken](./media/set-edit-policies/set-edit-policies01.png)
9. Plaats de cursor in de  **<outbound>**  element.
10. In het rechtervenster onder **claimtransformatiebeleidsinstellingen**, klikt u op **+ Set HTTP-header** tweemaal (tot twee beleid codefragmenten invoegen).

    ![Beleidsregels](./media/transform-api/transform-api.png)
11. Wijzig uw  **<outbound>**  code als volgt:

        <set-header name="X-Powered-By" exists-action="delete" />
        <set-header name="X-AspNet-Version" exists-action="delete" />
                
## <a name="replace-original-urls-in-the-body-of-the-api-response-with-apim-gateway-urls"></a>Oorspronkelijke URL's in de hoofdtekst van de API-reactie vervangen door APIM gateway-URL 's

Deze sectie wordt beschreven hoe oorspronkelijke URL's die worden weergegeven in de hoofdtekst van de HTTP-antwoord van de API en in plaats daarvan hen omleidt naar de gateway APIM verbergen.

### <a name="test-the-original-response"></a>Het oorspronkelijke antwoord testen

Naar de oorspronkelijke antwoord weergegeven:

1. Selecteer de **API** tabblad.
2. Klik op **Demo conferentie API** uit de lijst met API.
3. Selecteer de **GetSpeakers** bewerking.
4. Klik op de **Test** tabblad boven aan het scherm.
5. Druk op de **verzenden** knop aan de onderkant van het scherm. 

    Zoals u dat het oorspronkelijke antwoord ziet er als volgt ziet:

    ![Beleidsregels](./media/transform-api/original-response2.png)

### <a name="set-the-transformation-policy"></a>Stel het beleid voor transformatie

1. Blader naar uw APIM-exemplaar.
2. Selecteer de **API** tabblad.
3. Klik op **Demo conferentie API** uit de lijst met API.
4. Selecteer **alle bewerkingen**.
5. Selecteer boven aan het scherm **ontwerp** tabblad.
6. In de **uitgaande verwerking** venster, klikt u op de driehoek (naast de pen).
7. Selecteer **Code-editor**.
8. Plaats de cursor in de  **<outbound>**  element.
9. In het rechtervenster onder **claimtransformatiebeleidsinstellingen**, klikt u op **+ zoeken en vervangen tekenreeks in de hoofdtekst**.
10. Wijzig uw **< zoeken en vervangen** code (in de  **<outbound>**  element) de URL overeenkomt met uw gateway APIM vervangen. Bijvoorbeeld:

        <find-and-replace from="://conferenceapi.azurewebsites.net" to="://apiphany.azure-api.net/conference"/>

## <a name="protect-an-api-by-adding-rate-limit-policy-throttling"></a>Een API beveiligen door beleid voor Frequentielimiet (beperking) toe te voegen

Deze sectie wordt beschreven hoe u beveiliging voor uw back-end API toevoegen door de frequentielimieten configureren. U wilt bijvoorbeeld een aantal van de API wordt aangeroepen, zodat deze niet wordt gebruikt door ontwikkelaars aanroepen beperken. In dit voorbeeld wordt is de limiet ingesteld op 3 aanroepen per 15 seconden voor elk abonnement-id. Na 15 seconden kan een ontwikkelaar opnieuw proberen voor de API aanroept.

1. Blader naar uw APIM-exemplaar.
2. Selecteer de **API** tabblad.
3. Klik op **Demo conferentie API** uit de lijst met API.
4. Selecteer **alle bewerkingen**.
5. Selecteer boven aan het scherm **ontwerp** tabblad.
6. In de **binnenkomend verkeer** venster, klikt u op de driehoek (naast de pen).
7. Selecteer **Code-editor**.
8. Plaats de cursor in de  **<inbound>**  element.
9. In het rechtervenster onder **toegang tot softwarerestrictiebeleid**, klikt u op **+ aanroepfrequentie per sleutel**.
10. Wijzig uw **< rate-limit-by-key** code (in de  **<inbound>**  element) naar de volgende code:

        <rate-limit-by-key calls="3" renewal-period="15" counter-key="@(context.Subscription.Id)" />

## <a name="test-the-transformations"></a>De transformaties testen
        
Op dit moment uw beleid code ziet er als volgt:

    <policies>
        <inbound>
            <rate-limit-by-key calls="3" renewal-period="15" counter-key="@(context.Subscription.Id)" />
            <base />
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <set-header name="X-Powered-By" exists-action="delete" />
            <set-header name="X-AspNet-Version" exists-action="delete" />
            <find-and-replace from="://conferenceapi.azurewebsites.net" to="://apiphany.azure-api.net/conference"/>
            <base />
        </outbound>
        <on-error>
            <base />
        </on-error>
    </policies>

De rest van deze sectie beleid transformaties die u in dit artikel instelt wordt getest.

### <a name="test-the-stripped-response-headers"></a>Test de gestripte antwoordheaders

1. Blader naar uw APIM-exemplaar.
2. Selecteer de **API** tabblad.
3. Klik op **Demo conferentie API** uit de lijst met API.
4. Klik op de **GetSpeakers** bewerking.
5. Selecteer de **Test** tabblad.
6. Druk op **verzenden**.

    Zoals u dat de headers hebt verwijderd ziet:

    ![Beleidsregels](./media/transform-api/final-response1.png)

### <a name="test-the-replaced-url"></a>De URL vervangen testen

1. Blader naar uw APIM-exemplaar.
2. Selecteer de **API** tabblad.
3. Klik op **Demo conferentie API** uit de lijst met API.
4. Klik op de **GetSpeakers** bewerking.
5. Selecteer de **Test** tabblad.
6. Druk op **verzenden**.

    Zoals u ziet is de URL vervangen.

    ![Beleidsregels](./media/transform-api/final-response2.png)

### <a name="test-the-rate-limit-throttling"></a>De (beperking) frequentielimiet testen

1. Blader naar uw APIM-exemplaar.
2. Selecteer de **API** tabblad.
3. Klik op **Demo conferentie API** uit de lijst met API.
4. Klik op de **GetSpeakers** bewerking.
5. Selecteer de **Test** tabblad.
6. Druk op **verzenden** driemaal in een rij.

    Na het verzenden van de aanvraag 3 maal, krijgt u **429 te veel aanvragen** antwoord.
7. Wacht 15 seconden en druk op **verzenden** opnieuw. Deze tijd u krijgt een **200 OK** antwoord.

    ![Beperking](./media/transform-api/test-throttling.png)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een API om antwoordheaders van strook / transformeren
> * Oorspronkelijke URL's in de hoofdtekst van de API-reactie vervangen door APIM gateway-URL 's
> * Een API beveiligen door beleid voor Frequentielimiet (beperking) toe te voegen
> * De transformaties testen

Ga naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Uw API bewaken](api-management-howto-use-azure-monitor.md)
