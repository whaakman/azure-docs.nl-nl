---
title: 'Quickstart: Spraak herkennen, Unity - Speech Services'
titleSuffix: Azure Cognitive Services
description: Gebruik deze handleiding om een toepassing voor spraak-naar-tekst te maken met Unity en de Speech-SDK voor Unity (bètaversie). Als u klaar bent, kunt u de microfoon van de computer gebruiken om in realtime spraak naar tekst te transcriberen.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 2/20/2019
ms.author: wolfma
ms.openlocfilehash: cdde9f0ec69bec48ae0fb747db0cc49e81920817
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57872557"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-unity-beta"></a>Quickstart: Spraak herkennen met de Speech-SDK voor Unity (bètaversie)

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Gebruik deze handleiding om een toepassing voor spraak-naar-tekst te maken met [Unity](https://unity3d.com/) en de Speech-SDK voor Unity (bètaversie).
Als u klaar bent, kunt u de microfoon van de computer gebruiken om in realtime spraak naar tekst te transcriberen.
Als u niet bekend met Unity bent, is het raadzaam eerst de [gebruikershandleiding van Unity](https://docs.unity3d.com/Manual/UnityManual.html) te bestuderen voordat u begint met de ontwikkeling van uw toepassing.

> [!NOTE]
> De Speech-SDK voor Unity is momenteel alleen beschikbaar als bètaversie.
> Deze biedt ondersteuning voor Windows x86 en x64 (zelfstandige bureaubladtoepassing of universeel Windows-platform) en Android (ARM32/64, x86).

## <a name="prerequisites"></a>Vereisten

Als u dit project wilt voltooien, hebt u het volgende nodig:

* [Unity 2018.3 of hoger](https://store.unity.com/)
* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Een abonnementssleutel voor de Speech Service. [Gratis downloaden](get-started.md).
* Toegang tot de microfoon van uw computer.

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

We voegen een minimale gebruikersinterface aan onze scène toe, die bestaat uit een knop om spraakherkenning te activeren en een tekstveld om het resultaat weer te geven.

* In [Hierarchy Window](https://docs.unity3d.com/Manual/Hierarchy.html) (standaard aan de linkerkant), wordt een voorbeeldscène weergegeven die in Unity bij het nieuwe project is gemaakt.
* Klik op de **maken** aan de bovenkant van de hiërarchie-venster en selecteer **UI** > **knop**.
* Hiermee maakt u drie game-objecten die u kunt zien in het venster Hierarchy: het object **Button**, genest in het object **Canvas**, en het object **EventSystem**.
* [De weergave scène navigeren](https://docs.unity3d.com/Manual/SceneViewNavigation.html) , zodat u beschikt over een goed beeld van het canvas en de knop in de [scène weergave](https://docs.unity3d.com/Manual/UsingTheSceneView.html).
* Klik op het object **Button** in het venster Hierarchy om de bijbehorende instellingen weer te geven in het inspectievenster [Inspector Window](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standaard aan de rechterkant).
* Stel de eigenschappen **Pos X** en **Pos Y** in op **0**, zodat de knop midden in het canvas wordt gecentreerd.
* Klik op de **maken** aan de bovenkant van het venster van de hiërarchie opnieuw en selecteer **UI** > **tekst** te maken van een tekstveld.
* Klik op het object **Text** in het venster Hierarchy om de bijbehorende instellingen weer te geven in het inspectievenster [Inspector Window](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standaard aan de rechterkant).
* Stel de eigenschappen **Pos X** en **Pos Y** in op **0** en **120**, en stel de eigenschappen **Width** en **Height** in op **240** en **120** zodat het tekstveld en de knop elkaar niet overlappen.

Als u hiermee klaar bent, ziet de gebruikersinterface er ongeveer als volgt uit:

[![Schermopname van de quickstartgebruikersinterface in de Unity-editor](media/sdk/qs-csharp-unity-02-ui-inline.png)](media/sdk/qs-csharp-unity-02-ui-expanded.png#lightbox)

## <a name="add-the-sample-code"></a>De voorbeeldcode toevoegen

1. Klik in het projectvenster [Project Window](https://docs.unity3d.com/Manual/ProjectView.html) (standaard linksonder) op de knop **Create** en selecteer vervolgens  **C# script**. Naam van het script `HelloWorld`.

1. Open het script voor bewerking door erop te dubbelklikken.

   > [!NOTE]
   > U kunt configureren welke code-editor wordt gestart onder **Edit** > **Preferences**. Zie de [gebruikershandleiding van Unity](https://docs.unity3d.com/Manual/Preferences.html).

1. Vervang alle code door deze code:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/csharp-unity/Assets/Scripts/HelloWorld.cs#code)]

1. Zoek en vervang de tekenreeks `YourSubscriptionKey` met uw abonnementssleutel Speech Services.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die aan uw abonnement is gekoppeld. Als u bijvoorbeeld gebruikmaakt van de gratis proefversie, dan is `westus` de regio.

1. Sla het gewijzigde script op.

1. In de Unity-editor moet het script als onderdeel worden toegevoegd aan een van uw game-objecten.

   * Klik op het object **Canvas** in het venster Hierarchy. Hiermee opent u de instelling in de [Inspector-venster](https://docs.unity3d.com/Manual/UsingTheInspector.html) (standaard aan de rechterkant).
   * Klik op de knop **Add Component** in het venster Inspector Window, zoek het HelloWorld-script dat we hierboven hebben gemaakt en voeg het toe.
   * Het 'Hallo wereld'-onderdeel heeft twee niet-geïnitialiseerde eigenschappen, **Output Text** en **Start Reco Button**, die overeenkomen met de openbare eigenschappen van de klasse `HelloWorld`.
     Om deze te koppelen, klikt u op Object Picker (het pictogram van een cirkeltje, rechts van de eigenschap) en kiest u de objecten die u eerder hebt gemaakt: Text en Button.

     > [!NOTE]
     > De knop heeft ook een genest Text-object. Zorg ervoor dat u dit niet per ongeluk kiest voor tekstuitvoer (of de naam van een van de Text-objecten wijzigt met behulp van het veld Name in het venster Inspector Window om verwarring te voorkomen).

## <a name="run-the-application-in-the-unity-editor"></a>De toepassing uitvoeren in de Unity-editor

* Druk op de knop **Play** op de werkbalk van de Unity-editor (onder de menubalk).

* Nadat de app is gestart, klikt u op de knop en zegt u iets in het Engels in de microfoon van uw computer. Uw stem is verzonden naar de Services voor spraak en getranscribeerde tekst, die in het venster wordt weergegeven.

  [![Schermopname van de actieve quickstart in het Unity Game-venster](media/sdk/qs-csharp-unity-03-output-inline.png)](media/sdk/qs-csharp-unity-03-output-expanded.png#lightbox)

* Controleer het consolevenster [Console Window](https://docs.unity3d.com/Manual/Console.html) op foutopsporingsberichten.

* Wanneer u klaar bent met de spraakherkenning, klikt u op de knop **Play** op de werkbalk van de Unity-editor om de app te stoppen.

## <a name="additional-options-to-run-this-application"></a>Extra opties voor het uitvoeren van deze toepassing

Deze toepassing kan ook worden geïmplementeerd onder Android, als een zelfstandige Windows-app of UWP-toepassing.
Raadpleeg onze [voorbeeldopslagplaats](https://aka.ms/csspeech/samples) in de map quickstart/csharp-unity, waarin de configuratie voor deze aanvullende doelen wordt beschreven.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
