---
title: 'Snelstart: Antwoord uit knowledge base ophalen via Postman - QnA Maker'
titlesuffix: Azure Cognitive Services
description: Deze snelstart begeleidt u bij het ophalen van een antwoord uit een knowledge base via Postman.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 12/11/2018
ms.author: diberry
ms.openlocfilehash: 476e982bdddd41c1daf06c3a673d964ce2a85f98
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53270889"
---
# <a name="quickstart-get-an-answer-from-knowledge-base-using-postman"></a>Snelstart: Een antwoord ophalen uit knowledge base via Postman

Deze snelstart op basis van Postman begeleidt u bij het ophalen van een antwoord uit een knowledge base.

## <a name="prerequisites"></a>Vereisten

* Meest recente [**Postman**](https://www.getpostman.com/).
* U moet over een [QnA Maker-service](../How-To/set-up-qnamaker-service-azure.md) en een [knowledge base met vragen en antwoorden](../Tutorials/create-publish-query-in-portal.md) beschikken. 

## <a name="publish-to-get-endpoint"></a>Publiceren om eindpunten te verkrijgen

Wanneer u klaar bent voor het genereren van een antwoord op een vraag uit uw knowledge base, [publiceert](../How-to/publish-knowledge-base.md) u uw knowledge base.

## <a name="use-production-endpoint-with-postman"></a>Productie-eindpunt gebruiken met Postman

Wanneer uw knowledge base is gepubliceerd, geeft de pagina **Publiceren** de instellingen van de HTTP-aanvraag weer voor het genereren van een antwoord. De standaardweergave toont de instellingen die vereist zijn voor het genereren van een antwoord uit [Postman](https://www.getpostman.com).

[![Resultaten publiceren](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png)](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png#lightbox)

Voor het genereren van een antwoord met Postman voert u de volgende stappen uit:

1. Open Postman. 
1. Selecteer de bouwsteen voor het maken van een algemene aanvraag.
1. Stel de **Aanvraagnaam** in als `Generate QnA Maker answer` en stel de **verzameling** in als `Generate QnA Maker answers`.
1. Selecteer in de werkruimte de HTTP-methode van **POST**.
1. Voor de URL moet u de HOST-waarde en de Post-waarde samenvoegen om de volledige URL te maken. 

    [![Stel in Postman de methode in op Post en de volledige URL](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png#lightbox)

1. Selecteer het tabblad **Headers** onder de URL en selecteer vervolgens **Bulksgewijs bewerken**. 
1. Kopieer de headers naar het tekstvak.

    [![Stel in Postman de headers in](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png#lightbox)

1. Selecteer het tabblad **Hoofdtekst**.
1. Selecteer de **onbewerkte** opmaak en voer de JSON in die de vraag weergeeft.

    [![Stel in Postman de JSON-waarde van de hoofdtekst in](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png#lightbox)

1. Selecteer de knop **Verzenden**.
1. De reactie bevat het antwoord, samen met andere informatie die mogelijk belangrijk is voor de clienttoepassing. 

    [![Stel in Postman de JSON-waarde van de hoofdtekst in](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png)](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png#lightbox)

## <a name="use-staging-endpoint-with-curl"></a>Faseringseindpunt gebruiken met cURL

Als u een antwoord van het faseringseindpunt wilt krijgen, gebruikt u de booleaanse parameter `isTest` van de queryreeks met de waarde van `true`.

`isTest=true`

## <a name="next-steps"></a>Volgende stappen

De pagina Publiceren bevat ook informatie voor [het genereren van een antwoord](get-answer-from-kb-using-curl.md) met cURL. 

> [!div class="nextstepaction"]
> [Metagegevens gebruiken tijdens het genereren van een antwoord](../How-to/metadata-generateanswer-usage.md)