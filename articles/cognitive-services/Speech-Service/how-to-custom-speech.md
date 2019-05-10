---
title: Aan de slag met aangepaste spraak - spraakservices
titlesuffix: Azure Cognitive Services
description: Aangepaste spraak is een set online hulpprogramma's waarmee u kunt evalueren en Microsoft spraak-naar-tekst nauwkeurigheid voor uw toepassingen, hulpprogramma's en -producten te verbeteren. Alles die wat u nodig om te beginnen zijn een aantal testbestanden audio. Volg de koppelingen hieronder om te beginnen met het maken van een aangepaste spraak-naar-tekst-ervaring.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: ab33feb1ffdbced193afaba8f52719b3c215652f
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65511127"
---
# <a name="what-is-custom-speech"></a>Wat is aangepaste spraak?

[Aangepaste spraak](https://aka.ms/custom-speech) is een set online hulpprogramma's waarmee u kunt evalueren en Microsoft spraak-naar-tekst nauwkeurigheid voor uw toepassingen, hulpprogramma's en -producten te verbeteren. Alles die wat u nodig om te beginnen zijn een aantal testbestanden audio. Volg de koppelingen hieronder om te beginnen met het maken van een aangepaste spraak-naar-tekst-ervaring.

## <a name="whats-in-custom-speech"></a>Wat is er in aangepaste spraak?

Voordat u iets met aangepaste spraak doen kunt, moet u een Azure-account en een abonnement Speech Services. Nadat u hebt een account, kunt u uw gegevens voorbereiden, trainen en testen van uw modellen, erkenning kwaliteit inspecteren, nauwkeurigheid, evalueren en uiteindelijk implementeren en gebruiken de aangepaste spraak-naar-tekst-model.

Dit diagram ziet u de onderdelen van de aangepaste spraak-portal. Gebruik de onderstaande koppelingen voor meer informatie over elke stap.

![Ziet u de verschillende onderdelen die gezamenlijk de Custom Speech-portal.](./media/custom-speech/custom-speech-overview.png)

1. [Abonneren en maak een project](#set-up-your-azure-account) : Maak een Azure-account en abonnement van de spraakservices. Deze geïntegreerde abonnement hebt u toegang tot de spraak-naar-tekst, Text to Speech, spraakomzetting en de aangepaste spraak-portal. Met behulp van uw abonnement Speech Services, maak vervolgens uw eerste aangepaste spraak-project.

2. [Testgegevens uploaden](how-to-custom-speech-test-data.md) -Upload-testgegevens (audio-bestanden) van Microsoft spraak-naar-tekst biedt voor uw toepassingen, hulpprogramma's en -producten te evalueren.

3. [Inspecteer de kwaliteit van de herkenning](how-to-custom-speech-inspect-data.md) -gebruik van aangepaste spraak portal voor geüploade geluid afspelen en de kwaliteit van de spraakherkenning erkenning van uw testgegevens te inspecteren. Zie voor kwantitatieve metingen, [inspecteren gegevens](how-to-custom-speech-inspect-data.md).

4. [Evalueren van nauwkeurigheid](how-to-custom-speech-evaluate-data.md) -evalueren van de nauwkeurigheid van het model spraak-naar-tekst. De aangepaste spraak-portal biedt een *Word Foutfrequentie*, die kan worden gebruikt om te bepalen of extra training vereist is. Als u tevreden met de nauwkeurigheid bent, kunt u rechtstreeks de Speech-Service-API's gebruiken. Als u wilt de nauwkeurigheid verbeteren door een relatieve gemiddelde van 5-20%, gebruikt u de **Training** tabblad in de portal voor het uploaden van aanvullende gegevens, zoals human etiket transcripties en tekst.

5. [Het model te trainen](how-to-custom-speech-train-model.md) - verbeteren van de nauwkeurigheid van uw model spraak-naar-tekst door schriftelijke transcripties (10-1.000 uur) en tekst (< 200 MB), samen met uw audio-testgegevens. Deze gegevens kunt u de spraak-naar-tekst-model te trainen. Na de training, testen, en als u tevreden met het resultaat bent, kunt u uw model implementeren.

6. [Het model implementeren](how-to-custom-speech-deploy-model.md) : maken van een aangepast eindpunt voor uw model spraak-naar-tekst en worden gebruikt in uw toepassingen, hulpprogramma's of producten.

## <a name="set-up-your-azure-account"></a>Instellen van uw Azure-account

Een abonnement Speech Services is vereist voordat u de portal voor aangepaste spraak gebruiken kunt om een aangepast model te maken. Volg deze instructies voor het maken van een standaardabonnement Speech Services: [Maken van een abonnement spraak](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#new-azure-account).

> [!NOTE]
> Zorg ervoor dat standaardabonnementen (S0) maken, gratis proefabonnementen (F0) worden niet ondersteund.

Als u een Azure-account en een Speech Services-abonnement hebt gemaakt, moet u zich aanmelden bij de portal voor aangepaste spraak en verbinding maken met uw abonnement.

1. Uw abonnementssleutel Speech Services ophalen uit de Azure portal.
2. Aanmelden bij de [aangepaste spraak portal](https://aka.ms/custom-speech).
3. Selecteer het abonnement dat u werken wilt op en Verkrijg een spraak-project.
4. Als u uw abonnement niet wijzigen wilt, gebruikt u de **tandwiel** pictogram zich bevindt in de bovenste navigatiebalk.

## <a name="how-to-create-a-project"></a>Een project maken

Inhoud, zoals gegevens, modellen, tests en eindpunten zijn ingedeeld in **projecten** in de portal voor aangepaste spraak. Elk project is specifiek voor een domein en een land/taal. U kunt bijvoorbeeld een project voor callcenters die gebruikmaken van Engels in de Verenigde Staten maken.

Voor het maken van uw eerste project, selecteer de **spraak-naar-tekst/aangepaste spraak**, klikt u vervolgens op **nieuw Project**. Volg de instructies van de wizard om uw project te maken. Nadat u een project hebt gemaakt, ziet u vier tabbladen: **Gegevens**, **testen**, **Training**, en **implementatie**. Gebruik de koppelingen [Vervolgstappen](#next-steps) voor informatie over het gebruik van elk tabblad.

## <a name="next-steps"></a>Volgende stappen

* [Voorbereiden en testen van uw gegevens](how-to-custom-speech-test-data.md)
* [Controleer uw gegevens](how-to-custom-speech-inspect-data.md)
* [Evalueren van uw gegevens](how-to-custom-speech-evaluate-data.md)
* [Uw model te trainen](how-to-custom-speech-train-model.md)
* [Uw model implementeren](how-to-custom-speech-deploy-model.md)
