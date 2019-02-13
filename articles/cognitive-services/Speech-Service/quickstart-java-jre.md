---
title: 'Quickstart: Spraak herkennen, Java (Windows, Linux) - Spraakservices'
titleSuffix: Azure Cognitive Services
description: In deze quickstart leert u hoe u een eenvoudige Java-toepassing maakt die tekst ingesproken in de microfoon van uw computer vastlegt en transcribeert.
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/18/2018
ms.author: fmegen
ms.openlocfilehash: 9185d8d236b2a3283cf5a542002adadd0ac74686
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55820535"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-java"></a>Snelstart: Spraak herkennen met de Speech-SDK voor Java

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel maakt u een Java-consoletoepassing met behulp van de [Speech Service-SDK](speech-sdk.md). Doel is om realtime spraak naar tekst om te zetten vanuit de microfoon van uw pc. De toepassing wordt gebouwd met het Speech SDK Maven-pakket en de Eclipse Java IDE (v4.8) op 64-bit Windows of 64-bit Ubuntu Linux 16.04/18.04. De toepassing wordt uitgevoerd op een 64-bit Java 8 runtime-omgeving (JRE).

> [!NOTE]
> Zie [Speech Devices SDK](speech-devices-sdk.md) voor de Speech Devices-SDK en het Roobo-apparaat.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* Besturingssysteem: Windows (64-bit) of Ubuntu Linux 16.04/18.04 (64-bit)
* [Eclipse Java IDE](https://www.eclipse.org/downloads/)
* [Java 8](https://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) of [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* Een Azure-abonnementssleutel voor de Spraakservice. [Gratis downloaden](get-started.md).

Als u Ubuntu 16.04/18.04 gebruikt, zorg er dan voor dat deze afhankelijkheden zijn geïnstalleerd voordat u Eclipse start.

```console
sudo apt-get update
sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
```

Als u werkt met Windows (64-bits), controleer dan of u Microsoft Visual C++ Redistributable hebt geïnstalleerd voor uw platform.
* [Download Microsoft Visual C++ Redistributable voor Visual Studio 2017](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads)


## <a name="create-and-configure-project"></a>Project maken en configureren

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-create-proj.md)]

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Om een nieuwe lege klasse aan uw Java-project toe te voegen, selecteert u **Bestand** > **Nieuw** > **Klasse**.

1. Voer in het venster **Nieuwe Java-klasse**, in het veld **Pakket**, **speechsdk.quickstart** in en voer in het veld **Naam** **Main** in.

   ![Schermopname van het venster Nieuwe Java-klasse](media/sdk/qs-java-jre-06-create-main-java.png)

1. Vervang alle code in `Main.java` door het volgende codefragment:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/src/speechsdk/quickstart/Main.java#code)]

1. Vervang de tekenreeks `YourSubscriptionKey` door uw abonnementssleutel.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla de wijzigingen in het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

Druk op F11 of selecteer **Uitvoeren** > **Fouten opsporen**.
De volgende 15 seconden aan spraakinvoer vanuit uw microfoon worden herkend en geregistreerd in het consolevenster.

![Schermafbeelding van console-uitvoer na geslaagde herkenning](media/sdk/qs-java-jre-07-console-output.png)

## <a name="next-steps"></a>Volgende stappen

Op GitHub vindt u aanvullende voorbeelden, zoals hoe u spraak kunt lezen vanuit een audiobestand.

> [!div class="nextstepaction"]
> [Bekijk Java-voorbeelden op GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Snelstart: Omzetten van spraak, Java (Windows, Linux)](quickstart-translate-speech-java-jre.md)
- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
