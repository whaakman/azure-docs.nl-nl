---
title: Swagger-documentatie-Speech-Service
titleSuffix: Azure Cognitive Services
description: De Swagger-documentatie kan worden gebruikt om automatisch Sdk's te genereren voor een aantal programmeer talen. Alle bewerkingen in onze service worden ondersteund door Swagger
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: reference
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 232435a424d2461bce4598356a986473cb1d3644
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68552568"
---
# <a name="swagger-documentation"></a>Documentatie voor Swagger

De spraak services bieden een Swagger-specificatie om te communiceren met een aantal REST-Api's die worden gebruikt voor het importeren van gegevens, het maken van modellen, het testen van de model nauwkeurigheid, het maken van aangepaste eind punten, het in de wachtrij plaatsen van batch transcripties en het beheren van abonnementen. De meeste bewerkingen die beschikbaar zijn via de Custom Speech Portal, kunnen via een programma worden voltooid met behulp van deze Api's.

> [!NOTE]
> Bewerkingen voor spraak naar tekst en tekst naar spraak worden ondersteund als REST Api's, die op zijn beurt worden beschreven in de Swagger-specificatie.

## <a name="generating-code-from-the-swagger-specification"></a>Code genereren op basis van de Swagger-specificatie

De [Swagger-specificatie](https://cris.ai/swagger/ui/index) bevat opties waarmee u snel kunt testen op verschillende paden. Soms is het echter wenselijk om code te genereren voor alle paden, waarbij u één bibliotheek met aanroepen maakt waarmee u toekomstige oplossingen op kunt baseren. Laten we eens kijken naar het proces voor het genereren van een python-bibliotheek.

U moet Swagger instellen op dezelfde regio als uw spraak service-abonnement. U kunt uw regio bevestigen in het Azure Portal onder uw speech Services-resource. Zie [regio's](regions.md)voor een volledige lijst met ondersteunde regio's.

1. Ga naar https://editor.swagger.io
2. Klik op **bestand**en vervolgens op **importeren** .
3. Voer de URL voor Swagger in, inclusief de regio voor uw speech Services-abonnement`https://<your-region>.cris.ai/docs/v2.0/swagger`
4. Klik op **client genereren** en selecteer python
5. Sla de client bibliotheek op

U kunt de python-bibliotheek gebruiken die u hebt gegenereerd met de [voor beelden van speech Services op github](https://aka.ms/csspeech/samples).

## <a name="reference-docs"></a>Referentiedocumenten

* [REST (Swagger): Batch-transcriptie en-aanpassing](https://westus.cris.ai/swagger/ui/index)
* [REST API: Spraak naar tekst](rest-speech-to-text.md)
* [REST API: Tekst-naar-spraak](rest-text-to-speech.md)

## <a name="next-steps"></a>Volgende stappen

* [Voor beelden van speech Services op github](https://aka.ms/csspeech/samples).
* [Gratis een abonnements sleutel voor spraak Services aanschaffen](get-started.md)
