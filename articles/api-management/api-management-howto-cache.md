---
title: Opslaan in cache toevoegen om de prestaties in Azure API Management te verbeteren | Microsoft Docs
description: Informatie over het verbeteren van de latentie, het bandbreedteverbruik en de webservicewerklast voor API Management-serviceaanroepen.
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 740f6a27-8323-474d-ade2-828ae0c75e7a
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 7458ad6e0a864d742f74ce743ce3179594113c00
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="add-caching-to-improve-performance-in-azure-api-management"></a>Opslaan in cache toevoegen om de prestaties in Azure API Management te verbeteren
Bewerkingen in API Management kunnen worden geconfigureerd voor het opslaan van antwoorden in de cache. Door het opslaan van antwoorden in de cache kan de API-latentie, het bandbreedteverbruik en de webservicewerklast aanzienlijk worden verminderd voor gegevens die niet vaak wijzigen.
 
Zie [De cachebeleidsregels van API Management](api-management-caching-policies.md) en [Aangepaste opslaan in de cache in Azure API Management](api-management-sample-cache-by-key.md) voor meer informatie over opslaan in de cache.

![cachebeleid](media/api-management-howto-cache/cache-policies.png)

Wat u leert:

> [!div class="checklist"]
> * Antwoorden in cache opslaan toevoegen aan de API
> * Verificatie van opslaan in de cache in actie

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

+ [Een Azure API Management-exemplaar maken](get-started-create-service-instance.md)
+ [Import and publish an API](import-and-publish.md) (API's importeren en publiceren)

## <a name="caching-policies"> </a>De cachebeleidsregels toevoegen

Met de cachebeleidsregels in dit voorbeeld wordt met de eerste aanvraag voor de bewerking **GetSpeakers** een antwoord geretourneerd van de back-endservice. Dit antwoord wordt in de cache opgeslagen en voorzien van een sleutel door de opgegeven headers en querytekenreeksparameters. Voor volgende aanroepen voor de bewerking, met overeenkomende parameters, wordt het antwoord geretourneerd dat in de cache is opgeslagen, tot het cacheduurinterval is verlopen.

1. Meld u aan bij Azure Portal op [https://portal.azure.com](https://portal.azure.com).
2. Blader naar de APIM-instantie.
3. Selecteer het tabblad **API**.
4. Klik in de API-lijst op **Demo Conference API**.
5. Selecteer **GetSpeakers**.
6. Selecteer boven in het scherm het tabblad **Ontwerp**.
7. Klik in het venster **Binnenkomende verwerking** op het driehoekje (naast het potlood).

    ![code-editor](media/api-management-howto-cache/code-editor.png)
8. Selecteer **Code-editor**.
9. Voeg aan het **inkomende** element de volgende beleidsregel toe:

        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
            <vary-by-header>Accept</vary-by-header>
            <vary-by-header>Accept-Charset</vary-by-header>
            <vary-by-header>Authorization</vary-by-header>
        </cache-lookup>

10. Voeg aan het **uitgaande** element de volgende beleidsregel toe:

        <cache-store caching-mode="cache-on" duration="20" />

    Met **Duur** wordt het vervalinterval opgegeven van de antwoorden in de cache. In dit voorbeeld bedraagt het interval **20** seconden.

## <a name="test-operation"> </a>Een bewerking aanroepen en het opslaan in de cache testen
Als u opslaan in de cache in actie wilt zien, roept u de bewerking aan vanuit de ontwikkelaarsportal.

1. Blader in Azure Portal naar de APIM-instantie.
2. Selecteer het tabblad **API's**.
3. Selecteer de API waaraan u cachebeleidsregels wilt toevoegen.
4. Selecteer de bewerking **GetSpeakers**.
5. Klik rechtsboven in het menu op het tabblad **Test**.
6. Druk op **Verzenden**.

## <a name="next-steps"> </a>Volgende stappen
* Zie [Cachebeleidsregels][Caching policies] in [Naslaginformatie over beleid voor API Management][API Management policy reference] voor meer informatie over cachebeleidsregels.
* Zie [Aangepast opslaan in cache in Azure API Management](api-management-sample-cache-by-key.md) voor informatie over het opslaan van items in de cache per sleutel met behulp van beleidsexpressies.

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Get started with Azure API Management]: get-started-create-service-instance.md

[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Create an API Management service instance]: get-started-create-service-instance.md

[Configure an operation for caching]: #configure-caching
[Review the caching policies]: #caching-policies
[Call an operation and test the caching]: #test-operation
[Next steps]: #next-steps
