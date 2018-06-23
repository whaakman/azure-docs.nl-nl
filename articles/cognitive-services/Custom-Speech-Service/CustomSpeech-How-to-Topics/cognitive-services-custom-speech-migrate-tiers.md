---
title: Migreren van de eindpunten van aangepaste spraak Service prijscategorieën in Azure | Microsoft Docs
description: Informatie over het migreren van implementaties van opslaglagen S0 en S1 naar S2 van aangepaste spraak Service-eindpunten in cognitieve Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/05/2017
ms.author: panosper
ms.openlocfilehash: 6d92459deb3464cd97c215cbf9a8320628b6da80
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344756"
---
# <a name="migrate-deployments-to-the-new-pricing-model"></a>Implementaties naar het nieuwe prijsmodel migreren
Vanaf juli 2017 aangepaste spraak Service biedt een [nieuwe prijzen model](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/). Het nieuwe model wordt *gemakkelijker te begrijpen*, *eenvoudiger om kosten te berekenen*, en *flexibelere* in termen van schalen. Microsoft heeft het concept van een schaaleenheid geïntroduceerd voor schaling. Elke schaaleenheid staat vijf gelijktijdige aanvragen te verwerken. De schaal voor gelijktijdige aanvragen in het oude model is ingesteld op 5 gelijktijdige aanvragen voor laag S0 en is ingesteld op 12 gelijktijdige aanvragen voor laag S1. We hebt deze limieten wilt bieden u meer flexibiliteit met de vereisten van uw use case geopend.

Als u een oude S0 of S1 laag uitvoert, raden wij aan uw bestaande implementaties te migreren naar de nieuwe S2 laag. De nieuwe S2 laag bevat informatie over de S0 en de S1 lagen. Hier ziet u de beschikbare opties in de volgende afbeelding:

![De pagina 'Kies uw prijscategorie'](../../../media/cognitive-services/custom-speech-service/custom-speech-pricing-tier.png)

Microsoft omgaat met de migratie in een semi automatische manier. U eerst activeren de migratie door het selecteren van de nieuwe prijscategorie. We Migreer vervolgens uw implementatie automatisch.

In de volgende tabel ziet u de toewijzing van de oude lagen eenheden schalen:

| Laag | Gelijktijdige aanvragen (oude model) | Migratie | Gelijktijdige aanvragen |
|----- | ----- | ---- | ---- |
| S0 |  5   |   => **S2** met 1 schaaleenheid |   5 |
| S1 |  12  |   => **S2** met 3 schaaleenheden |  15 |

Als u wilt migreren naar de nieuwe laag, het volgende doen:

## <a name="step-1-check-your-existing-deployment"></a>Stap 1: Controleer de implementatie van uw bestaande
Ga naar de [aangepaste spraak-serviceportal](http://cris.ai), en controleer uw bestaande implementaties. In ons voorbeeld zijn er twee implementaties. Een implementatie wordt uitgevoerd op een laag S0 en de andere implementatie wordt uitgevoerd op een laag S1. De implementaties worden weergegeven in de **implementatieopties** kolom van de volgende tabel:

![De pagina implementaties](../../../media/cognitive-services/custom-speech-service/custom-speech-deployments.png)

## <a name="step-2-select-your-new-pricing-tier-in-the-azure-portal"></a>Stap 2: Uw nieuwe prijscategorie selecteren in de Azure portal
1. Open een nieuw browsertabblad en meld u bij de [Azure-portal](http://ms.portal.azure.com/). 

2. In de **cognitieve Services** deelvenster, in de **abonnementen** , selecteert u uw abonnement aangepaste spraak. 

3. Selecteer in het deelvenster voor uw abonnement **prijscategorie**.

    ![De koppeling 'Prijscategorie'](../../../media/cognitive-services/custom-speech-service/custom-speech-update-tier.png)

4. Op de **Kies uw prijscategorie** pagina **Standard S2**. Deze prijscategorie is de nieuwe, vereenvoudigd en flexibeler prijscategorie.

5. Selecteer de **Selecteer** knop.

    ![De pagina 'Kies uw prijscategorie'](../../../media/cognitive-services/custom-speech-service/custom-speech-update-pricing.png)

## <a name="step-3-check-the-migration-status-in-the-custom-speech-service-portal"></a>Stap 3: Controleer de migratiestatus in de aangepaste spraak Service portal
Terug naar de serviceportal aangepaste spraak en uw implementaties te controleren. (Als uw browservenster nog steeds geopend is, vernieuwd.) 

De status van de gerelateerde implementatie moet zijn overgeschakeld naar *verwerken*. U kunt ook de migratie valideren door het controleren van de **implementatieopties** kolom. U kunt er nu informatie over schaaleenheden en logboekregistratie zoeken. De schaaleenheden die moeten overeenkomen met uw vorige prijscategorie. De logboekregistratie moet ook worden ingeschakeld, zoals weergegeven in de tabel:

![De kolom implementatieopties](../../../media/cognitive-services/custom-speech-service/custom-speech-deployments-new.png)


> [!NOTE]
> Als u problemen tijdens de migratie hebt, contact met ons opnemen.
>

## <a name="next-steps"></a>Volgende stappen
Zie voor meer zelfstudies:
* [Een aangepaste akoestisch model maken](cognitive-services-custom-speech-create-acoustic-model.md)
* [Een aangepaste taalmodel maken](cognitive-services-custom-speech-create-language-model.md)
* [Maak een aangepaste spraak naar tekst-eindpunt](cognitive-services-custom-speech-create-endpoint.md)
