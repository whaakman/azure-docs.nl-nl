---
title: Aan de slag met de Bing Speech-API in JavaScript | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Gebruik de Bing Speech-API in Cognitive Services toepassingen te ontwikkelen die doorlopend Converteer gesproken audio naar tekst.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ms.openlocfilehash: 79f2aa9c3e73037aa25848b7d11c34a73bf35f64
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49342395"
---
# <a name="get-started-with-the-speech-recognition-api-in-javascript"></a>Aan de slag met de spraak-API in JavaScript

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

U kunt ontwikkelen van toepassingen die Converteer gesproken audio naar tekst met behulp van de spraak-API. Maakt gebruik van de JavaScript-clientbibliotheek de [Speech Service WebSocket-protocol](../API-Reference-REST/websocketprotocol.md), waarmee u te communiceren en te ontvangen tekst tegelijkertijd worden omgezet. Dit artikel helpt u aan de slag met de spraak-API in JavaScript.

## <a name="prerequisites"></a>Vereisten

### <a name="subscribe-to-the-speech-recognition-api-and-get-a-free-trial-subscription-key"></a>Abonneer u op de spraak-API en een gratis proefabonnement-sleutel ophalen

De spraak-API maakt deel uit van Cognitive Services. U krijgt een gratis proefabonnement sleutels uit de [Cognitive Services-abonnement](https://azure.microsoft.com/try/cognitive-services/) pagina. Nadat u de spraak-API selecteert, selecteert u **API-sleutel ophalen** om op te halen van de sleutel. Het resultaat een primaire en secundaire sleutel. Beide sleutels zijn gekoppeld aan dezelfde quota, zodat u beide sleutels kunt gebruiken.

> [!IMPORTANT]
> De abonnementssleutel van een ophalen. Voordat u de spraak-clientbibliotheken gebruiken kunt, moet u beschikken over een [abonnementssleutel](https://azure.microsoft.com/try/cognitive-services/).

## <a name="get-started"></a>Aan de slag

In deze sectie begeleidt we u stapsgewijs door de stappen voor het laden van een voorbeeld-HTML-pagina. Het voorbeeld bevindt zich onze [github-opslagplaats](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript). U kunt **open het voorbeeld rechtstreeks** vanuit de opslagplaats of **open het voorbeeld van een lokale kopie** van de opslagplaats. 

> [!NOTE]
> Sommige browsers blokkeren toegang tot de microfoon van de oorsprong niet beveiligd. Dus het verdient aanbeveling voor het hosten van de 'voorbeeld' / 'uw app' op https zodat deze werkt op alle ondersteunde browsers. 

### <a name="open-the-sample-directly"></a>Open het voorbeeld rechtstreeks

Een abonnementssleutel verkrijgen, zoals hierboven beschreven. Open vervolgens de [koppeling naar het voorbeeld](https://htmlpreview.github.io/? https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript/blob/preview/samples/browser/Sample.html). Hiermee wordt de pagina laden in de standaardbrowser (weergegeven met behulp van [htmlPreview](https://github.com/htmlpreview/htmlpreview.github.com)).

### <a name="open-the-sample-from-a-local-copy"></a>Open het voorbeeld van een lokale kopie

Als u wilt proberen het voorbeeld lokaal, deze opslagplaats te klonen:

```
git clone https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript
```

de TypeScript-bronnen worden gecompileerd en bundelt u deze in één JavaScript-bestand ([npm](https://www.npmjs.com/) moet worden geïnstalleerd op uw computer). Wijzig in de hoofdmap van de gekloonde opslagplaats en voer de opdrachten uit:

```
cd SpeechToText-WebSockets-Javascript && npm run bundle
```

Open `samples\browser\Sample.html` in uw browser.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het opnemen van de SDK in uw eigen webpagina [hier](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript).

## <a name="remarks"></a>Opmerkingen

- De spraak-API biedt ondersteuning voor drie [erkenning modi](../concepts.md#recognition-modes). U kunt de modus schakelen door het bijwerken van de **Setup()** functie vinden in het bestand Sample.html. Het voorbeeld wordt de modus ingesteld op `Interactive` standaard. Als u wilt wijzigen in de modus, bijwerken met de parameter `SR.RecognitionMode.Interactive` naar een andere modus. Wijzig bijvoorbeeld de parameter `SR.RecognitionMode.Conversation`.
- Zie voor een volledige lijst van ondersteunde talen, [ondersteunde talen](../API-Reference-REST/supportedlanguages.md).

## <a name="related-topics"></a>Verwante onderwerpen

- [De voorbeeldopslagplaats JavaScript spraak Recognition-API](https://github.com/Azure-Samples/SpeechToText-WebSockets-Javascript)
- [Aan de slag met de REST-API](GetStartedREST.md)
