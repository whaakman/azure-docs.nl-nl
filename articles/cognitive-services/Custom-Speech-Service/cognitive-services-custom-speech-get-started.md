---
title: Aan de slag met de aangepaste spraak-Service op Azure | Microsoft Docs
description: Abonneren op de aangepaste spraak-service en de serviceactiviteiten koppelen aan een Azure-abonnement voor een model te trainen en een implementatie.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: fe7a140f5ba2d712014f03663a88d516958d188e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344528"
---
# <a name="get-started-with-custom-speech-service"></a>Aan de slag met aangepaste spraak-Service

Verken de belangrijkste functies van de aangepaste spraak-Service en informatie over het bouwen en acoustic- en taalinstellingen modellen gebruiken voor de behoeften van uw toepassing. Uitgebreidere documentatie en stapsgewijze instructies vindt u nadat u zich aanmeldt op de aangepaste spraakservices-portal.

## <a name="samples"></a>Voorbeelden  
Er is een nice voorbeeldtoepassing die wij u aan de gaan die u kunt vinden [hier](https://github.com/Microsoft/Cognitive-Custom-Speech-Service).

## <a name="prerequisites"></a>Vereisten  

### <a name="subscribe-to-custom-speech-service-and-get-a-subscription-key"></a>Abonneer u op aangepaste spraak Service en de abonnementssleutel voor een ophalen
Voordat u met de bovenstaande speelt in het voorbeeld moet u zich abonneert op aangepaste spraak Service en een abonnement, raadpleegt u [abonnementen](https://portal.azure.com/#create/Microsoft.CognitiveServices/apitype/CustomSpeech) of voert u de uitleg [hier](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-subscribe.md). Zowel de primaire en secundaire sleutel kan worden gebruikt in deze zelfstudie. Zorg ervoor dat u Volg de aanbevolen procedures voor het bewaren van uw API-sleutel geheim en beveiligen.

### <a name="get-the-client-library-and-example"></a>Ophalen van de client-bibliotheek en voorbeeld
U kunt een client-bibliotheek en voorbeeld via downloaden [SDK](https://www.microsoft.com/cognitive-services/en-us/SDK-Sample?api=bing%20speech&category=sdk). Het gedownloade zip-bestand moet worden uitgepakt naar een map van uw keuze, veel gebruikers selecteert u de map Visual Studio 2015.

## <a name="creating-a-custom-acoustic-model"></a>Maken van een aangepaste akoestisch model
Voor het aanpassen van het akoestisch model voor een bepaald domein, is een verzameling spraakgegevens vereist. Deze verzameling bestaat uit een reeks audio-bestanden van spraakgegevens en een tekstbestand met transcriptie van elk bestand audio. De audiogegevens moet vertegenwoordiger van het scenario waarin u wilt de herkenner gebruiken

Bijvoorbeeld: als u spraak in een omgeving met veel ruis veroorzaken factory beter te herkennen wilt, de audio-bestanden van mensen in een veel ruis veroorzaken factory spreken moeten bestaan.
Als u geïnteresseerd bent in het optimaliseren van de prestaties voor een enkele spreker, zoals u wilt alle FDR van Fireside Chats transcriberen vervolgens de audio-bestanden van veel voorbeelden van alleen die spreker moeten bestaan.

U vindt een gedetailleerde beschrijving voor het maken van een aangepaste akoestisch model [hier](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-acoustic-model.md).

## <a name="creating-a-custom-language-model"></a>Maken van een aangepaste taalmodel
De procedure voor het maken van een taalmodel aangepaste is vergelijkbaar met het maken van een akoestisch model, maar er geen audio-gegevens, alleen tekst zijn. De tekst moet bestaan uit veel voorbeelden van query's of utterances u verwacht dat gebruikers in te spreken of zich aangemeld hebben gebruikers zeggen (of typen) in uw toepassing.

U vindt een gedetailleerde beschrijving voor het maken van een aangepaste taalmodel [hier](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-language-model.md).

## <a name="creating-a-custom-speech-to-text-endpoint"></a>Een aangepaste spraak naar tekst-eindpunt maken
Wanneer u aangepaste akoestisch modellen en/of modellen taal hebt gemaakt, kunnen ze worden geïmplementeerd in een aangepaste spraak naar tekst-eindpunt. Klik op 'Implementaties' in het menu 'Cri's ' boven aan de pagina voor het maken van een nieuwe aangepaste eindpunt. Hiermee gaat u naar een tabel met de naam 'Implementaties' van de huidige aangepaste eindpunten. Als u nog geen eindpunten hebt gemaakt, wordt de tabel niet leeg zijn. De huidige landinstelling wordt doorgevoerd in de titel van de tabel. Als u maken van een implementatie voor een andere taal wilt, klik op 'Wijziging landinstelling'. Meer informatie over ondersteunde talen vindt u in de sectie over het wijzigen van de landinstelling.

U vindt een gedetailleerde beschrijving voor het maken van een aangepaste spraak tekst eindpunt [hier](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-endpoint.md).

## <a name="using-a-custom-speech-endpoint"></a>Met behulp van een aangepaste spraak-eindpunt
Aanvragen kunnen worden verzonden naar een eindpunt van de spraak naar tekst cri's op een zeer vergelijkbare manier als het standaardeindpunt van Microsoft cognitieve Services spraak. Houd er rekening mee dat deze eindpunten identiek aan de Standaardeindpunten van de Speech-API zijn. Dus is dezelfde functionaliteit die beschikbaar is via de REST-API voor de Speech-API of clientbibliotheek ook de beschikbaar voor uw aangepaste eindpunt.

U vindt een gedetailleerde beschrijving voor het gebruik van een aangepaste spraak naar tekst eindpunt [hier](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-use-endpoint.md).


Houd er rekening mee dat de eindpunten is gemaakt via cri's verschillend aantal gelijktijdige aanvragen, afhankelijk van de laag dat het abonnement is gekoppeld aan kunnen verwerken. Als er meer erkenning dan die zijn aangevraagd, wordt deze de foutcode 429 retourneren (te veel aanvragen). Voor meer informatie raadpleegt u de [prijsgegevens](https://www.microsoft.com/cognitive-services/en-us/pricing). Daarnaast is er een maandelijks quotum van aanvragen voor de laag gratis. In gevallen dat u toegang tot uw eindpunt in de laag gratis boven uw maandelijkse quotum van de service retourneert de foutcode 403 verboden.

De service wordt ervan uitgegaan audio in realtime worden verzonden. Als het sneller wordt verzonden, wordt de aanvraag beschouwd uitgevoerd totdat de duur in realtime is verstreken.

* [Overzicht](cognitive-services-custom-speech-home.md)
* [Veelgestelde vragen](cognitive-services-custom-speech-faq.md)
* [Woordenlijst](cognitive-services-custom-speech-glossary.md)
