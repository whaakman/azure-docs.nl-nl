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
ms.date: 01/03/2019
ms.author: diberry
ms.openlocfilehash: 75732a3ea78a89c06063102d37b77f315a3bcf62
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "54001801"
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

De gele nummers in de volgende afbeelding geven aan welk naamwaardepaar moet worden gebruikt in de volgende stappen.

[![Resultaten publiceren](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png)](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png#lightbox)

Voor het genereren van een antwoord met Postman voert u de volgende stappen uit:

1. Open Postman. Als u wordt gevraagd een bouwsteen te kiezen, selecteert u de bouwsteen **Algemene aanvraag**. Stel de **Aanvraagnaam** in als `Generate QnA Maker answer` en stel de **verzameling** in als `Generate QnA Maker answers`. Als u niet wilt opslaan naar een verzameling, selecteert u de knop **Annuleren**.
1. Selecteer in de werkruimte de HTTP-methode van **POST**.

    [![Stel in Postman POST-methode in](../media/qnamaker-quickstart-get-answer-with-postman/postman-select-post-method.png)](../media/qnamaker-quickstart-get-answer-with-postman/postman-select-post-method.png#lightbox)

1. Voor de URL moet u de HOST-waarde (nummer 2 in de afbeelding) en de Post-waarde (nummer 1 in de afbeelding) samenvoegen om de volledige URL te maken. Een volledige voorbeeld-URL ziet er als volgt uit: 

    `https://qnamaker-f0.azurewebsites.net/qnamaker/knowledgebases/e1115f8c-d01b-4698-a2ed-85b0dbf3348c/generateAnswer`

    [![Stel in Postman de volledige URL in](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png#lightbox)

1. Selecteer het tabblad **Headers** onder de URL en selecteer vervolgens **Bulksgewijs bewerken**. 

1. Kopieer de headers (nummer 3 en 4 in de afbeelding) in het tekstvak.

    [![Stel in Postman de headers in](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png#lightbox)

1. Selecteer het tabblad **Hoofdtekst**.
1. Selecteer de **onbewerkte** opmaak en voer de JSON (nummer 5 in de afbeelding) in die de vraag weergeeft.

    `{"question":"How do I programmatically update my Knowledge Base?"}`

    [![Stel in Postman de JSON-waarde van de hoofdtekst in](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png#lightbox)

1. Selecteer de knop **Verzenden**.
1. De reactie bevat het antwoord, samen met andere informatie die mogelijk belangrijk is voor de clienttoepassing. 

    [![Stel in Postman de JSON-waarde van de hoofdtekst in](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png)](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png#lightbox)

## <a name="use-staging-endpoint"></a>Faseringseindpunt gebruiken

Als u een antwoord van het faseringseindpunt wilt krijgen, voegt u de URL toe met de booleaanse parameter `isTest` van de queryreeks met de waarde van `true`.

`?isTest=true`

## <a name="next-steps"></a>Volgende stappen

De pagina Publiceren bevat ook informatie voor [het genereren van een antwoord](get-answer-from-kb-using-curl.md) met cURL. 

> [!div class="nextstepaction"]
> [Metagegevens gebruiken tijdens het genereren van een antwoord](../How-to/metadata-generateanswer-usage.md)