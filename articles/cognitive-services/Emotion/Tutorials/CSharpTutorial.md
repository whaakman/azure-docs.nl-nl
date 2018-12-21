---
title: 'Zelfstudie: Emoties op een gezicht in een afbeelding herkennen - Emotion-API, C#'
titlesuffix: Azure Cognitive Services
description: Leer hoe u met een eenvoudige Windows-app de emoties kunt herkennen die worden uitgedrukt door gezichten in een afbeelding.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: tutorial
ms.date: 01/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: da605ec4013fb11606f99f3d9a2dcfcfcab00d3b
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53163324"
---
# <a name="tutorial-recognize-emotions-on-a-face-in-an-image"></a>Zelfstudie: Emoties op een gezicht in een afbeelding herkennen.

> [!IMPORTANT]
> De Emotion-API wordt op 15 februari 2019 afgeschaft. De mogelijkheid voor de herkenning van emoties is nu algemeen beschikbaar als onderdeel van de [Face-API](https://docs.microsoft.com/azure/cognitive-services/face/). 

Leer hoe u met een eenvoudige Windows-toepassing en de Emotion-API de emoties kunt herkennen die worden uitgedrukt door gezichten in een afbeelding. Met het onderstaande voorbeeld kunt u een afbeeldings-URL of een lokaal opgeslagen bestand verzenden voor analyse. U kunt dit open-sourcevoorbeeld gebruiken als een sjabloon voor het bouwen van uw eigen app voor Windows met behulp van de Emotion-API en WPF (Windows Presentation Foundation), een onderdeel van .NET Framework.

## <a name="Prerequisites">Vereisten</a>
#### <a name="platform-requirements"></a>Platformvereisten  
Het voorbeeld hieronder is ontwikkeld voor het .NET Framework met behulp van [Visual Studio 2015, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs).  

#### <a name="subscribe-to-emotion-api-and-get-a-subscription-key"></a>Abonneren op Emotion-API en een abonnementssleutel ophalen  
Voordat u het voorbeeld maakt, moet u zich abonneren op Emotion-API, die deel uitmaakt van Microsoft Cognitive Services. Zie [Abonnementen](https://azure.microsoft.com/try/cognitive-services/) voor meer informatie. In deze zelfstudie kan zowel de primaire als de secundaire sleutel worden gebruikt. Zorg ervoor dat u de aanbevolen procedures hanteert voor het beveiligen en geheim houden van uw API-sleutel.  

#### <a name="get-the-client-library-and-example"></a>De clientbibliotheek en het voorbeeld downloaden  
U kunt de clientbibliotheek van de Emotion-API downloaden via de [SDK](https://www.github.com/microsoft/cognitive-emotion-windows). Het gedownloade ZIP-bestand moet worden uitgepakt naar een map van uw keuze. Veel gebruikers kiezen hiervoor de map Visual Studio 2015.
## <a name="Step1">Stap 1: Het voorbeeld openen</a>
1.  Start Microsoft Visual Studio 2015, klik op **File** en selecteer achtereenvolgens **Open** en **Project/Solution**.
2.  Blader naar de map waar u de gedownloade bestanden van de Emotion-API hebt opgeslagen. Klik achtereenvolgens op **Emotion**, **Windows** en de map **Sample-WPF**.
3.  Dubbelklik om het Visual Studio 2015-oplossingsbestand (.sln) met de naam **EmotionAPI-WPF-Samples.sln** te openen. De oplossing wordt nu geopend in Visual Studio.

## <a name="Step2">Stap 2: Het voorbeeld bouwen</a>
1. Klik in **Solution Explorer** met de rechtermuisknop op **References** en selecteer **Manage NuGet Packages**.

  ![NuGet Package Manager openen](../Images/EmotionNuget.png)

2.  Het venster **NuGet Package Manager** wordt geopend. Selecteer **Browse** in de linkerbovenhoek, typ 'newtonsoft.json' in het zoekvak, selecteer het pakket **Newtonsoft.Json** en klik op **Install**.  

  ![Bladeren naar het NuGet-pakket](../Images/EmotionNugetBrowse.png)  

3.  Druk op Ctrl+Shift+B of klik op **Build** in het lintmenu en selecteer vervolgens **Build Solution**.

## <a name="Step3">Stap 3: Het voorbeeld uitvoeren</a>
1.  Als het compileren is voltooid, drukt u op **F5** of klikt u op **Start** op het lintmenu om het voorbeeld uit te voeren.
2.  Zoek het venster van de Emotion-API met het **tekstvak** '**Paste your subscription key here to start**'. Plak uw abonnementssleutel in het tekstvak zoals wordt weergegeven in de schermafbeelding hieronder. U kunt ervoor kiezen om de abonnementssleutel op te slaan op uw pc of laptop door op de knop Save Key te klikken. Als u de abonnementssleutel wilt verwijderen van uw computer, klikt u op Delete Key.

  ![Interface van Emotion-API](../Images/EmotionKey.png)

3.  Klik onder **Select a scenario** op het scenario dat u wilt gebruiken, **Detect emotion using a stream** of **Detect emotion using a URL**, en volg de instructies op het scherm. Microsoft ontvangt de afbeeldingen die u uploadt en kan deze gebruiken om de Emotion-API en gerelateerde services te verbeteren. Door het indienen van een afbeelding bevestigt u dat u zich hebt gehouden aan onze [gedragscode voor ontwikkelaars](https://azure.microsoft.com/support/legal/developer-code-of-conduct/).
4.  Er zijn voorbeeldafbeeldingen beschikbaar voor gebruik met deze voorbeeldtoepassing. U vindt deze afbeeldingen in de [GitHub-opslagplaats Face API](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data), onder de map **Data**. Het gebruik van deze afbeeldingen is in licentie gegeven onder de Fair Use-overeenkomst, wat betekent dat u ze mag gebruiken voor het testen van dit voorbeeld, maar niet voor herpublicatie.

## <a name="Review">Bekijken en leren</a>
Nu u een werkende toepassing hebt, gaan we kijken hoe deze voorbeeld-app kan worden geïntegreerd met Microsoft Cognitive Services. Dit maakt het eenvoudiger om deze app verder te ontwikkelen of om uw eigen app te bouwen met de Emotion-API van Microsoft.

Deze voorbeeld-app maakt gebruik van de clientbibliotheek van de Emotion-API, een thin C#-client-wrapper voor de Emotion-API van Microsoft. Wanneer u de voorbeeld-app volgens de bovenstaande instructies hebt gebouwd, hebt u de clientbibliotheek al ontvangen via het NuGet-pakket. U kunt de broncode van de clientbibliotheek bekijken in de map met de naam [Client Library](https://github.com/Microsoft/Cognitive-Emotion-Windows/tree/master/ClientLibrary), onder **Emotion**, **Windows**, **Client Library**, die deel uitmaakt van de gedownloade opslagplaats voor bestanden die hierboven wordt genoemd in [Vereisten](#Prerequisites).

In **Solution Explorer** vindt u ook informatie voor het werken met de code van de clientbibliotheek: Vouw onder **EmotionAPI WPF_Samples** het onderdeel **DetectEmotionUsingStreamPage.xaml** uit en zoek **DetectEmotionUsingStreamPage.xaml.cs**. Dit bestand wordt gebruikt om te bladeren naar een lokaal opgeslagen bestand. U kunt ook **DetectEmotionUsingURLPage.xaml** uitvouwen om naar  **DetectEmotionUsingURLPage.xaml.cs** te gaan. Dit bestand wordt gebruikt bij het uploaden van een afbeeldings-URL. Dubbelklik op de. xaml.cs-bestanden om ze te openen in nieuwe vensters in Visual Studio.

Om te zien hoe de clientbibliotheek van de Emotion-API wordt gebruikt in onze voorbeeld-app, bekijken we twee codefragmenten uit **DetectEmotionUsingStreamPage.xaml.cs** en **DetectEmotionUsingURLPage.xaml.cs**. Beide bestanden bevatten de codeopmerkingen 'KEY SAMPLE CODE STARTS HERE' en 'KEY SAMPLE CODE ENDS HERE' om u te helpen de codefragmenten te vinden die u hieronder ziet.

De Emotion-API kan werken met een afbeeldings-URL of binaire afbeeldingsgegevens (in de vorm van een octet-stream) als invoer. De twee opties worden hieronder beschreven. In beide gevallen moet u eerst een using-instructie vinden, waarmee u de Emotion-clientbibliotheek kunt gebruiken.
```csharp

            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Use the following namespace for EmotionServiceClient
            // -----------------------------------------------------------------------
            using Microsoft.ProjectOxford.Emotion;
            using Microsoft.ProjectOxford.Emotion.Contract;
            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE
            // -----------------------------------------------------------------------
```
#### <a name="detectemotionusingurlpagexamlcs"></a>DetectEmotionUsingURLPage.xaml.cs

Dit codefragment laat zien hoe u de clientbibliotheek gebruikt voor het verzenden van uw abonnementssleutel en de URL van een foto naar de Emotion-API-service.

```csharp

            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // -----------------------------------------------------------------------

            window.Log("EmotionServiceClient is created");

            //
            // Create Project Oxford Emotion API Service client
            //
            EmotionServiceClient emotionServiceClient = new EmotionServiceClient(subscriptionKey);

            window.Log("Calling EmotionServiceClient.RecognizeAsync()...");
            try
            {
                //
                // Detect the emotions in the URL
                //
                Emotion[] emotionResult = await emotionServiceClient.RecognizeAsync(url);
                return emotionResult;
            }
            catch (Exception exception)
            {
                window.Log("Detection failed. Please make sure that you have the right subscription key and proper URL to detect.");
                window.Log(exception.ToString());
                return null;
            }
            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE
            // -----------------------------------------------------------------------
```
#### <a name="detectemotionusingstreampagexamlcs"></a>DetectEmotionUsingStreamPage.xaml.cs

Hieronder ziet u hoe u uw abonnementssleutel en een lokaal opgeslagen afbeelding naar de Emotion-API verzendt.


```csharp


            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // -----------------------------------------------------------------------

            //
            // Create Project Oxford Emotion API Service client
            //
            EmotionServiceClient emotionServiceClient = new EmotionServiceClient(subscriptionKey);

            window.Log("Calling EmotionServiceClient.RecognizeAsync()...");
            try
            {
                Emotion[] emotionResult;
                using (Stream imageFileStream = File.OpenRead(imageFilePath))
                {
                    //
                    // Detect the emotions in the URL
                    //
                    emotionResult = await emotionServiceClient.RecognizeAsync(imageFileStream);
                    return emotionResult;
                }
            }
            catch (Exception exception)
            {
                window.Log(exception.ToString());
                return null;
            }
            // -----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE
            // -----------------------------------------------------------------------
```
<!--
## <a name="Related">Related Topics</a>
[Emotion API Overview](.)
-->
