---
title: Computer Vision -API C#-zelfstudie | Microsoft Docs
description: Gebruik een eenvoudige Windows-app die gebruikmaakt van de Computer Vision-API in cognitieve Microsoft-Services. OCR uitvoeren, miniaturen en werken met de visual functies in een afbeelding.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 05/22/2017
ms.author: kefre
ms.openlocfilehash: f2aeb1734f8858ed8b80e5cdf48ef7e558703919
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344911"
---
# <a name="computer-vision-api-c35-tutorial"></a>Computer Vision-API C&#35; zelfstudie

Gebruik een eenvoudige Windows-toepassing die gebruikmaakt van de Computer Vision-API om (optische tekenherkenning), smartcards bijgesneden miniaturen maken plus detecteren, te categoriseren, label en beschrijven visual functies, zoals vlakken, in een afbeelding. Het onderstaande voorbeeld kunt u een afbeeldings-URL of een lokaal bestand verzenden. U kunt in dit voorbeeld open-source als sjabloon gebruiken voor het bouwen van uw eigen app voor Windows met behulp van de Vision-API en WPF (Windows Presentation Foundation), een onderdeel van .NET Framework.

### <a name="prerequisites"></a>Vereisten

#### <a name="platform-requirements"></a>Platformvereisten

Het onderstaande voorbeeld is ontwikkeld voor .NET Framework met [Visual Studio 2015, Community Edition](https://www.visualstudio.com/downloads/).

#### <a name="subscribe-to-computer-vision-api-and-get-a-subscription-key"></a>Abonneer u op de Computer Vision-API en de abonnementssleutel voor een ophalen 

Voordat u het voorbeeld maakt, moet u zich abonneren op de Computer Vision-API die deel uitmaakt van de cognitieve Microsoft-Services (voorheen Project Oxford). Zie voor abonnement en sleutelbeheer details [abonnementen](https://azure.microsoft.com/try/cognitive-services/). Zowel de primaire en secundaire sleutel kan worden gebruikt in deze zelfstudie. 

> [!NOTE]
> De zelfstudie is ontworpen voor gebruik van abonnement sleutels in de **westcentralus** regio. Het abonnement sleutels die zijn gegenereerd in het gebruik van de gratis audittrail Computer Vision de **westcentralus** regio, zodat ze goed werken. Als u de sleutels van uw abonnement met uw Azure-account via gegenereerd [ https://azure.microsoft.com/ ](https://azure.microsoft.com/), moet u de **westcentralus** regio. Sleutels die zijn gegenereerd buiten de **westcentralus** regio werkt niet.

#### <a name="get-the-client-library-and-example"></a>Ophalen van de client-bibliotheek en voorbeeld

U kunt de Computer Vision-API-bibliotheek en voorbeeld-clienttoepassing op uw computer via klonen [SDK](https://www.github.com/microsoft/cognitive-vision-windows). Download het als een ZIP.

### <a name="Step1">Stap 1: Installeer het voorbeeld</a>

Open de voorbeeld-WPF\VisionAPI-WPF-Samples.sln in uw bureaublad GitHub.

### <a name="Step2">Stap 2: Het voorbeeld bouwen</a>

* Druk op Ctrl + Shift + B of Build Klik op het lintmenu en selecteer vervolgens oplossing bouwen.

### <a name="Step3">Stap 3: Voer in het voorbeeld</a>

1. Nadat de build voltooid is, drukt u op **F5** of klik op **Start** in het lintmenu om het voorbeeld uitvoert.
2. Zoek het venster Computer Vision-API gebruikersinterface met de tekstinvoervak lezen 'Uw subscription-key plakken om te starten'.
U kunt kiezen om vast te leggen van de abonnementssleutel van uw op uw PC of laptop door te klikken op de knop 'Sleutel opslaan'. Als u wilt dat de abonnementssleutel verwijderen uit het systeem, klikt u op 'Sleutel verwijderen' om het te verwijderen van uw PC of laptop.

    ![Visie Abonnementssleutel](../Images/Vision_UI_Subscription.PNG)

3. Onder 'Selecteer Scenario' een van de zes scenario's gebruiken en klik Volg de instructies op het scherm. Microsoft ontvangt de installatiekopieën die u uploaden en deze kan gebruiken om Computer Vision-API en verwante services te verbeteren. Door het indienen van een installatiekopie van een u bevestigen dat u hebt gevolgd onze [Developer gedragscode](https://azure.microsoft.com/support/legal/developer-code-of-conduct/).

    ![Afbeelding Interface analyseren](../Images/Analyze_Image_Example.PNG)

4. Er zijn installatiekopieën van het voorbeeld moet worden gebruikt met de voorbeeldtoepassing van dit. U kunt deze installatiekopieën vinden op de Face-API Windows Github-repo, in de [gegevensmap](https://github.com/Microsoft/Cognitive-Face-Windows/tree/master/Data). Houd rekening met het gebruik van deze installatiekopieën is gelicentieerd onder overeenkomst [licentie-IMAGE](https://github.com/Microsoft/Cognitive-Face-Windows/blob/master/LICENSE-IMAGE.md).

### <a name="Review">Bekijk en meer informatie</a>

Nu dat u een actieve toepassing hebt, laat het ons Bekijk hoe deze voorbeeld-app kan worden geïntegreerd met cognitieve Services-technologie. Hiermee wordt het eenvoudiger te bouwen die naar deze app gaan of uw eigen app met behulp van Microsoft Computer Vision-API ontwikkelen.

Deze voorbeeld-app wordt gebruikgemaakt van de Computer Vision-API-clientbibliotheek een thin C#-client-wrapper voor de Computer Vision-API van Microsoft. Wanneer u de voorbeeld-app zoals hierboven beschreven gebouwd, kunt u de clientbibliotheek verkregen uit een NuGet-pakket. U kunt de broncode van de clientbibliotheek in de map met de titel bekijken '**clientbibliotheek**' onder **visie**, **Windows**, **clientbibliotheek**, Dit maakt deel uit van de gedownloade bestandsopslagplaats hierboven vermeld in de vereisten.

U vindt hier ook het gebruik van de clientbibliotheek code in Solution Explorer: onder **VisionAPI WPF_Samples**, vouw **AnalyzePage.xaml** vinden **AnalyzePage.xaml.cs**, die wordt gebruikt voor het indienen van een installatiekopie van het eindpunt van de installatiekopie-analyse. Dubbelklik op de. xaml.cs bestanden te laten openen in nieuw windows in Visual Studio.

Bekeken hoe de visie-clientbibliotheek in ons voorbeeld-app wordt gebruikt, bekijken we twee codefragmenten van **AnalyzePage.xaml.cs**. Het bestand bevat codeopmerkingen die aangeeft 'Sleutel voorbeeld CODE BEGINT hier' en 'Sleutel voorbeeld CODE EINDIGT hier' om te vinden van de code codefragmenten hieronder.

Het eindpunt analyseren is kunnen werken met een afbeeldings-URL of binaire gegevens (in de vorm van een octet-stream) als invoer. Eerst vindt u een met Richtlijn, waarmee u de visie-clientbibliotheek gebruiken.

```
                // ----------------------------------------------------------------------
                // KEY SAMPLE CODE STARTS HERE
                // Use the following namespace for VisionServiceClient 
                // ---------------------------------------------------------------------- 
                using Microsoft.ProjectOxford.Vision; 
                using Microsoft.ProjectOxford.Vision.Contract; 
                // ----------------------------------------------------------------------
                // KEY SAMPLE CODE ENDS HERE 
                // ----------------------------------------------------------------------

```
**UploadAndAnalyzeImage(...)**  Dit codefragment ziet u hoe de clientbibliotheek gebruikt voor het verzenden van de abonnementssleutel van uw en een lokaal opgeslagen installatiekopie van het eindpunt van het analyseren van de Computer Vision-API-service.

```
    private async Task<AnalysisResult> UploadAndAnalyzeImage(string imageFilePath)
    {
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE STARTS HERE
        // -----------------------------------------------------------------------  
        //
        // Create Project Oxford Computer Vision API Service client
        //
        VisionServiceClient VisionServiceClient = new VisionServiceClient(SubscriptionKey);
        Log("VisionServiceClient is created");
    
        using (Stream imageFileStream = File.OpenRead(imageFilePath))
        {
            //
            // Analyze the image for all visual features
            //
            Log("Calling VisionServiceClient.AnalyzeImageAsync()...");
         VisualFeature[] visualFeatures = new VisualFeature[] { VisualFeature.Adult, VisualFeature.Categories, VisualFeature.Color, VisualFeature.Description, VisualFeature.Faces, VisualFeature.ImageType, VisualFeature.Tags };
            AnalysisResult analysisResult = await VisionServiceClient.AnalyzeImageAsync(imageFileStream, visualFeatures);
            return analysisResult;
        }
    
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE ENDS HERE
        // -----------------------------------------------------------------------
        }
```
**AnalyzeUrl(...)**  Dit codefragment ziet u hoe de clientbibliotheek gebruikt voor het verzenden van de abonnementssleutel van uw en een foto-URL naar het eindpunt van het analyseren van de Computer Vision-API-service.

```
    private async Task<AnalysisResult> AnalyzeUrl(string imageUrl)
    {
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE STARTS HERE
        // -----------------------------------------------------------------------
    
        //
        // Create Project Oxford Computer Vision API Service client
        //
     VisionServiceClient VisionServiceClient = new VisionServiceClient(SubscriptionKey);
        Log("VisionServiceClient is created");
    
        //
        // Analyze the url for all visual features
        //
        Log("Calling VisionServiceClient.AnalyzeImageAsync()...");
        VisualFeature[] visualFeatures = new VisualFeature[] { VisualFeature.Adult, VisualFeature.Categories, VisualFeature.Color, VisualFeature.Description, VisualFeature.Faces, VisualFeature.ImageType, VisualFeature.Tags };
        AnalysisResult analysisResult = await VisionServiceClient.AnalyzeImageAsync(imageUrl, visualFeatures);
     return analysisResult;
    }
        // -----------------------------------------------------------------------
        // KEY SAMPLE CODE ENDS HERE
        // -----------------------------------------------------------------------
```
**Andere pagina's en de eindpunten** interactie met de andere eindpunten die worden weergegeven door de Computer Vision-API-service kan worden gezien door te kijken naar de andere pagina's in de steekproef; bijvoorbeeld, het eindpunt OCR wordt weergegeven als onderdeel van de programmacode in OCRPage.xaml.cs 

### <a name="Related">Verwante onderwerpen</a>
 * [Aan de slag met Face-API](../../Face/Tutorials/FaceAPIinCSharpTutorial.md)
 
 


