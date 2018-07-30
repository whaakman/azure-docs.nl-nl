---
title: 'Snelstartgids: Spraakherkenning in Java op Android met behulp van de Cognitive Services Speech SDK herkennen'
titleSuffix: Microsoft Cognitive Services
description: Meer informatie over het herkennen van gesproken tekst in Java op Android met behulp van de Cognitive Services spraak-SDK
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: fmegen
ms.openlocfilehash: ad1bf15f198529487dbed92d8f8639704b0c5fc7
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325122"
---
# <a name="quickstart-recognize-speech-in-java-on-android-using-the-speech-sdk"></a>Snelstartgids: Spraakherkenning in Java op Android met behulp van de SDK spraak herkennen

[!include[Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel leert u over het maken van een Java-toepassing voor Android met behulp van de Cognitive Services Speech SDK spraak naar tekst te transcriberen.
De toepassing is gebaseerd op de Microsoft Cognitive Services Speech SDK Maven-pakket, versie 0.5.0 en Android Studio 3.1.

> [!NOTE]
> Voor de SDK van de apparaten spraak en het apparaat Roobo, gaat u naar de [spraak Devices SDK](speech-devices-sdk.md) pagina.

## <a name="prerequisites"></a>Vereisten

* Een abonnementssleutel voor de Speech-service. Zie [de spraakservice gratis uitproberen](get-started.md).
* Een PC (Windows, Linux, Mac) die geschikt zijn om uit te voeren Android Studio.
* Versie 3.1 van [Android Studio](https://developer.android.com/studio/).
* Een op basis van ARM Android-apparaat (API 23: Android 6.0 Marshmallow of hoger) [ingeschakeld voor de ontwikkeling van](https://developer.android.com/studio/debug/dev-options) met een microfoon werken.

## <a name="create-an-android-studio-project"></a>Een Android Studio-project maken

Android Studio starten en selecteer **Start een nieuw Android Studio-project**.

![](media/sdk/qs-java-android-01-start-new-android-studio-project.png)

In de **nieuw Project maken** wizard die wordt geleverd omhoog, moet u de volgende opties:

1. In de **Android-Project maken** scherm, voert u **snelstartgids** als **toepassingsnaam**, **samples.speech.cognitiveservices.microsoft.com** als **bedrijfsdomein**, en kies de projectlocatie van een. Laat u de selectievakjes uitgeschakeld en klik op **volgende**.

   ![](media/sdk/qs-java-android-02-create-android-project.png)

1. In de **Android doelapparaten** scherm selectievakje **telefoons en tablets** als de enige optie kiezen **API 23: Android 6.0 (Marshmallow)** in de vervolgkeuzelijst en klik op **Volgende**.

   ![](media/sdk/qs-java-android-03-target-android-devices.png)

1. In de **toevoegen van een activiteit aan Mobile** scherm, selecteer **lege activiteit** en klikt u op **volgende**.

   ![](media/sdk/qs-java-android-04-add-an-activity-to-mobile.png)

1. In de **configureren activiteit** scherm **MainActivity** als naam van de activiteit en **activiteit\_belangrijkste** als de naam van de lay-out. Beide selectievakjes uit en klikt u op **voltooien**.

   ![](media/sdk/qs-java-android-05-configure-activity.png)

Nadat het is uitgevoerd voor een tijdje uw zojuist gemaakte Android Studio-project moet zijn beschikbaar.

## <a name="configure-your-project-for-the-speech-sdk"></a>Uw project voor de spraak-SDK configureren

[!include[License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

De huidige versie van de Cognitive Services Speech SDK is `0.5.0`.

De spraak-SDK voor Android wordt geleverd als een [AAR (Android-bibliotheek)](https://developer.android.com/studio/projects/android-library), waaronder de vereiste bibliotheken, evenals de vereiste Android-machtigingen voor het gebruik ervan.
Deze wordt gehost in een Maven-opslagplaats op https://csspeechstorage.blob.core.windows.net/maven/.

We dat hieronder wordt beschreven hoe u het instellen van uw project met de spraak-SDK.

Open het venster van de structuur project onder **bestand** \> **projectstructuur**.
In het venster dat weergegeven maken de volgende wijzigingen wordt (Klik op **OK** alleen nadat u alle stappen hebt voltooid):

1. Selecteer **Project**, en bewerk de **standaard bibliotheek opslagplaats** instellingen door een komma en onze opslagplaats-URL op Maven tussen enkele aanhalingstekens toe te voegen `'https://csspeechstorage.blob.core.windows.net/maven/'`:

  ![](media/sdk/qs-java-android-06-add-maven-repository.png)

1. Selecteer nog steeds in hetzelfde scherm, aan de linkerkant de **App** -module, en in de rechterbovenhoek de **afhankelijkheden** tabblad. Vervolgens klikt u op het groen plusteken in de rechterbovenhoek rechts en selecteer **bibliotheek afhankelijkheid**.

  ![](media/sdk/qs-java-android-07-add-module-dependency.png)

1. Voer in het venster dat weergegeven wordt, de naam en versie van onze spraak-SDK voor Android, `com.microsoft.cognitiveservices.speech:client-sdk:0.5.0`, klikt u vervolgens op **OK**.
   De spraak-SDK moet worden toegevoegd aan de lijst met afhankelijkheden nu, zoals hieronder wordt weergegeven:

  ![](media/sdk/qs-java-android-08-dependency-added.png)

1. Selecteer in de rechterbovenhoek de **eigenschappen** tabblad. Selecteer **1.8** voor **compatibele gegevensbron** en **gericht op compatibiliteit**.

  ![](media/sdk/qs-java-android-09-dependency-added.png)

1. Klik tot slot, **OK** sluiten de **projectstructuur** windows en alle updates toe te passen.

## <a name="create-a-minimal-ui"></a>Een minimale gebruikersinterface maken

De indeling van uw belangrijkste activiteit bewerken `activity_main.xml`.
Standaard deze moet zijn beschikbaar met een titelbalk met de naam van uw toepassing en een TextView met de tekst 'Hallo wereld!'.

* Klik op de TextView. Wijzigen van het id-kenmerk in de rechterbovenhoek op `hello`.

* In het palet in de linkerbovenhoek van de `activity_main.xml` venster, Sleep een knop in de lege ruimte boven de tekst.

* In de kenmerken van de knop aan de rechterkant, in de waarde voor de `onClick` kenmerk, voer `onSpeechButtonClicked`, worden de naam van de handler van onze knop.
  Wijzigen van het id-kenmerk in de rechterbovenhoek op `button`.

* Gebruik het pictogram Toverstaf aan de bovenkant van de ontwerper als u wilt afleiden van lay-out beperkingen voor u.

  ![](media/sdk/qs-java-android-10-infer-layout-constraints.png)

De tekst en de grafische versie van de gebruikersinterface moeten er nu uitzien als volgt:

<table>
<tr>
<td valign="top">
![](media/sdk/qs-java-android-11-gui.png)
</td>
<td valign="top">
[! code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/res/layout/activity_main.xml)]
</td>
</tr>
</table>

## <a name="add-the-sample-code"></a>De voorbeeldcode toevoegen

1. Bewerk de `MainActivity.java` bronbestand en vervang de code door het volgende (onder de pakketinstructie):

   [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/quickstart/MainActivity.java#code)]

   * De `onCreate` methode omvat een code die vraagt om microfoon en machtigingen voor Internet, evenals de systeemeigen platform-binding wordt geïnitialiseerd. Het configureren van de systeemeigen platform-bindingen is slechts eenmaal vereist, dat wil zeggen, deze moet worden gedaan vroeg tijdens de initialisatie van toepassingen.
   
   * De methode `onSpeechButtonClicked` is eerder zo ingericht dat als de knop klikt u op de handler. Druk op een knop wordt de werkelijke spraakherkenning geactiveerd.

1. Vervang de tekenreeks `YourSubscriptionKey` met de abonnementssleutel van uw.

1. Vervang de tekenreeks `YourServiceRegion` met de [regio](regions.md) die zijn gekoppeld aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

## <a name="build-and-run-the-sample"></a>Het voorbeeldproject compileren en uitvoeren

* Om te bouwen, druk op Ctrl + F9 of selecteer **bouwen** \> **Project maken**.

* Uw Android-apparaat verbinden met ontwikkeling van uw PC. Zorg ervoor dat u hebt [Ontwikkelingsmodus en USB-foutopsporing ingeschakeld](https://developer.android.com/studio/debug/dev-options).

* U start de app, drukt u op Shift + F10 of selecteer **uitvoeren** \> **uitvoeren 'app'**.

* In de windows-implementatie doel dat wordt weergegeven, kiest u uw Android-apparaat.

  ![Start de app into-foutopsporing](media/sdk/qs-java-android-12-deploy.png)

* De app te op uw apparaat starten.
  Zodra u op de knop drukt, de volgende 15 seconden worden herkend en weergegeven in de gebruikersinterface (u zou ook het mogelijk om te zien van het antwoord in het venster logcat in Android Studio):

  ![Gebruikersinterface na geslaagde opname](media/sdk/qs-java-android-13-gui-on-device.png)

Deze schermopname is de Snelstartgids voor Android. De voorbeeldcode volledige project kan worden gedownload vanuit de opslagplaats met voorbeelden.

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Zoek in dit voorbeeld in de `quickstart/java-android` map.

## <a name="next-steps"></a>Volgende stappen

* [Onze voorbeelden ophalen](speech-sdk.md#get-the-samples)
