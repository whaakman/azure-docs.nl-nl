---
title: 'Snelstartgids: Spraak herkennen, Java (Android) - Speech Services'
titleSuffix: Azure Cognitive Services
description: Leer gesproken tekst herkennen in Java onder Android met behulp van de Speech SDK
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 2/20/2019
ms.author: wolfma
ms.openlocfilehash: 2ee5028e0766ec52c407bf0ef0e18885c11734d8
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961043"
---
# <a name="quickstart-recognize-speech-in-java-on-android-by-using-the-speech-sdk"></a>Quickstart: Gesproken tekst herkennen in Java onder Android met behulp van de Speech-SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel leert u hoe u een Java-toepassing voor Android kunt ontwikkelen met behulp van de Cognitive Services Speech SDK om spraak om te zetten in tekst.
De toepassing is gebaseerd op het Microsoft Cognitive Services Speech SDK Maven-pakket, versie 1.3.1 en Android Studio 3.1.
De Speech SDK is op dit moment compatibel met Android-apparaten met 32/64-bits ARM- en Intel x86/x64-compatibele processors.

> [!NOTE]
> Zie [Speech Devices SDK](speech-devices-sdk.md) voor de Speech Devices-SDK en het Roobo-apparaat.

## <a name="prerequisites"></a>Vereisten

U hebt een abonnementssleutel voor de Speech-service nodig om deze snelstartgids te doorlopen. U kunt er gratis een krijgen. Zie [Speech-service gratis uitproberen](get-started.md) voor meer informatie.

## <a name="create-and-configure-a-project"></a>Een project maken en configureren

1. Start Android Studio starten en kies **Start a new Android Studio project** in het welkomstscherm.

    ![Schermafbeelding van het welkomstscherm van Android Studio](media/sdk/qs-java-android-01-start-new-android-studio-project.png)

1. De wizard **Create New Project** wordt weergegeven. Voer in het scherm **Create Android Project** **Quickstart** in voor **Application name**, **samples.speech.cognitiveservices.microsoft.com** voor **Company domain** en kies een projectmap. Laat de selectievakjes voor C++ en Kotlin uitgeschakeld, en selecteer **Next**.

   ![Schermafbeelding van de wizard Create New Project](media/sdk/qs-java-android-02-create-android-project.png)

1. Selecteer in het scherm **Target Android Devices** alleen **Phone and Tablet**. Kies in de vervolgkeuzelijst hieronder **API 23: Android 6.0 (Marshmallow)** en selecteer **Volgende**.

   ![Schermafbeelding van de wizard Create New Project](media/sdk/qs-java-android-03-target-android-devices.png)

1. Selecteer **Empty Activity** in het scherm **Add an Activity to Mobile** en klik op **Next**.

   ![Schermafbeelding van de wizard Create New Project](media/sdk/qs-java-android-04-add-an-activity-to-mobile.png)

1. Gebruik in het scherm **Configure Activity** **MainActivity** als de naam van de activiteit en **activity\_main** als de naam van de lay-out. Schakel beide selectievakjes in en selecteer **Finish**.

   ![Schermafbeelding van de wizard Create New Project](media/sdk/qs-java-android-05-configure-activity.png)

Android Studio heeft even de tijd nodig om het nieuwe Android-project voor te bereiden. Configureer het project vervolgens voor gebruik van de Speech SDK en Java 8.

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

De huidige versie van de Speech SDK van Cognitive Services is `1.3.1`.

