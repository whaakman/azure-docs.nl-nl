---
title: Swagger-documentatie - spraakservices
titleSuffix: Azure Cognitive Services
description: De Swagger-documentatie kan worden gebruikt voor het automatisch genereren van SDK's voor een aantal programmeertalen. Alle bewerkingen in onze service worden ondersteund door de Swagger
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
ms.date: 04/12/2019
ms.author: erhopf
ms.openlocfilehash: 6cf3ab6480900aa763598120e6ff7e308f5044e1
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743205"
---
# <a name="swagger-documentation"></a>Documentatie voor Swagger

De spraakservices bieden een Swagger-specificatie om te communiceren met een handvol REST-API's gebruikt voor het importeren van gegevens, modellen maken, de nauwkeurigheid van model testen, aangepaste eindpunten maken, in de wachtrij plaatsen batch transcripties en abonnementen beheren. De meeste bewerkingen weergegeven die beschikbaar zijn via de portal voor aangepaste spraak kunnen worden uitgevoerd via een programma met behulp van deze API's. 

> [!NOTE]
> Zowel spraak naar tekst en spraak bewerkingen worden ondersteund als REST-API's, die op zijn beurt worden beschreven in de Swagger-specificatie beschikbaar zijn.

## <a name="generating-code-from-the-swagger-specification"></a>Genereren van code van de Swagger-specificatie

De [Swagger-specificatie](https://cris.ai/swagger/ui/index) bevat opties waarmee u kunt snel testen van de verschillende paden. Maar is soms het wenselijk zijn voor het genereren van code voor alle paden, het maken van een één-bibliotheek van aanroepen waarop u oplossingen kunt baseren. We gaan een overzicht van het proces voor het genereren van een Python-bibliotheek.

U moet Swagger ingesteld op dezelfde regio als uw Speech Service-abonnement. U kunt uw regio in de Azure-portal onder uw resource Speech Services controleren. Zie voor een volledige lijst met ondersteunde regio's, [regio's](regions.md).

1. Ga naar https://editor.swagger.io
2. Klik op **bestand**, klikt u vervolgens op **importeren**
3. Voer de swagger-URL met inbegrip van de regio voor uw abonnement spraakservices `https://<your-region>.cris.ai/docs/v2.0/swagger`
4. Klik op **Client genereren** en selecteer Python
5. Opslaan van de clientbibliotheek

U kunt de Python-bibliotheek die u hebt gegenereerd met de [Speech Services-voorbeelden op GitHub](https://aka.ms/csspeech/samples).

## <a name="reference-docs"></a>Referentiedocumenten

* [REST (Swagger): Batch transcriptie en aanpassen](https://westus.cris.ai/swagger/ui/index)
* [REST API: Spraak-naar-tekst](rest-speech-to-text.md)
* [REST API: Text-to-speech](rest-text-to-speech.md)

## <a name="next-steps"></a>Volgende stappen

* [Speech Services-voorbeelden op GitHub](https://aka.ms/csspeech/samples).
* [Ontvangt u een abonnementssleutel Speech Services gratis](get-started.md)
