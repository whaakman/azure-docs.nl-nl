---
title: 'Snelstart: Tekstinhoud op ongewenst materiaal analyseren in Python'
titlesuffix: Azure Cognitive Services
description: Tekstinhoud analyseren op diverse soorten ongewenst materiaal met behulp van de Content Moderator SDK voor Python
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: quickstart
ms.date: 11/19/2018
ms.author: pafarley
ms.openlocfilehash: acafd666b2d3791450499ebdf173ffb67ad2c932
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53095184"
---
# <a name="quickstart-analyze-text-content-for-objectionable-material-in-python"></a>Snelstart: Tekstinhoud op ongewenst materiaal analyseren in Python

In dit artikel vindt u informatie en codevoorbeelden om aan de slag te gaan met de Content Moderator SDK voor Python. U leert hoe u kunt filteren op termen en tekstinhoud classificeren met het doel toezicht te houden op mogelijk ongewenst materiaal.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="prerequisites"></a>Vereisten
- Een abonnementssleutel voor Content Moderator. Volg de instructies in [Een Cognitive Services-account maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) om u te abonneren op Content Moderator en uw sleutel op te halen.
- [Python 2.7+ of 3.5+](https://www.python.org/downloads/)
- [pip](https://pip.pypa.io/en/stable/installing/)-hulpprogramma

## <a name="get-the-content-moderator-sdk"></a>De Content Moderator SDK ophalen

Installeer de Python-SDK voor Content Moderator door de opdrachtprompt te openen en de volgende opdracht uit te voeren:

```
pip install azure-cognitiveservices-vision-contentmoderator
```

## <a name="import-modules"></a>Modules importeren

Maak een nieuw Python-script met de naam _ContentModeratorQS.py_ en voeg de volgende code toe voor het importeren van de benodigde onderdelen van de SDK.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/text-moderation-quickstart-python.py?range=1-10)]

Importeer ook de functie 'vrij afdrukken' voor het afhandelen van de uiteindelijke uitvoer.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/text-moderation-quickstart-python.py?range=12)]


## <a name="initialize-variables"></a>Variabelen initialiseren

Vervolgens voegt u variabelen toe voor uw abonnementssleutel en de eindpunt-URL voor Content Moderator. U dient `<your subscription key>` te vervangen door de waarde van de sleutel. Mogelijk dient u ook de waarde van `endpoint_url` te wijzigen, zodat u de regio-id kunt gebruiken die overeenkomt met uw abonnementssleutel. Abonnementssleutels voor een gratis proefversie worden gegenereerd in de regio **westus**.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/text-moderation-quickstart-python.py?range=14-16)]


Omwille van de eenvoud analyseert u tekst rechtstreeks vanuit het script. Definieer een nieuwe reeks tekstinhoud voor het beheer:

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/text-moderation-quickstart-python.py?range=18-21)]


## <a name="query-the-moderator-service"></a>Query uitvoeren op de service Moderator

Maak een **ContentModeratorClient**-instantie met behulp van uw abonnementssleutel en de eindpunt-URL. Vervolgens gebruikt u het deelexemplaar **TextModerationOperations** ervan voor het aanroepen van de beheer-API. Zie de naslagdocumentatie **[screen_text](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-contentmoderator/azure.cognitiveservices.vision.contentmoderator.operations.textmoderationoperations?view=azure-python#screen-text)** voor meer informatie over het aanroepen.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/text-moderation-quickstart-python.py?range=23-36)]

## <a name="print-the-response"></a>Het antwoord weergeven

Controleer ten slotte of de aanroep is voltooid en een instantie **Scherm** heeft geretourneerd. Druk daarna de naar de console geretourneerde gegevens af.

[!code-python[](~/cognitive-services-content-moderator-samples/documentation-samples/python/text-moderation-quickstart-python.py?range=38-39)]


De voorbeeldtekst die wordt gebruikt in deze snelstart resulteert in de volgende uitvoer:

```console
{'auto_corrected_text': '" Is this a garbage email abide@ abed. com, phone: '
                        '6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, '
                        'Redmond, WA 98052. Crap is the profanity here. Is '
                        'this information PII? phone 3144444444\\ n"',
 'classification': {'category1': {'score': 0.00025233393535017967},
                    'category2': {'score': 0.18468093872070312},
                    'category3': {'score': 0.9879999756813049},
                    'review_recommended': True},
 'language': 'eng',
 'normalized_text': '" Is this a garbage email abide@ abed. com, phone: '
                    '6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, '
                    'Redmond, WA 98052. Crap is the profanity here. Is this '
                    'information PII? phone 3144444444\\ n"',
 'original_text': '"Is this a grabage email abcdef@abcd.com, phone: '
                  '6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, '
                  'WA 98052. Crap is the profanity here. Is this information '
                  'PII? phone 3144444444\\n"',
 'pii': {'address': [{'index': 82,
                      'text': '1 Microsoft Way, Redmond, WA 98052'}],
         'email': [{'detected': 'abcdef@abcd.com',
                    'index': 25,
                    'sub_type': 'Regular',
                    'text': 'abcdef@abcd.com'}],
         'ipa': [{'index': 65, 'sub_type': 'IPV4', 'text': '255.255.255.255'}],
         'phone': [{'country_code': 'US', 'index': 49, 'text': '6657789887'},
                   {'country_code': 'US', 'index': 177, 'text': '3144444444'}]},
 'status': {'code': 3000, 'description': 'OK'},
 'terms': [{'index': 118, 'list_id': 0, 'original_index': 118, 'term': 'crap'}],
 'tracking_id': 'b253515c-e713-4316-a016-8397662a3f1a'}
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een eenvoudig Python-script ontwikkeld dat de Content Moderator-service gebruikt om relevante informatie over een bepaald tekstvoorbeeld te retourneren. Nu leert u meer over wat de verschillende vlaggen en classificaties betekenen, zodat u kunt beslissen welke gegevens u nodig hebt en hoe uw app ermee om moet gaan.

> [!div class="nextstepaction"]
> [Teksttoezichthandleiding](text-moderation-api.md)