De Speech SDK voor Android wordt aangeboden als een [AAR (Android Library)](https://developer.android.com/studio/projects/android-library) die de vereiste bibliotheken en Android-machtigingen bevat.
De SDK wordt gehost in een Maven-opslagplaats op https://csspeechstorage.blob.core.windows.net/maven/.

Stel uw project in voor gebruik van de Speech SDK. Open het venster Project Structure door **File** > **Project Structure** te kiezen in de menubalk van Android Studio. Breng in het venster Project Structure de volgende wijzigingen aan:

1. Selecteer **Project** in de lijst aan de linkerkant van het venster. Bewerk de instellingen voor **Default Library Repository** door een komma toe te voegen en dan de URL van onze Maven-opslagplaats tussen enkele aanhalingstekens. 'https://csspeechstorage.blob.core.windows.net/maven/'

   ![Schermafbeelding van het venster Project Structure](media/sdk/qs-java-android-06-add-maven-repository.png)

1. Selecteer in hetzelfde scherm aan de linkerkant **app**. Selecteer vervolgens bovenaan het venster het tabblad **Dependencies**. Selecteer het groen plusteken (+) en kies **Library dependency** in de vervolgkeuzelijst.

   ![Schermafbeelding van het venster Project Structure](media/sdk/qs-java-android-07-add-module-dependency.png)

1. Voer in het venster dat verschijnt de naam en versie van onze Speech SDK voor Android in, `com.microsoft.cognitiveservices.speech:client-sdk:1.3.1`. Selecteer vervolgens **OK**.
   De Speech SDK moet nu worden toegevoegd aan de lijst met afhankelijkheden, zoals hieronder wordt weergegeven:

   ![Schermafbeelding van het venster Project Structure](media/sdk/qs-java-android-08-dependency-added-1.0.0.png)

1. Selecteer het tabblad **Properties**. Selecteer voor zowel **Source Compatibility** als **Target Compatibility** versie **1.8**.

   ![](media/sdk/qs-java-android-09-dependency-added.png)

1. Selecteer **OK** om het venster Project Structure te sluiten en wijzigingen toe te passen op het project.

## <a name="create-user-interface"></a>Gebruikersinterface maken

We gaan een eenvoudige gebruikersinterface maken voor de toepassing. Bewerk de lay-out voor de hoofdactiviteit, `activity_main.xml`. In eerste instantie bevat de lay-out een titelbalk met de naam van uw toepassing en een TextView met de tekst "Hello World!".

* Klik op het TextView-element. Wijzig het kenmerk ID in de rechterbovenhoek in `hello`.

* Sleep vanuit het palet in de linkerbovenhoek van het venster `activity_main.xml` een knop naar de lege ruimte boven de tekst.

* Ga naar de kenmerken van de knop aan de rechterkant en voer `onSpeechButtonClicked` in als de waarde voor het kenmerk `onClick`. We gaan een methode met deze naam schrijven om de knopgebeurtenis af te handelen.  Wijzig het kenmerk ID in de rechterbovenhoek in `button`.

* Gebruik het pictogram van de toverstaf bovenaan de ontwerpfunctie om beperkingen voor de lay-out af te leiden.

  ![Schermafbeelding van toverstafpictogram](media/sdk/qs-java-android-10-infer-layout-constraints.png)

De tekst en de grafische weergave van uw gebruikersinterface moeten er nu ongeveer als volgt uitzien:

<table>
<tr>
<td valign="top">
![](media/sdk/qs-java-android-11-gui.png)
</td>
<td valign="top">
[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/res/layout/activity_main.xml)]
</td>
</tr>
</table>

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Open het bronbestand `MainActivity.java`. Vervang alle code in het bestand door de volgende code.

   [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/quickstart/MainActivity.java#code)]

   * De methode `onCreate` omvat code die toestemming vraagt voor gebruik van de microfoon en internet, en initialiseert de binding met het native platform. Configuratie van bindingen voor het native platform hoeft maar één keer. Dit moet gebeuren in een vroeg stadium tijdens de initialisatie van de toepassing.

   * De methode `onSpeechButtonClicked` is, zoals eerder gezegd, de methode voor het afhandelen van het klikken op de knop. Een klik op de knop betekent dat de spraak wordt omgezet in tekst.

1. Vervang in hetzelfde bestand de tekenreeks `YourSubscriptionKey` door uw abonnementssleutel.

1. Vervang ook de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld (bijvoorbeeld `westus` voor het gratis proefabonnement).

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Sluit uw Android-apparaat aan op de ontwikkelmachine. Zorg ervoor dat u de [ontwikkelmodus en USB-foutopsporing](https://developer.android.com/studio/debug/dev-options) hebt ingeschakeld op het apparaat.

1. Druk op Ctrl + F9 om de toepassing te compileren, of kies **Build** > **Make Project** in de menubalk.

1. Als u de toepassing wilt starten, drukt u op Shift + F10 of kiest u **Run** > **Run 'app'**.

1. Kies uw Android-apparaat in het venster Select Deployment Target dat wordt weergegeven.

   ![Schermafbeelding van het venster Select Deployment Target](media/sdk/qs-java-android-12-deploy.png)

Klik op de knop in de toepassing om een sectie voor spraakherkenning te starten. De volgende 15 seconden aan Engels gesproken tekst wordt verzonden naar de Speech-service en getranscribeerd. Het resultaat wordt weergegeven in de Android-toepassing, en in het logcat-venster in Android Studio.

![Schermafbeelding van de Android-toepassing](media/sdk/qs-java-android-13-gui-on-device.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Bekijk Java-voorbeelden op GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
