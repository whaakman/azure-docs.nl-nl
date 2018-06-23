---
title: Abonnement-sleutels ophalen voor de aangepaste spraak-Service op Azure | Microsoft Docs
description: Informatie over het ophalen van abonnement sleutels voor het aanroepen van de aangepaste spraak-Service in cognitieve Services.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 02/08/2017
ms.author: panosper
ms.openlocfilehash: fcef86a19a77581ff82b64173e2ac68b26ae708a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344498"
---
# <a name="obtain-subscription-keys"></a>Abonnementssleutels verkrijgen
Om te beginnen met de Service Azure aangepaste spraak, moet u eerst uw account koppelen aan een Azure-abonnement. Abonnementen op gratis en betaalde niveaus zijn beschikbaar. Zie voor informatie over de lagen, de [pagina met prijzen](https://www.microsoft.com/cognitive-services/en-us/pricing).

## <a name="get-a-subscription-key"></a>Een abonnementssleutel ophalen
1. De abonnementssleutel van een kunt u krijgen via de Azure-portal op twee manieren:

    * Ga naar de [Azure-portal](https://ms.portal.azure.com), en een nieuwe cognitieve Services API toevoegen door te zoeken naar _cognitieve Services_ en selecteren **cognitieve API's voor Services**.

      ![Cognitieve Services zoeken](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-subscription.png)

    * Of gaat u rechtstreeks naar de [cognitieve API's voor Services](https://ms.portal.azure.com/#create/Microsoft.CognitiveServices).

        ![Cognitive Services-API's](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-subscription2.png)

    
2. Vul de volgende vereiste velden in:

      a. **Accountnaam**. Gebruik een naam die voor u geschikt. Deze naam onthouden, zodat u uw abonnement cognitieve Services in de lijst met resources vinden kunt.

      b. **Abonnement**. Selecteer een van uw Azure-abonnementen.

      c. **API-type**. Selecteer **aangepaste spraak-Service (Preview)**.

      d. **Locatie**. Het is momenteel **VS-West**.

      e. **Prijscategorie**. Selecteer de laag die geschikt is voor u. **F0** gratis laag is. De quota die mogen worden uitgelegd op de [pagina met prijzen](https://www.microsoft.com/cognitive-services/en-us/pricing).

      ![Cognitieve Services-account maken](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-blade.png)

3. U moet ofwel een weergave op uw dashboard of een service met de opgegeven accountnaam vinden in de lijst met uw resources. Wanneer u deze optie inschakelt, kunt u een overzicht van uw service. In de lijst aan de linkerkant onder **bronbeheer**, selecteer **sleutels**. Kopiëren **sleutel 1**.

      Deze abonnementssleutel is vereist in de volgende stappen.

      ![SLEUTEL 1](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-keys2.png)

      > [!NOTE]
      > Kopieer niet de **abonnements-ID** van de overzichtspagina. U moet de sleutel van het abonnement in de volgende stap.
      >

      ![Overzicht abonnements-ID](../../../media/cognitive-services/custom-speech-service/custom-speech-azure-cris-keys.png)

4. Selecteer uw gebruikersaccount om in te voeren van uw abonnementssleutel op het lint in de rechterbovenhoek. Selecteer op de vervolgkeuzelijst **abonnementen**.

      ![Abonnementen menu-item](../../../media/cognitive-services/custom-speech-service/custom-speech-subscription-selection.png)

    Een tabel van abonnementen weergegeven die leeg is de eerste keer dat deze wordt geopend.

    ![Abonnementen tabel](../../../media/cognitive-services/custom-speech-service/custom-speech-subscription-list.png)

5. Selecteer **nieuwe toevoegen**. Voer een naam voor het abonnement en de abonnementssleutel. Het kan zijn **KEY 1** (primaire sleutel) of **sleutel 2** (secundaire sleutel) van uw abonnement.

      ![De sleutelnaam abonnement](../../../media/cognitive-services/custom-speech-service/custom-speech-enter-subsciption.png)

Als u wilt uploaden van gegevens een model trainen of een implementatie, moet u uw aangepaste spraak serviceactiviteiten koppelen aan een Azure-abonnement. Het kan een gratis laag of een laag betaald abonnement zijn. Zie de pagina [prijzen](https://www.microsoft.com/cognitive-services/en-us/pricing) voor meer informatie.

## <a name="get-a-subscription-id"></a>Een abonnement-ID ophalen
Als u een abonnements-ID, gaat u naar de Azure-portal. Zoeken naar *cognitieve Services* en *aangepaste spraak Service*, en volg de instructies.

> [!NOTE]
> De abonnementssleutel is verderop in dit proces vereist.
>

## <a name="next-steps"></a>Volgende stappen
* Beginnen met het maken van uw [aangepaste akoestisch model](cognitive-services-custom-speech-create-acoustic-model.md).
* Beginnen met het maken van uw [aangepaste taalmodel](cognitive-services-custom-speech-create-language-model.md).
