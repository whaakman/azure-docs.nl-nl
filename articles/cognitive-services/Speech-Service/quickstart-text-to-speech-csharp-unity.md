---
title: 'Quickstart: Spraak, Unity - spraakservices bootsen'
titleSuffix: Azure Cognitive Services
description: Deze handleiding gebruiken om een Text to Speech-toepassing maken met Unity en de spraak-SDK voor Unity (bèta). Wanneer u klaar bent, kunt u spraak vanuit tekst in realtime op van uw apparaat spreker bootsen.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 6/26/2019
ms.author: yinhew
ms.openlocfilehash: 5240ea45097ce3c0ae7ccbc15a7f99b2f5990832
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67467486"
---
# <a name="quickstart-synthesize-speech-with-the-speech-sdk-for-unity-beta"></a>Quickstart: Bootsen spraak met de spraak-SDK voor Unity (bèta)

Snelstartgidsen zijn ook beschikbaar voor [spraakherkenning](quickstart-csharp-unity.md).

Gebruik deze handleiding voor het maken van een toepassing voor tekst naar spraak met [Unity](https://unity3d.com/) en de spraak-SDK voor Unity (bèta).
Wanneer u klaar bent, kunt u spraak vanuit tekst in realtime op van uw apparaat spreker bootsen.
Als u niet bekend met Unity bent, is het raadzaam eerst de [gebruikershandleiding van Unity](https://docs.unity3d.com/Manual/UnityManual.html) te bestuderen voordat u begint met de ontwikkeling van uw toepassing.

> [!NOTE]
> De Speech-SDK voor Unity is momenteel alleen beschikbaar als bètaversie.
> Ondersteunt Windows-bureaublad (x86 en x64) of Universal Windows Platform (x86, x64, ARM/ARM64) en Android (x86, ARM32/64).

## <a name="prerequisites"></a>Vereisten

Als u dit project wilt voltooien, hebt u het volgende nodig:

* [Unity 2018.3 of hoger](https://store.unity.com/) met [Unity 2019.1 ondersteuning toegevoegd voor UWP-ARM64](https://blogs.unity3d.com/2019/04/16/introducing-unity-2019-1/#universal)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
     * Voor ondersteuning voor ARM64, installeert u de [optionele build-hulpprogramma's voor arm64 uit, en de Windows 10 SDK voor ARM64](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/) 
* Een abonnementssleutel voor de Speech Service. [Gratis downloaden](get-started.md).

## <a name="create-a-unity-project"></a>Een Unity-project maken

* Start Unity, selecteer het tabblad **Projecten** en vervolgens **Nieuw**.
* Geef bij **Projectnaam** **csharp-unity** op, geef bij **Sjabloon** **3D** op en kies een locatie.
  Selecteer vervolgens **Project maken**.
* Na enige tijd wordt het editorvenster van Unity weergegeven.

## <a name="install-the-speech-sdk"></a>De Speech-SDK installeren

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

* De Speech-SDK voor Unity (bètaversie) wordt geleverd als een Unity-activumpakket (.unitypackage).
  U kunt het [hier](https://aka.ms/csspeech/unitypackage) downloaden.
* Selecteer **Activa** > **Pakket importeren** > **Aangepast pakket** om de Speech-SDK te importeren.
  Raadpleeg de [documentatie voor Unity](https://docs.unity3d.com/Manual/AssetPackages.html) voor meer informatie.
* Selecteer in de bestandenkiezer het Speech-SDK-bestand (.unitypackage) dat u hierboven hebt gedownload.
* Zorg ervoor dat alle bestanden zijn geselecteerd en klik op **Importeren**:

  ![Schermopname van de Unity-editor bij het importeren van het activumpakket voor de Speech-SDK voor Unity](media/sdk/qs-csharp-unity-01-import.png)

## <a name="add-ui"></a>Gebruikersinterface toevoegen

We toevoegen een minimale gebruikersinterface aan onze scène, die bestaan uit een invoerveld de tekst invoeren voor synthese, een knop voor spraaksynthese trigger en een nieuw veld om het resultaat weer te geven.

* In [Hierarchy Window](https://docs.unity3d.com/Manual/Hierarchy.html) (standaard aan de linkerkant), wordt een voorbeeldscène weergegeven die in Unity bij het nieuwe project is gemaakt.
* Klik op de **maken** aan de bovenkant van de hiërarchie-venster en selecteer **UI** > **invoer veld**.
* Hiermee maakt u drie game objecten die u in het venster hiërarchie kunt zien: een **invoer veld** object genest binnen een **Canvas** -object, en een **EventSystem** object.
* [De weergave scène navigeren](https://docs.unity3d.com/Manual/SceneViewNavigation.html) , zodat u beschikt over een goed beeld van het canvas en het invoerveld in de [scène weergave](https://docs.unity3d.com/Manual/UsingTheSceneView.html).
* Klik op de **invoer veld** object in de hiërarchie-venster om weer te geven van de instellingen in de [Inspector-venster](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standaard aan de rechterkant).
* Instellen de **Pos X** en **Pos Y** eigenschappen **0**, zodat het invoerveld wordt gecentreerd in het midden van het canvas.
* Klik op de **maken** aan de bovenkant van het venster van de hiërarchie opnieuw en selecteer **UI** > **knop** om een knop te maken.
* Klik op het object **Button** in het venster Hierarchy om de bijbehorende instellingen weer te geven in het inspectievenster [Inspector Window](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standaard aan de rechterkant).
* Stelt de **Pos X** en **Pos Y** eigenschappen **0** en **-48**, en stel de **breedte** en **Hoogte** eigenschappen **160** en **30** om ervoor te zorgen dat de knop en het invoerveld niet overlappen.
* Klik op de **maken** aan de bovenkant van het venster van de hiërarchie opnieuw en selecteer **UI** > **tekst** te maken van een tekstveld.
* Klik op het object **Text** in het venster Hierarchy om de bijbehorende instellingen weer te geven in het inspectievenster [Inspector Window](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standaard aan de rechterkant).
* Stelt de **Pos X** en **Pos Y** eigenschappen **0** en **80**, en stel de **breedte** en  **Hoogte** eigenschappen **320** en **80** om ervoor te zorgen dat het tekstveld en in het invoerveld niet overlappen.
* Klik op de **maken** aan de bovenkant van het venster van de hiërarchie opnieuw en selecteer **Audio** > **audiobron** te maken van een audio-bron.

Als u hiermee klaar bent, ziet de gebruikersinterface er ongeveer als volgt uit:

[![Schermopname van de quickstartgebruikersinterface in de Unity-editor](media/sdk/qs-tts-csharp-unity-ui-inline.png)](media/sdk/qs-tts-csharp-unity-ui-expanded.png#lightbox)

## <a name="add-the-sample-code"></a>De voorbeeldcode toevoegen

1. Klik in het projectvenster [Project Window](https://docs.unity3d.com/Manual/ProjectView.html) (standaard linksonder) op de knop **Create** en selecteer vervolgens  **C# script**. Naam van het script `HelloWorld`.

1. Open het script voor bewerking door erop te dubbelklikken.

   > [!NOTE]
   > U kunt configureren welke code-editor wordt gestart onder **Edit** > **Preferences**. Zie de [gebruikershandleiding van Unity](https://docs.unity3d.com/Manual/Preferences.html).

1. Vervang alle code door deze code:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-unity/Assets/Scripts/HelloWorld.cs#code)]

1. Zoek en vervang de tekenreeks `YourSubscriptionKey` met uw abonnementssleutel Speech Services.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld. Als u bijvoorbeeld gebruikmaakt van de gratis proefversie, dan is `westus` de regio.

1. Sla het gewijzigde script op.

1. In de Unity-editor moet het script als onderdeel worden toegevoegd aan een van uw game-objecten.

   * Klik op het object **Canvas** in het venster Hierarchy. Hiermee opent u de instelling in de [Inspector-venster](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standaard aan de rechterkant).
   * Klik op de knop **Add Component** in het venster Inspector Window, zoek het HelloWorld-script dat we hierboven hebben gemaakt en voeg het toe.
   * Houd er rekening mee dat het ' Hallo wereld '-onderdeel vier eigenschappen van niet-geïnitialiseerde heeft **uitvoertekst**, **invoer veld**, **knop spreken** en **Audio bron**, die overeenkomen met de openbare eigenschappen van de `HelloWorld` klasse.
     Om deze te koppelen, klikt u op Object Picker (het pictogram van een cirkeltje, rechts van de eigenschap) en kiest u de objecten die u eerder hebt gemaakt: Text en Button.

     > [!NOTE]
     > Het invoerveld en de knop hebt ook een object met geneste tekst. Zorg ervoor dat u deze niet per ongeluk kiezen voor tekst uitvoer (of wijzig de naam van de SMS-objecten met behulp van het veld in het venster Inspector om verwarring te voorkomen dat).

## <a name="run-the-application-in-the-unity-editor"></a>De toepassing uitvoeren in de Unity-editor

* Druk op de knop **Play** op de werkbalk van de Unity-editor (onder de menubalk).

* Nadat de app wordt gestart, voer tekst in het invoerveld en klik op de knop. De tekst is verzonden naar de Services spraak en gemaakt naar-spraak, die op de spreker afgespeeld.

  [![Schermopname van de actieve quickstart in het Unity Game-venster](media/sdk/qs-tts-csharp-unity-output-inline.png)](media/sdk/qs-tts-csharp-unity-output-expanded.png#lightbox)

* Controleer het consolevenster [Console Window](https://docs.unity3d.com/Manual/Console.html) op foutopsporingsberichten.

* Wanneer u klaar bent met het stof voor de synthese spraak, klikt u op de **afspelen** knop in de werkbalk Unity-Editor om te stoppen van de app.

## <a name="additional-options-to-run-this-application"></a>Extra opties voor het uitvoeren van deze toepassing

Deze toepassing kan ook worden geïmplementeerd onder Android, als een zelfstandige Windows-app of UWP-toepassing.
Raadpleeg onze [voorbeeldopslagplaats](https://aka.ms/csspeech/samples) in de map quickstart/csharp-unity, waarin de configuratie voor deze aanvullende doelen wordt beschreven.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Spraakstijlen aanpassen](how-to-customize-voice-font.md)
- [Record voice-voorbeelden](record-custom-voice-samples.md)
