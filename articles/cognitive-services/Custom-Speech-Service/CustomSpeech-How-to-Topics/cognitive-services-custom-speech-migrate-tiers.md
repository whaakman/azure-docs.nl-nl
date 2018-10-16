---
title: Migreren van Prijscategorieën van Custom Speech Service-eindpunten in Azure | Microsoft Docs
description: Informatie over het migreren van implementaties van lagen S0 en S1 naar S2 van Custom Speech Service-eindpunten in Cognitive Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/05/2017
ms.author: panosper
ms.openlocfilehash: a9bdb257137db0063d39f028a69e2164eccbdc31
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49340457"
---
# <a name="migrate-deployments-to-the-new-pricing-model"></a>Migreren van implementaties naar het nieuwe prijsmodel

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-custom-speech-deprecation-note.md)]

Vanaf juli 2017, Custom Speech Service biedt een [nieuwe prijsmodel](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/). Het nieuwe model is *gemakkelijker te begrijpen*, *eenvoudiger om kosten te berekenen*, en *flexibelere* in termen van schalen. Voor het schalen, heeft Microsoft introduceert het concept van een schaaleenheid. Elke schaaleenheid kan vijf gelijktijdige aanvragen worden verwerkt. De schaal voor gelijktijdige aanvragen in het oude model is ingesteld op 5 gelijktijdige aanvragen voor S0-laag en is ingesteld op 12 gelijktijdige aanvragen voor laag S1. We hebben deze limieten te te bieden u meer flexibiliteit met uw use-casevereisten geopend.

Als u een oude S0 of S1-laag uitvoert, wordt u aangeraden dat u uw bestaande implementaties naar de nieuwe S2-laag migreren. De nieuwe S2-laag omvat de S0- en de S1-laag. Hier ziet u de beschikbare opties in de volgende afbeelding:

![De pagina 'Uw prijscategorie kiezen'](../../../media/cognitive-services/custom-speech-service/custom-speech-pricing-tier.png)

Microsoft verwerkt de migratie op een semi-automatische manier. U kunt eerst de migratie activeren door de nieuwe prijscategorie te selecteren. We Migreer vervolgens de implementatie automatisch.

De toewijzing van de oude lagen eenheden schalen wordt weergegeven in de volgende tabel:

| Laag | Gelijktijdige aanvragen (oud model) | Migratie | Gelijktijdige aanvragen |
|----- | ----- | ---- | ---- |
| S0 |  5   |   => **S2** met 1 schaaleenheid |   5 |
| S1 |  12  |   => **S2** met 3 schaaleenheden |  15 |

Als u wilt migreren naar de nieuwe laag, het volgende doen:

## <a name="step-1-check-your-existing-deployment"></a>Stap 1: Controleer de bestaande implementatie
Ga naar de [Custom Speech Service portal](http://cris.ai), en controleert u uw bestaande implementaties. In ons voorbeeld zijn er twee implementaties. Een implementatie wordt uitgevoerd op een S0-laag en de andere implementatie wordt uitgevoerd op een S1-laag. De implementaties worden weergegeven in de **implementatieopties** kolom van de volgende tabel:

![De pagina implementaties](../../../media/cognitive-services/custom-speech-service/custom-speech-deployments.png)

## <a name="step-2-select-your-new-pricing-tier-in-the-azure-portal"></a>Stap 2: Selecteer de nieuwe prijscategorie in Azure portal
1. Open een nieuw browsertabblad en zich aanmelden bij de [Azure-portal](http://ms.portal.azure.com/). 

2. In de **Cognitive Services** deelvenster in de **abonnementen** , selecteert u uw aangepaste spraak-abonnement. 

3. Selecteer in het deelvenster voor uw abonnement, **prijscategorie**.

    ![De koppeling "Prijscategorie"](../../../media/cognitive-services/custom-speech-service/custom-speech-update-tier.png)

4. Op de **uw prijscategorie kiezen** weergeeft, schakelt **S2 Standard**. Deze prijscategorie is de nieuwe, vereenvoudigde en flexibelere prijscategorie.

5. Selecteer de **Selecteer** knop.

    ![De pagina 'Uw prijscategorie kiezen'](../../../media/cognitive-services/custom-speech-service/custom-speech-update-pricing.png)

## <a name="step-3-check-the-migration-status-in-the-custom-speech-service-portal"></a>Stap 3: Controleer de migratiestatus in de portal voor Custom Speech Service
Ga terug naar de Custom Speech Service-portal en controleer uw implementaties. (Als het browservenster nog steeds geopend is, vernieuwd.) 

De status van de gerelateerde implementatie moet zijn overgeschakeld naar *verwerking*. U kunt ook de migratie valideren door het controleren van de **implementatieopties** kolom. Er kan nu vindt u informatie over schaaleenheden en logboekregistratie. De schaaleenheden moeten overeenkomen met uw vorige prijscategorie. De logboekregistratie moet ook worden ingeschakeld op, zoals wordt weergegeven in de tabel:

![De kolom implementatie-opties](../../../media/cognitive-services/custom-speech-service/custom-speech-deployments-new.png)


> [!NOTE]
> Als u problemen tijdens de migratie hebt, contact met ons opnemen.
>

## <a name="next-steps"></a>Volgende stappen
Zie voor meer zelfstudies:
* [Een aangepast akoestisch model maken](cognitive-services-custom-speech-create-acoustic-model.md)
* [Een aangepaste taalmodel maken](cognitive-services-custom-speech-create-language-model.md)
* [Maak een aangepaste spraak-naar-tekst-eindpunt](cognitive-services-custom-speech-create-endpoint.md)
