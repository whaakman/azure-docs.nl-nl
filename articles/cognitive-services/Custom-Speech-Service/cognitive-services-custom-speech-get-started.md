---
title: Aan de slag met de Custom Speech Service op Azure | Microsoft Docs
description: Abonneer u op de Custom Speech service en de serviceactiviteiten koppelen aan een Azure-abonnement naar een model te trainen en een implementatie.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.subservice: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: 7392459f0b80558aac22bd585c0d30bf4105d76f
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55224442"
---
# <a name="get-started-with-custom-speech-service"></a>Aan de slag met Custom Speech Service

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Verken de belangrijkste functies van de Custom Speech Service en meer informatie over het bouwen, implementeren en gebruiken van akoestische en modellen voor de behoeften van uw toepassing. Uitgebreidere documentatie en stapsgewijze instructies vindt u nadat u bent aangemeld op de Custom Speech Services-portal.

## <a name="samples"></a>Voorbeelden  
Er is een goed voorbeeld waarmee we u aan de slag die u kunt vinden [hier](https://github.com/Microsoft/Cognitive-Custom-Speech-Service).

## <a name="prerequisites"></a>Vereisten  

### <a name="subscribe-to-custom-speech-service-and-get-a-subscription-key"></a>Abonneer u op de Custom Speech Service en de abonnementssleutel van een ophalen
Voordat u met de bovenstaande speelt in het voorbeeld moet u zich abonneren op Custom Speech Service en een abonnement, raadpleegt u [abonnementen](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/CustomSpeech) of voert u de uitleg [hier](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-subscribe.md). In deze zelfstudie kan zowel de primaire als de secundaire sleutel worden gebruikt. Zorg ervoor dat u de aanbevolen procedures hanteert voor het beveiligen en geheim houden van uw API-sleutel.

### <a name="get-the-client-library-and-example"></a>De clientbibliotheek en het voorbeeld downloaden
U kunt een clientbibliotheek en voorbeeld via downloaden [SDK](https://www.microsoft.com/cognitive-services/en-us/SDK-Sample?api=bing%20speech&category=sdk). Het gedownloade ZIP-bestand moet worden uitgepakt naar een map van uw keuze. Veel gebruikers kiezen hiervoor de map Visual Studio 2015.

## <a name="creating-a-custom-acoustic-model"></a>Het maken van een aangepast akoestisch model
Om het ​​akoestische model aan een bepaald domein aan te passen, is een verzameling spraakgegevens vereist. Deze verzameling bestaat uit een reeks audiobestanden met spraakgegevens en een tekstbestand met transcripties van elk audiobestand. De gegevens moet een vertegenwoordiger van het scenario waarin u wilt gebruiken de herkenning

Bijvoorbeeld: Als u spraak beter wilt herkennen in een lawaaierige fabrieksomgeving, moeten de audiobestanden bestaan ​​uit mensen die in een luidruchtige fabriek spreken.
Als u geïnteresseerd bent in de prestaties optimaliseren voor een enkele spreker, zoals u wilt alle van FDR Fireside Chats transcriberen en vervolgens de audio-bestanden moeten bestaan uit veel voorbeelden van deze spreker alleen.

U vindt een gedetailleerde beschrijving over het maken van een aangepast akoestisch model [hier](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-acoustic-model.md).

## <a name="creating-a-custom-language-model"></a>Het maken van een aangepast taalmodel
De procedure voor het maken van een aangepast taalmodel is vergelijkbaar met het maken van een akoestisch model, maar er geen audiogegevens, alleen tekst is. De tekst moet bestaan uit veel voorbeelden van query's of uitingen u verwacht dat gebruikers zeggen of hebben geregistreerde gebruikers zeggen (of door te voeren) in uw toepassing.

U vindt een gedetailleerde beschrijving over het maken van een aangepast taalmodel [hier](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-language-model.md).

## <a name="creating-a-custom-speech-to-text-endpoint"></a>Het maken van een aangepaste spraak-naar-tekst-eindpunt
Wanneer u aangepaste akoestische modellen en/of taalmodellen hebt gemaakt, kunnen ze worden geïmplementeerd in een aangepast eindpunt voor spraak-naar-tekst. Klik op 'Implementatie' in het menu 'CRI' boven aan de pagina om een nieuw aangepast eindpunt. Hiermee gaat u naar een tabel met de naam 'Implementaties' van de huidige aangepaste eindpunten. Als u geen eindpunten nog niet hebt gemaakt, is de tabel niet leeg zijn. De huidige landinstelling wordt weergegeven in de titel van de tabel. Als u een implementatie voor een andere taal maken wilt, klikt u op 'Landinstellingen wijzigen'. Meer informatie over ondersteunde talen kan worden gevonden in de sectie over het wijzigen van de landinstelling.

U vindt een gedetailleerde beschrijving over het maken van een eindpunt aangepaste speech text [hier](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-endpoint.md).

## <a name="using-a-custom-speech-endpoint"></a>Met behulp van een aangepaste spraak-eindpunt
Aanvragen worden verzonden naar een eindpunt van de spraak-naar-tekst CRI op een vergelijkbare manier als het standaardeindpunt voor Microsoft Cognitive Services spraak. Houd er rekening mee dat deze eindpunten identiek aan de Standaardeindpunten van de spraak-API zijn. Dezelfde functionaliteit die beschikbaar is via de REST-API voor de spraak-API of clientbibliotheek is dus ook de beschikbaar voor uw aangepast eindpunt.

U vindt een gedetailleerde beschrijving over het gebruik van een aangepaste spraak-naar-tekst-eindpunt [hier](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-use-endpoint.md).


Houd er rekening mee dat de eindpunten die zijn gemaakt via CRI verschillende aantallen gelijktijdige aanvragen, afhankelijk van de laag dat het abonnement is gekoppeld aan kunnen verwerken. Als meer herkenningen dan die zijn aangevraagd, retourneert deze de foutcode 429 (te veel aanvragen). Ga voor meer informatie naar de [informatie over de prijzen](https://www.microsoft.com/cognitive-services/en-us/pricing). Er is bovendien een maandelijkse quotum van aanvragen voor de gratis laag. In gevallen u toegang tot uw eindpunt in de gratis laag boven het maandelijkse quotum voor de service retourneert de foutcode 403 verboden.

De service wordt ervan uitgegaan dat audio in real time wordt verzonden. Als het sneller wordt verzonden, wordt de aanvraag beschouwd wordt uitgevoerd totdat de duur in realtime is verstreken.

* [Overzicht](cognitive-services-custom-speech-home.md)
* [Veelgestelde vragen](cognitive-services-custom-speech-faq.md)
* [Woordenlijst](cognitive-services-custom-speech-glossary.md)
