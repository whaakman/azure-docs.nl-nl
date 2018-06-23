---
title: Emotion -API C#-zelfstudie | Microsoft Docs
description: Gebruik een eenvoudige Windows-app die gebruikmaakt van de cognitieve Services Emotion-API voor het herkennen van de emoties uitgedrukt in vlakken in een afbeelding.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 01/23/2017
ms.author: anroth
ms.openlocfilehash: f015e5720f65d0951a77de76ce8882a6dcdc1c3b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344767"
---
# <a name="emotion-api-in-c35-tutorial"></a>Emotion-API in C&#35; zelfstudie

> [!IMPORTANT]
> Video API Preview eindigt op 30 oktober 2017. Het nieuwe [Video indexeerfunctie API Preview](https://azure.microsoft.com/services/cognitive-services/video-indexer/) eenvoudig inzichten extraheren van video's en ervaringen van inhoud, zoals de lijst met zoekresultaten verbeteren doordat gesproken woorden, vlakken tekens en emoties. [Meer informatie](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Gebruik een eenvoudige Windows-toepassing die gebruikmaakt van Emotion-API voor het herkennen van de emoties uitgedrukt in de vlakken in een afbeelding. Het onderstaande voorbeeld kunt u een afbeeldings-URL of een lokaal bestand verzenden. U kunt in dit voorbeeld open-source als sjabloon gebruiken voor het bouwen van uw eigen app voor Windows met behulp van de Emotion-API en WPF (Windows Presentation Foundation), een onderdeel van .NET Framework.

## <a name="Prerequisites">Vereisten</a>
#### <a name="platform-requirements"></a>Platformvereisten  
Het onderstaande voorbeeld is ontwikkeld voor .NET Framework met [Visual Studio 2015, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs).  

#### <a name="subscribe-to-emotion-api-and-get-a-subscription-key"></a>Abonneer u op Emotion-API en de abonnementssleutel voor een ophalen  
Voordat u het voorbeeld maakt, moet u zich abonneert op Emotion-API die deel uitmaakt van de cognitieve Microsoft-Services. Zie [abonnementen](https://azure.microsoft.com/try/cognitive-services/). Zowel de primaire en secundaire sleutel kan worden gebruikt in deze zelfstudie. Zorg ervoor dat u Volg de aanbevolen procedures voor het bewaren van uw API-sleutel geheim en beveiligen.  

#### <a name="get-the-client-library-and-example"></a>Ophalen van de client-bibliotheek en voorbeeld  
U kunt downloaden via de Emotion-API-clientbibliotheek [SDK](https://www.github.com/microsoft/cognitive-emotion-windows). Het gedownloade zip-bestand moet worden uitgepakt naar een map van uw keuze, veel gebruikers selecteert u de map Visual Studio 2015.
## <a name="Step1">Stap 1: Open in het voorbeeld</a>
1.  Start Microsoft Visual Studio 2015 en op **bestand**, selecteer **Open**, klikt u vervolgens **Project/oplossing**.
2.  Blader naar de map waar u de gedownloade Emotion-API-bestanden opgeslagen. Klik op **Emotion**, klikt u vervolgens **Windows**, en vervolgens de **voorbeeld WPF** map.
3.  Dubbelklik om te openen van de Visual Studio 2015-oplossing (.sln)-bestand met de naam **EmotionAPI-WPF-Samples.sln**. Hiermee opent u de oplossing in Visual Studio.

## <a name="Step2">Stap 2: Het voorbeeld bouwen</a>
1. In **Solution Explorer**, klik met de rechtermuisknop **verwijzingen** en selecteer **NuGet-pakketten beheren**.

  ![Open Nuget Package Manager](../Images/EmotionNuget.png)

2.  De **NuGet Package Manager** venster wordt geopend. Selecteer eerst **Bladeren** in de linkerbovenhoek en vervolgens de Typ in het zoekvak 'Newtonsoft.Json', selecteer de **Newtonsoft.Json** van het pakket en klikt u op **installeren**.  

  ![Blader naar de NuGet-pakket](../Images/EmotionNugetBrowse.png)  

3.  Druk op Ctrl + Shift + B, of klik op **bouwen** in het lintmenu Selecteer **Build Solution**.

## <a name="Step3">Stap 3: Voer in het voorbeeld</a>
1.  Nadat de build voltooid is, drukt u op **F5** of klik op **Start** in het lintmenu om het voorbeeld uitvoert.
2.  Zoek de Emotion-API-venster met de **tekstvak** lezen '**plak hier uw abonnementssleutel voor het starten van**'. De abonnementssleutel van uw in het tekstvak plakken, zoals weergegeven in onderstaande schermafbeelding. U kunt kiezen om vast te leggen van de abonnementssleutel van uw op uw PC of laptop door te klikken op de knop 'Sleutel opslaan'. Als u wilt dat de abonnementssleutel verwijderen uit het systeem, klikt u op 'Sleutel verwijderen' om het te verwijderen van uw PC of laptop.
  
  ![Emotion functionaliteit Interface](../Images/EmotionKey.png)

3.  Onder '**Selecteer Scenario**'Klik hier om te gebruiken op een van de twee scenario's,'**detecteren met behulp van een stroom emotion**"of"**detecteren met behulp van een URL emotion**', volg de instructies op het scherm. Microsoft ontvangt de installatiekopieën die u uploaden en deze kan gebruiken om Emotion-API en verwante services te verbeteren. Door het indienen van een installatiekopie van een u bevestigen dat u hebt gevolgd onze [Developer gedragscode](https://azure.microsoft.com/support/legal/developer-code-of-conduct/).
4.  Er zijn installatiekopieën van het voorbeeld moet worden gebruikt met de voorbeeldtoepassing van dit. U kunt deze installatiekopieën vinden op [de Face-API Github-repo](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data) onder de **gegevens** map. Houd rekening met dat het gebruik van deze installatiekopieën is een licentie verleend onder evenredige gebruiksrechtovereenkomst wat betekent dat ze zijn mogen worden gebruikt voor het testen van dit voorbeeld, maar niet voor het opnieuw te publiceren.

## <a name="Review">Bekijk en meer informatie</a>
Nu dat u een actieve toepassing hebt, laat het ons Bekijk hoe deze voorbeeld-app is geïntegreerd met cognitieve Services van Microsoft. Hiermee wordt het eenvoudiger te bouwen die naar deze app gaan of uw eigen app met behulp van Microsoft Emotion-API ontwikkelen. 

Deze voorbeeld-app wordt gebruikgemaakt van de clientbibliotheek Emotion-API, een thin C#-client-wrapper voor de Emotion-API van Microsoft. Wanneer u de voorbeeld-app zoals hierboven beschreven gebouwd, kunt u de clientbibliotheek verkregen uit een NuGet-pakket. U kunt de broncode van de clientbibliotheek in de map met de titel bekijken '[clientbibliotheek](https://github.com/Microsoft/Cognitive-Emotion-Windows/tree/master/ClientLibrary)' onder **Emotion**, **Windows**, **clientbibliotheek** , die deel uitmaken van de opslagplaats gedownloade bestand hierboven wordt genoemd [vereisten](#Prerequisites).
 
U vindt hier ook het gebruik van de clientbibliotheek code in **Solution Explorer**: onder **EmotionAPI WPF_Samples**, vouw **DetectEmotionUsingStreamPage.xaml** naar Zoek **DetectEmotionUsingStreamPage.xaml.cs**, die wordt gebruikt voor het bladeren naar een lokaal bestand of vouw **DetectEmotionUsingURLPage.xaml** vinden  **DetectEmotionUsingURLPage.xaml.cs**, die wordt gebruikt tijdens het uploaden van een afbeeldings-URL. Dubbelklik op de. xaml.cs bestanden te laten openen in nieuw windows in Visual Studio. 

Bekeken hoe de clientbibliotheek Emotion in ons voorbeeld-app wordt gebruikt, bekijken we twee codefragmenten van **DetectEmotionUsingStreamPage.xaml.cs** en **DetectEmotionUsingURLPage.xaml.cs**. Elk bestand bevat codeopmerkingen die aangeeft 'Sleutel voorbeeld CODE BEGINT hier' en 'Sleutel voorbeeld CODE EINDIGT hier' om te vinden van de code codefragmenten hieronder.

De Emotion-API is kunnen werken met een afbeeldings-URL of binaire gegevens (in de vorm van een octet-stream) als invoer. De twee opties worden hieronder beoordeeld. In beide gevallen moet u eerst vinden een met Richtlijn, waarmee u de Emotion-clientbibliotheek gebruiken. 
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

Dit codefragment laat zien hoe de clientbibliotheek gebruikt voor het verzenden van de abonnementssleutel van uw en een foto-URL naar de Emotion-API-service. 

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

Hieronder wordt weergegeven, is het indienen van de abonnementssleutel van uw en een lokaal opgeslagen installatiekopie aan de Emotion-API. 


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
