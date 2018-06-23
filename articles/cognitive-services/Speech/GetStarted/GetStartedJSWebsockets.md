---
title: Aan de slag met de Microsoft Speech Recognition API in JavaScript | Microsoft Docs
description: De Microsoft Speech-API opname in cognitieve Services gebruiken voor het ontwikkelen van toepassingen die continu gesproken audio naar tekst wilt converteren.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 12/21/2017
ms.author: zhouwang
ms.openlocfilehash: 56c41fd7f6a00d80bc6bccd61894654e057e926e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344676"
---
# <a name="get-started-with-the-speech-recognition-api-in-javascript"></a>Aan de slag met de erkenning Speech-API in JavaScript

U kunt toepassingen ontwikkelen die gesproken audio naar tekst wilt converteren met behulp van de erkenning Speech-API. De JavaScript-clientbibliotheek gebruikt de [spraak Service WebSocket-protocol](../API-Reference-REST/websocketprotocol.md), waarmee u om te communiceren en ontvangen tekst tegelijkertijd worden omgezet. In dit artikel helpt u bij aan de slag met de erkenning Speech-API in JavaScript.

## <a name="prerequisites"></a>Vereisten

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Abonneer u op de erkenning Speech-API en de sleutel voor een gratis proefabonnement ophalen

De Speech-API maakt deel uit van cognitieve Services. Krijgt u gratis proefabonnement sleutels van de [abonnement cognitieve Services](https://azure.microsoft.com/try/cognitive-services/) pagina. Nadat u de Speech-API selecteert, selecteert u **API-sleutel ophalen** ophalen van de sleutel. Het resultaat een primaire en secundaire sleutel. Beide sleutels zijn gekoppeld aan hetzelfde contingent, zodat u van sleutel kunt gebruiken.

> [!IMPORTANT]
> De abonnementssleutel voor een ophalen. Voordat u spraak clientbibliotheken gebruiken kunt, moet u beschikken over een [abonnementssleutel](https://azure.microsoft.com/try/cognitive-services/).

## <a name="get-started"></a>Aan de slag

In deze sectie begeleidt we u stapsgewijs door de benodigde stappen om een voorbeeld HTML-pagina te laden. Het voorbeeld bevindt zich onze [github-opslagplaats](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript). U kunt **rechtstreeks openen van het voorbeeld** vanuit de opslagplaats of **open het voorbeeld van een lokale kopie** van de opslagplaats. 

> [!NOTE]
> Sommige browsers blokkeren microfoontoegang van de oorsprong niet beveiligd. Daarom, wordt het aanbevolen voor het hosten van de 'voorbeeld' / 'uw app' op https downloaden werkt op alle ondersteunde browsers. 

### <a name="open-the-sample-directly"></a>Het voorbeeld rechtstreeks openen

De abonnementssleutel van een aan te schaffen zoals hierboven is beschreven. Open vervolgens de [koppeling met het voorbeeld](https://htmlpreview.github.io/?https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript/blob/preview/samples/browser/Sample.html). Hiermee wordt de pagina in de standaardbrowser geladen (gegenereerd door middel van [htmlPreview](https://github.com/htmlpreview/htmlpreview.github.com)).

### <a name="open-the-sample-from-a-local-copy"></a>Open het voorbeeld van een lokale kopie

Om te proberen het voorbeeld lokaal, moet u deze opslagplaats klonen:

```
git clone https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript
```

de machineschrift bronnen en de bundel/browserfy compileren bij één JavaScript-bestand ([npm](https://www.npmjs.com/) moet worden geïnstalleerd op uw computer). Wijzigen in de hoofdmap van de gekloonde opslagplaats en voer de opdrachten:

```
cd SpeechToText-WebSockets-Javascript && npm run bundle
```

Open `samples\browser\Sample.html` in uw favoriete browser.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het opnemen van de SDK in uw eigen webpagina [hier](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript).

## <a name="remarks"></a>Opmerkingen

- De erkenning Speech-API ondersteunt drie [erkenning modi](../concepts.md#recognition-modes). U kunt de modus schakelen door het bijwerken van de **Setup()** , in het bestand Sample.html gevonden. Het voorbeeld wordt de modus instellen op `Interactive` standaard. Als u wilt wijzigen in de modus, werk de parameter `SR.RecognitionMode.Interactive` op een andere modus. Wijzig bijvoorbeeld de parameter voor `SR.RecognitionMode.Conversation`.
- Zie voor een volledige lijst van ondersteunde talen [ondersteunde talen](../API-Reference-REST/supportedlanguages.md).

## <a name="related-topics"></a>Verwante onderwerpen

- [Herkenning van JavaScript API-voorbeeld opslagplaats](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript)
- [Aan de slag met de REST-API](GetStartedREST.md)
