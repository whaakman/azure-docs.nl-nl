---
title: 'Zelfstudie: Een afbeelding verwerking app bouwen:C#'
titleSuffix: Computer Vision - Cognitive Services - Azure
description: Verken een eenvoudige Windows-app die gebruikmaakt van de Computer Vision-API in Microsoft Cognitive Services. Voer OCR uit, maak miniaturen en werk met visuele kenmerken in een afbeelding.
services: cognitive-services
author: PatrickFarley
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 08/28/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: f4f588ad9123ec98cbae9f9bf03c08ec551a31a5
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54232750"
---
# <a name="tutorial-build-an-image-processing-app---c35"></a>Zelfstudie: Bouw een installatiekopie voor het verwerken van app - C&#35;

Verken een eenvoudige Windows-toepassing die gebruikmaakt van de Computer Vision optische tekenherkenning (OCR) uitvoeren, smart bijgesneden miniaturen, plus detecteren, categoriseren, label en beschrijven visuele kenmerken, met inbegrip van gezichten, in een afbeelding. Met het onderstaande voorbeeld kunt u een afbeeldings-URL of een lokaal opgeslagen bestand verzenden voor analyse. U kunt dit open-source-voorbeeld gebruiken als een sjabloon voor het bouwen van uw eigen app voor Windows met behulp van de Computer Vision-API en Windows Presentation Foundation (WPF), een onderdeel van .NET Framework.

> [!div class="checklist"]
> * Ophalen van de voorbeeld-app vanuit GitHub
> * Open en bouwen van de voorbeeld-app in Visual Studio
> * De voorbeeld-app uitvoeren en communiceren met het uitvoeren van verschillende scenario 's
> * Verken de verschillende scenario's met de voorbeeld-app

## <a name="prerequisites"></a>Vereisten

Daarna wordt ingegaan op de voorbeeld-app, zorg ervoor dat u hebt voldaan aan de volgende vereisten:

* U hebt [Visual Studio 2015](https://visualstudio.microsoft.com/downloads/) of hoger nodig.
* U moet beschikken over een abonnementssleutel voor Computer Vision. Zie [Abonnementssleutels verkrijgen](../Vision-API-How-to-Topics/HowToSubscribe.md) voor meer informatie over het verkrijgen van een abonnementssleutel.

## <a name="get-the-sample-app"></a>De voorbeeld-app downloaden

De Computer Vision-voorbeeld-app is beschikbaar op GitHub uit de `Microsoft/Cognitive-Vision-Windows` opslagplaats. Deze opslagplaats bevat ook de `Microsoft/Cognitive-Common-Windows` opslagplaats bevindt als een submodule Git. U kunt recursief kloon deze opslagplaats, inclusief de submodule door de `git clone --recurse-submodules` opdracht vanaf de opdrachtregel of met behulp van GitHub Desktop.

Voor bijvoorbeeld naar recursief kloon de opslagplaats voor de Computer Vision-voorbeeld-app vanaf een opdrachtprompt, voert u de volgende opdracht uit:

```Console
git clone --recurse-submodules https://github.com/Microsoft/Cognitive-Vision-Windows.git
```

> [!IMPORTANT]
> Download deze opslagplaats niet als een POSTCODE. GIT geen submodules bij het downloaden van een als een ZIP-opslagplaats.

### <a name="get-optional-sample-images"></a>Optionele voorbeeldafbeeldingen ophalen

U kunt eventueel de voorbeeldafbeeldingen opgenomen in de [Face](../../Face/Overview.md) voorbeeld-app, beschikbaar op GitHub uit de `Microsoft/Cognitive-Face-Windows` opslagplaats. Deze voorbeeld-app bevat een map `/Data`, die meerdere installatiekopieën van mensen bevat. U kunt deze opslagplaats, ook recursief klonen door de methoden die zijn beschreven voor de Computer Vision-voorbeeld-app.

Voor bijvoorbeeld naar recursief kloon de opslagplaats voor de Face-voorbeeld-app vanaf een opdrachtprompt, voert u de volgende opdracht uit:

```Console
git clone --recurse-submodules https://github.com/Microsoft/Cognitive-Face-Windows.git
```

## <a name="open-and-build-the-sample-app-in-visual-studio"></a>Open en bouwen van de voorbeeld-app in Visual Studio

U moet eerst de voorbeeld-app bouwen zodat Visual Studio afhankelijkheden, oplossen kunt voordat u kunt uitvoeren of de voorbeeld-app verkennen. Om te openen en de voorbeeld-app bouwen, voer de volgende stappen uit:

1. Open het Visual Studio-oplossingsbestand `/Sample-WPF/VisionAPI-WPF-Samples.sln`, in Visual Studio.
1. Zorg ervoor dat de Visual Studio-oplossing twee projecten bevat:  

   * SampleUserControlLibrary
   * VisionAPI-WPF-voorbeelden  

   Als het project SampleUserControlLibrary niet beschikbaar is, controleert u of die u gekloond recursief hebt de `Microsoft/Cognitive-Vision-Windows` opslagplaats.
1. In Visual Studio, druk op Ctrl + Shift + B of kies **bouwen** in het lintmenu en kies vervolgens **Build Solution** om de oplossing te bouwen.

## <a name="run-and-interact-with-the-sample-app"></a>Uitvoeren en communiceren met de voorbeeld-app

U kunt de voorbeeld-app, om te zien hoe deze samenwerkt met u en de Computer Vision-clientbibliotheek bij het uitvoeren van verschillende taken, zoals het genereren van miniaturen of labels afbeeldingen uitvoeren. Als u wilt uitvoeren en communiceren met de voorbeeld-app, voer de volgende stappen uit:

1. Nadat de bewerking voltooid is, kunt u drukt u op **F5** of kies **Debug** in het lintmenu en kies vervolgens **foutopsporing starten** om uit te voeren van de voorbeeld-app.
1. Wanneer de voorbeeld-app wordt weergegeven, kiest u **abonnement Key Management** in het navigatiedeelvenster om de pagina beheer van abonnementen sleutel weer te geven.
   ![Abonnement Key Management-pagina](../Images/Vision_UI_Subscription.PNG)  
1. Geef uw abonnementssleutel in **Abonnementssleutel**.
1. De eindpunt-URL invoeren als u weglaat de `/vision/v1.0`, van de Computer Vision-resource voor uw abonnementssleutel in **eindpunt**.  
   Bijvoorbeeld, als u de abonnementssleutel van de Computer Vision-gratis proefversie gebruikt, voert u de volgende eindpunt-URL voor de regio West-Centraal VS Azure: `https://westcentralus.api.cognitive.microsoft.com`
1. Als u niet wilt dat uw abonnementssleutel en -eindpunt in te voeren URL de volgende keer dat u bij het uitvoeren van de voorbeeld-app, kies **opslaan** om op te slaan de abonnement-sleutel en het eindpunt-URL naar uw computer. Als u verwijderen van de sleutel en het eindpunt URL van uw abonnement eerder hebt opgeslagen wilt, kiest u **instelling verwijderen**.

   > [!NOTE]
   > De voorbeeld-app maakt gebruik van geïsoleerde opslag en `System.IO.IsolatedStorage`, voor het opslaan van uw abonnement-sleutel en het eindpunt-URL.

1. Onder **selecteert u een scenario** in het navigatiedeelvenster, selecteer een van de scenario's die momenteel is opgenomen in de voorbeeld-app:  

   | Scenario | Description |
   |----------|-------------|
   |Afbeelding analyseren | Maakt gebruik van de [analyseren installatiekopie](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) bewerking voor het analyseren van een lokale of externe-installatiekopie. U kunt kiezen voor de visuele kenmerken en de taal voor de analyse en zowel de installatiekopie als de resultaten zien.  |
   |Afbeelding met domeinmodel analyseren | Maakt gebruik van de [lijst domeinspecifieke modellen](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fd) bewerking om de domeinmodellen van waaruit u selecteren kunt, weer te geven en de [herkennen domein specifieke inhoud](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e200) bewerking voor het analyseren van een lokale of externe-installatiekopie met de model van het geselecteerde domein. U kunt ook de taal voor de analyse. |
   |Afbeelding beschrijven | Maakt gebruik van de [beschrijven installatiekopie](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fe) tijdens het maken van een leesbare beschrijving van de installatiekopie van een lokaal of extern. U kunt ook de taal voor de beschrijving. |
   |Codes genereren | Maakt gebruik van de [Tag Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1ff) bewerking voor het taggen van de visuele kenmerken van een lokale of externe-installatiekopie. U kunt ook de taal die wordt gebruikt voor de labels. |
   |OCR (OCR) | Maakt gebruik van de [OCR](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) bewerking te extraheren afgedrukt tekst uit een afbeelding. U kunt Kies de taal te gebruiken of u kunt Computer-Vision de taal automatisch te detecteren. |
   |Herkennen tekst V2 (Engels) | Maakt gebruik van de [tekst herkennen](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) en [ophalen herkennen tekst bewerkingsresultaat](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2cf1154055056008f201) bewerkingen moeten asynchroon herkennen en afgedrukt of handgeschreven tekst extraheren uit een afbeelding. |
   |Miniatuur ophalen | Maakt gebruik van de [halen miniatuur](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) bewerking voor het genereren van een miniatuur voor een lokale of externe-installatiekopie. |

   De volgende schermafbeelding ziet u de pagina die is opgegeven voor het scenario met afbeelding analyseren na het analyseren van een voorbeeldafbeelding.
   ![Schermafbeelding van de pagina van de installatiekopie analyseren](../Images/Analyze_Image_Example.PNG)

## <a name="explore-the-sample-app"></a>De voorbeeld-app verkennen

De Visual Studio-oplossing voor de Computer Vision-voorbeeld-app bevat twee projecten:

* SampleUserControlLibrary  
  Het project SampleUserControlLibrary biedt functionaliteit gedeeld door meerdere Cognitive Services-voorbeelden. Het project bevat het volgende:
  * SampleScenarios  
    Een UserControl waarmee een gestandaardiseerde presentatie, zoals de titelbalk, navigatievenster en inhoudsvenster voor voorbeelden. De Computer Vision-voorbeeld-app maakt gebruik van dit besturingselement in het venster MainWindow.xaml voor scenario's weergeven en toegang krijgen tot informatie die wordt gedeeld via scenario's, zoals de abonnement-sleutel en het eindpunt-URL.
  * SubscriptionKeyPage  
    Een pagina met een gestandaardiseerde indeling voor het invoeren van een abonnement-sleutel en het eindpunt-URL voor de voorbeeld-app. De Computer Vision-voorbeeld-app maakt gebruik van deze pagina om de abonnementssleutel te beheren en eindpunt-URL wordt gebruikt door de scenario's.
  * VideoResultControl  
    Een UserControl waarmee een gestandaardiseerde presentatie voor informatie over de video. De Computer Vision-voorbeeld-app gebruik niet van dit besturingselement.
* VisionAPI-WPF-voorbeelden  
  Het belangrijkste project voor de Computer Vision-voorbeeld-app, dit project bevat alle functionaliteit die interessante voor Computer Vision. Het project bevat het volgende:
  * AnalyzeInDomainPage.xaml  
    De scenario-pagina voor de installatiekopie van het analyseren met domeinmodel scenario.
  * AnalyzeImage.xaml  
    De scenario-pagina voor het analyseren van installatiekopie-scenario.
  * DescribePage.xaml  
    De scenario-pagina voor het scenario van de afbeelding wordt beschreven.
  * ImageScenarioPage.cs  
    De klasse ImageScenarioPage, waarvan alle scenario's in de voorbeeld-app zijn afgeleid. Deze klasse beheert functionaliteit, zoals referenties op te geven en opmaken van uitvoer, gedeeld door alle scenario's.
  * MainWindow.xaml  
    Het hoofdvenster van de voorbeeld-app, het maakt gebruik van het besturingselement SampleScenarios om weer te geven van de SubscriptionKeyPage en scenario's.
  * OCRPage.xaml  
    De scenario-pagina voor het scenario tekst herkennen (OCR).
  * RecognizeLanguage.cs  
    De klasse RecognizeLanguage, waarmee u informatie over de talen die worden ondersteund door de verschillende methoden in de voorbeeld-app.
  * TagsPage.xaml  
    De scenario-pagina voor het genereren van Tags-scenario.
  * TextRecognitionPage.xaml  
    De scenario-pagina voor het herkennen tekst V2 (Engels)-scenario.
  * ThumbnailPage.xaml  
    De scenario-pagina voor het scenario ophalen miniatuur.

### <a name="explore-the-sample-code"></a>Nu de voorbeeldcode verkennen

Belangrijkste gedeelten van de voorbeeldcode zijn opgesteld met opmerking blokken die met beginnen `KEY SAMPLE CODE STARTS HERE` en eindigen met `KEY SAMPLE CODE ENDS HERE`, zodat u gemakkelijk om te verkennen van de voorbeeld-app. Deze sleutel delen van de voorbeeldcode bevatten de meest relevante te leren over het gebruik van de Computer Vision-API-clientbibliotheek voor verschillende taken code. U kunt zoeken naar `KEY SAMPLE CODE STARTS HERE` in Visual Studio om te verplaatsen tussen de meest relevante secties met code in de Computer Vision-voorbeeld-app. 

Bijvoorbeeld, de `UploadAndAnalyzeImageAsync` weergegeven methode, volgen en opgenomen in AnalyzePage.xaml, ziet u hoe u van de clientbibliotheek voor het analyseren van een lokale installatiekopie door het aanroepen van de `ComputerVisionClient.AnalyzeImageInStreamAsync` methode.

```csharp
private async Task<ImageAnalysis> UploadAndAnalyzeImageAsync(string imageFilePath)
{
    // -----------------------------------------------------------------------
    // KEY SAMPLE CODE STARTS HERE
    // -----------------------------------------------------------------------

    //
    // Create Cognitive Services Vision API Service client.
    //
    using (var client = new ComputerVisionClient(Credentials) { Endpoint = Endpoint })
    {
        Log("ComputerVisionClient is created");

        using (Stream imageFileStream = File.OpenRead(imageFilePath))
        {
            //
            // Analyze the image for all visual features.
            //
            Log("Calling ComputerVisionClient.AnalyzeImageInStreamAsync()...");
            VisualFeatureTypes[] visualFeatures = GetSelectedVisualFeatures();
            string language = (_language.SelectedItem as RecognizeLanguage).ShortCode;
            ImageAnalysis analysisResult = await client.AnalyzeImageInStreamAsync(imageFileStream, visualFeatures, null, language);
            return analysisResult;
        }
    }

    // -----------------------------------------------------------------------
    // KEY SAMPLE CODE ENDS HERE
    // -----------------------------------------------------------------------
}
```

### <a name="explore-the-client-library"></a>De clientbibliotheek verkennen

Deze voorbeeld-app maakt gebruik van de Computer Vision-API-clientbibliotheek, een thin C#-client-wrapper voor de Computer Vision-API in Azure Cognitive Services. De clientbibliotheek is beschikbaar via NuGet in de [Microsoft.Azure.CognitiveServices.Vision.ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision/) pakket. Wanneer u de Visual Studio-toepassing hebt gebouwd, kunt u de clientbibliotheek opgehaald uit de bijbehorende NuGet-pakket. U kunt ook weergeven met de broncode voor de clientbibliotheek in de `/ClientLibrary` map van de `Microsoft/Cognitive-Vision-Windows` opslagplaats.

De functionaliteit van de clientbibliotheek draait om de `ComputerVisionClient` klasse, in de `Microsoft.Azure.CognitiveServices.Vision.ComputerVision` naamruimte, terwijl de modellen die worden gebruikt door de `ComputerVisionClient` klasse tijdens interactie met Computer Vision vindt u in de `Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models` naamruimte. In de verschillende pagina's voor XAML scenario opgenomen met de voorbeeld-app, vindt u de volgende `using` richtlijnen voor naamruimten:

```csharp
// -----------------------------------------------------------------------
// KEY SAMPLE CODE STARTS HERE
// Use the following namespace for ComputerVisionClient.
// -----------------------------------------------------------------------
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;
// -----------------------------------------------------------------------
// KEY SAMPLE CODE ENDS HERE
// -----------------------------------------------------------------------
```

U leert meer over de verschillende methoden die zijn opgenomen in de `ComputerVisionClient` klasse als u de scenario's die deel uitmaakt van de Computer Vision-voorbeeld-app verkennen.

## <a name="explore-the-analyze-image-scenario"></a>Het scenario analyseren installatiekopie verkennen

In dit scenario wordt beheerd door de pagina AnalyzePage.xaml. U kunt kiezen voor de visuele kenmerken en de taal voor de analyse en zowel de installatiekopie als de resultaten zien. De pagina scenario doet dit met behulp van een van de volgende methoden, afhankelijk van de bron van de afbeelding:

* UploadAndAnalyzeImageAsync  
  Deze methode wordt gebruikt voor lokale installatiekopieën, waarin de installatiekopie moet worden gecodeerd als een `Stream` en verzonden naar de Computer Vision door het aanroepen van de `ComputerVisionClient.AnalyzeImageInStreamAsync` methode.
* AnalyzeUrlAsync  
  Deze methode wordt gebruikt voor externe afbeeldingen, waarin de URL voor de installatiekopie is verzonden naar de Computer Vision door het aanroepen van de `ComputerVisionClient.AnalyzeImageAsync` methode.

De `UploadAndAnalyzeImageAsync` methode maakt u een nieuwe `ComputerVisionClient` instantie, met de opgegeven abonnements-sleutel en het eindpunt-URL. Omdat de voorbeeld-app een lokale installatiekopie analyseren is, heeft de inhoud van die installatiekopie naar de Computer Vision verzenden. Het openen van het lokale bestand opgegeven in `imageFilePath` om te lezen als een `Stream`, klikt u vervolgens worden de visuele kenmerken en de taal die is geselecteerd op de pagina scenario opgehaald. Roept de `ComputerVisionClient.AnalyzeImageInStreamAsync` methode, waardoor de `Stream` voor het bestand, de visuele kenmerken en de taal, retourneert het resultaat vervolgens als een `ImageAnalysis` exemplaar. De methoden die zijn overgenomen van de `ImageScenarioPage` klasse de geretourneerde resultaten presenteren op de pagina scenario.

De `AnalyzeUrlAsync` methode maakt u een nieuwe `ComputerVisionClient` instantie, met de opgegeven abonnements-sleutel en het eindpunt-URL. Het wordt de visuele kenmerken en taal in de scenario-pagina hebt geselecteerd. Roept de `ComputerVisionClient.AnalyzeImageInStreamAsync` methode, waardoor de afbeeldings-URL, de visuele kenmerken en de taal, retourneert het resultaat als vervolgens een `ImageAnalysis` exemplaar. De methoden die zijn overgenomen van de `ImageScenarioPage` klasse de geretourneerde resultaten presenteren op de pagina scenario.

## <a name="explore-the-analyze-image-with-domain-model-scenario"></a>De installatiekopie van het analyseren met domeinmodel scenario verkennen

In dit scenario wordt beheerd door de pagina AnalyzeInDomainPage.xaml. U kunt het model van een domein, zoals `celebrities` of `landmarks`, en het uitvoeren van een domein-specifieke analyse uit van de installatiekopie en de installatiekopie en de resultaten. De pagina scenario gebruikt de volgende methoden, afhankelijk van de bron van de afbeelding:

* GetAvailableDomainModelsAsync  
  Deze methode wordt een lijst van beschikbare modellen opgehaald van de Computer Vision en vult de `_domainModelComboBox` keuzelijst met invoervak op de pagina met behulp van de `ComputerVisionClient.ListModelsAsync` methode.
* UploadAndAnalyzeInDomainImageAsync  
  Deze methode wordt gebruikt voor lokale installatiekopieën, waarin de installatiekopie moet worden gecodeerd als een `Stream` en verzonden naar de Computer Vision door het aanroepen van de `ComputerVisionClient.AnalyzeImageByDomainInStreamAsync` methode.
* AnalyzeInDomainUrlAsync  
  Deze methode wordt gebruikt voor externe afbeeldingen, waarin de URL voor de installatiekopie is verzonden naar de Computer Vision door het aanroepen van de `ComputerVisionClient.AnalyzeImageByDomainAsync` methode.

De `UploadAndAnalyzeInDomainImageAsync` methode maakt u een nieuwe `ComputerVisionClient` instantie, met de opgegeven abonnements-sleutel en het eindpunt-URL. Omdat de voorbeeld-app een lokale installatiekopie analyseren is, heeft de inhoud van die installatiekopie naar de Computer Vision verzenden. Het openen van het lokale bestand opgegeven in `imageFilePath` om te lezen als een `Stream`, vervolgens wordt de taal selecteren in de scenario-pagina. Roept de `ComputerVisionClient.AnalyzeImageByDomainInStreamAsync` methode, waardoor de `Stream` voor het bestand, de naam van het model van het domein en de taal, retourneert het resultaat vervolgens als een `DomainModelResults` exemplaar. De methoden die zijn overgenomen van de `ImageScenarioPage` klasse de geretourneerde resultaten presenteren op de pagina scenario.

De `AnalyzeInDomainUrlAsync` methode maakt u een nieuwe `ComputerVisionClient` instantie, met de opgegeven abonnements-sleutel en het eindpunt-URL. Hiermee haalt u de taal selecteren in de scenario-pagina. Roept de `ComputerVisionClient.AnalyzeImageByDomainAsync` methode, waardoor de afbeeldings-URL, de visuele kenmerken en de taal, retourneert het resultaat als vervolgens een `DomainModelResults` exemplaar. De methoden die zijn overgenomen van de `ImageScenarioPage` klasse de geretourneerde resultaten presenteren op de pagina scenario.

## <a name="explore-the-describe-image-scenario"></a>De afbeelding wordt beschreven scenario verkennen

In dit scenario wordt beheerd door de pagina DescribePage.xaml. U kunt een taal voor het maken van een leesbare beschrijving van de installatiekopie kiezen en de installatiekopie en de resultaten. De pagina scenario gebruikt de volgende methoden, afhankelijk van de bron van de afbeelding:

* UploadAndDescribeImageAsync  
  Deze methode wordt gebruikt voor lokale installatiekopieën, waarin de installatiekopie moet worden gecodeerd als een `Stream` en verzonden naar de Computer Vision door het aanroepen van de `ComputerVisionClient.DescribeImageInStreamAsync` methode.
* DescribeUrlAsync  
  Deze methode wordt gebruikt voor externe afbeeldingen, waarin de URL voor de installatiekopie is verzonden naar de Computer Vision door het aanroepen van de `ComputerVisionClient.DescribeImageAsync` methode.

De `UploadAndDescribeImageAsync` methode maakt u een nieuwe `ComputerVisionClient` instantie, met de opgegeven abonnements-sleutel en het eindpunt-URL. Omdat de voorbeeld-app een lokale installatiekopie analyseren is, heeft de inhoud van die installatiekopie naar de Computer Vision verzenden. Het openen van het lokale bestand opgegeven in `imageFilePath` om te lezen als een `Stream`, vervolgens wordt de taal selecteren in de scenario-pagina. Roept de `ComputerVisionClient.DescribeImageInStreamAsync` methode, waardoor de `Stream` voor het bestand, geeft het maximumaantal kandidaten (in dit geval 3) en de taal, klikt u vervolgens het resultaat als een `ImageDescription` exemplaar. De methoden die zijn overgenomen van de `ImageScenarioPage` klasse de geretourneerde resultaten presenteren op de pagina scenario.

De `DescribeUrlAsync` methode maakt u een nieuwe `ComputerVisionClient` instantie, met de opgegeven abonnements-sleutel en het eindpunt-URL. Hiermee haalt u de taal selecteren in de scenario-pagina. Roept de `ComputerVisionClient.DescribeImageAsync` methode, waardoor de afbeeldings-URL, het maximumaantal kandidaten (in dit geval 3) en de taal, retourneert het resultaat als vervolgens een `ImageDescription` exemplaar. De methoden die zijn overgenomen van de `ImageScenarioPage` klasse de geretourneerde resultaten presenteren op de pagina scenario.

## <a name="explore-the-generate-tags-scenario"></a>Het genereren van codes scenario verkennen

In dit scenario wordt beheerd door de pagina TagsPage.xaml. U kunt een taal voor het taggen van de visuele kenmerken van een afbeelding kiezen en de installatiekopie en de resultaten. De pagina scenario gebruikt de volgende methoden, afhankelijk van de bron van de afbeelding:

* UploadAndGetTagsForImageAsync  
  Deze methode wordt gebruikt voor lokale installatiekopieën, waarin de installatiekopie moet worden gecodeerd als een `Stream` en verzonden naar de Computer Vision door het aanroepen van de `ComputerVisionClient.TagImageInStreamAsync` methode.
* GenerateTagsForUrlAsync  
  Deze methode wordt gebruikt voor externe afbeeldingen, waarin de URL voor de installatiekopie is verzonden naar de Computer Vision door het aanroepen van de `ComputerVisionClient.TagImageAsync` methode.

De `UploadAndGetTagsForImageAsync` methode maakt u een nieuwe `ComputerVisionClient` instantie, met de opgegeven abonnements-sleutel en het eindpunt-URL. Omdat de voorbeeld-app een lokale installatiekopie analyseren is, heeft de inhoud van die installatiekopie naar de Computer Vision verzenden. Het openen van het lokale bestand opgegeven in `imageFilePath` om te lezen als een `Stream`, vervolgens wordt de taal selecteren in de scenario-pagina. Roept de `ComputerVisionClient.TagImageInStreamAsync` methode, waardoor de `Stream` voor het bestand en de taal, retourneert het resultaat vervolgens als een `TagResult` exemplaar. De methoden die zijn overgenomen van de `ImageScenarioPage` klasse de geretourneerde resultaten presenteren op de pagina scenario.

De `GenerateTagsForUrlAsync` methode maakt u een nieuwe `ComputerVisionClient` instantie, met de opgegeven abonnements-sleutel en het eindpunt-URL. Hiermee haalt u de taal selecteren in de scenario-pagina. Roept de `ComputerVisionClient.TagImageAsync` methode, waardoor de afbeeldings-URL en de taal, retourneert het resultaat als vervolgens een `TagResult` exemplaar. De methoden die zijn overgenomen van de `ImageScenarioPage` klasse de geretourneerde resultaten presenteren op de pagina scenario.

## <a name="explore-the-recognize-text-ocr-scenario"></a>Verken de tekst herkennen (OCR)-scenario

In dit scenario wordt beheerd door de pagina OCRPage.xaml. U kunt een taal te herkennen en gedrukte tekst extraheren uit een afbeelding kiezen en de installatiekopie en de resultaten. De pagina scenario gebruikt de volgende methoden, afhankelijk van de bron van de afbeelding:

* UploadAndRecognizeImageAsync  
  Deze methode wordt gebruikt voor lokale installatiekopieën, waarin de installatiekopie moet worden gecodeerd als een `Stream` en verzonden naar de Computer Vision door het aanroepen van de `ComputerVisionClient.RecognizePrintedTextInStreamAsync` methode.
* RecognizeUrlAsync  
  Deze methode wordt gebruikt voor externe afbeeldingen, waarin de URL voor de installatiekopie is verzonden naar de Computer Vision door het aanroepen van de `ComputerVisionClient.RecognizePrintedTextAsync` methode.

De `UploadAndRecognizeImageAsync` methode maakt u een nieuwe `ComputerVisionClient` instantie, met de opgegeven abonnements-sleutel en het eindpunt-URL. Omdat de voorbeeld-app een lokale installatiekopie analyseren is, heeft de inhoud van die installatiekopie naar de Computer Vision verzenden. Het openen van het lokale bestand opgegeven in `imageFilePath` om te lezen als een `Stream`, vervolgens wordt de taal selecteren in de scenario-pagina. Roept de `ComputerVisionClient.RecognizePrintedTextInStreamAsync` methode, waarmee wordt aangegeven dat de richting niet is gedetecteerd en door te geven de `Stream` voor het bestand en de taal, retourneert het resultaat vervolgens als een `OcrResult` exemplaar. De methoden die zijn overgenomen van de `ImageScenarioPage` klasse de geretourneerde resultaten presenteren op de pagina scenario.

De `RecognizeUrlAsync` methode maakt u een nieuwe `ComputerVisionClient` instantie, met de opgegeven abonnements-sleutel en het eindpunt-URL. Hiermee haalt u de taal selecteren in de scenario-pagina. Roept de `ComputerVisionClient.RecognizePrintedTextAsync` methode, waarmee wordt aangegeven dat de richting niet is gedetecteerd en door te geven de afbeeldings-URL en de taal, retourneert het resultaat als vervolgens een `OcrResult` exemplaar. De methoden die zijn overgenomen van de `ImageScenarioPage` klasse de geretourneerde resultaten presenteren op de pagina scenario.

## <a name="explore-the-recognize-text-v2-english-scenario"></a>Verken de herkennen tekst V2 (Engels)-scenario

In dit scenario wordt beheerd door de pagina TextRecognitionPage.xaml. U kunt de opname-modus en een taal die moet worden asynchroon herkennen en afgedrukt of handgeschreven tekst extraheren uit een afbeelding kiezen en de installatiekopie en de resultaten. De pagina scenario gebruikt de volgende methoden, afhankelijk van de bron van de afbeelding:

* UploadAndRecognizeImageAsync  
  Deze methode wordt gebruikt voor lokale installatiekopieën, waarin de installatiekopie moet worden gecodeerd als een `Stream` en verzonden naar de Computer Vision door het aanroepen van de `RecognizeAsync` methode en het doorgeven van een geparameteriseerde gemachtigde voor de `ComputerVisionClient.RecognizeTextInStreamAsync` methode.
* RecognizeUrlAsync  
  Deze methode wordt gebruikt voor externe afbeeldingen, waarin de URL voor de installatiekopie is verzonden naar de Computer Vision door het aanroepen van de `RecognizeAsync` methode en het doorgeven van een geparameteriseerde gemachtigde voor de `ComputerVisionClient.RecognizeTextAsync` methode.
* Deze methode RecognizeAsync verwerkt de asynchrone aanroep voor zowel de `UploadAndRecognizeImageAsync` en `RecognizeUrlAsync` methoden, evenals het controleren op resultaten door het aanroepen van de `ComputerVisionClient.GetTextOperationResultAsync` methode.

In tegenstelling tot de andere scenario's van de Computer Vision-voorbeeld-app, is dit scenario is asynchroon, in dat één methode wordt aangeroepen om het proces te starten, maar een andere methode om te controleren op de status en het resultaat van dit proces wordt genoemd. De logische stroom in dit scenario verschilt enigszins van die in de andere scenario's.

De `UploadAndRecognizeImageAsync` methode Hiermee opent u het lokale bestand dat is opgegeven `imageFilePath` om te lezen als een `Stream`, roept vervolgens de `RecognizeAsync` methode te geven:

* Een lambda-expressie voor een geparameteriseerde asynchrone gemachtigde van de `ComputerVisionClient.RecognizeTextInStreamAsync` methode, met de `Stream` voor het bestand en de opname-modus als parameters in `GetHeadersAsyncFunc`.
* Een lambda-expressie voor een gemachtigde om op te halen de `Operation-Location` response-header-waarde, in `GetOperationUrlFunc`.

De `RecognizeUrlAsync` methodeaanroepen de `RecognizeAsync` methode te geven:

* Een lambda-expressie voor een geparameteriseerde asynchrone gemachtigde van de `ComputerVisionClient.RecognizeTextAsync` methode met de URL van de installatiekopie van het externe en de opname-modus als parameters in `GetHeadersAsyncFunc`.
* Een lambda-expressie voor een gemachtigde om op te halen de `Operation-Location` response-header-waarde, in `GetOperationUrlFunc`.

Wanneer de `RecognizeAsync` methode is voltooid, beide `UploadAndRecognizeImageAsync` en `RecognizeUrlAsync` methoden het resultaat als een `TextOperationResult` exemplaar. De methoden die zijn overgenomen van de `ImageScenarioPage` klasse de geretourneerde resultaten presenteren op de pagina scenario.

De `RecognizeAsync` methode roept de geparameteriseerde gemachtigde voor de `ComputerVisionClient.RecognizeTextInStreamAsync` of `ComputerVisionClient.RecognizeTextAsync` methode doorgegeven `GetHeadersAsyncFunc` en wacht tot het antwoord. De methode roept vervolgens de gemachtigde die wordt doorgegeven in `GetOperationUrlFunc` om op te halen de `Operation-Location` response-header-waarde uit het antwoord. Deze waarde is de URL die wordt gebruikt om op te halen van de resultaten van de methode die wordt doorgegeven in `GetHeadersAsyncFunc` van Computer Vision.

De `RecognizeAsync` methode roept vervolgens de `ComputerVisionClient.GetTextOperationResultAsync` methode, waardoor de URL die is opgehaald uit de `Operation-Location` response-header in om op te halen van de status en het resultaat van de methode die wordt doorgegeven in `GetHeadersAsyncFunc`. Als de status wordt niet aangegeven dat de methode is voltooid, geslaagd of mislukt, de `RecognizeAsync` methodeaanroepen `ComputerVisionClient.GetTextOperationResultAsync` 3 meer keren, wachten op 3 seconden tussen aanroepen. De `RecognizeAsync` methode worden de resultaten geretourneerd aan de methode die wordt aangeroepen.

## <a name="explore-the-get-thumbnail-scenario"></a>Het scenario ophalen miniatuur verkennen

In dit scenario wordt beheerd door de pagina ThumbnailPage.xaml. U kunt aangeven of u gebruik van slim bijsnijden, en geef de gewenste hoogte en breedte, voor het genereren van een miniatuur van een afbeelding, en zowel de installatiekopie als de resultaten zien. De pagina scenario gebruikt de volgende methoden, afhankelijk van de bron van de afbeelding:

* UploadAndThumbnailImageAsync  
  Deze methode wordt gebruikt voor lokale installatiekopieën, waarin de installatiekopie moet worden gecodeerd als een `Stream` en verzonden naar de Computer Vision door het aanroepen van de `ComputerVisionClient.GenerateThumbnailInStreamAsync` methode.
* ThumbnailUrlAsync  
  Deze methode wordt gebruikt voor externe afbeeldingen, waarin de URL voor de installatiekopie is verzonden naar de Computer Vision door het aanroepen van de `ComputerVisionClient.GenerateThumbnailAsync` methode.

De `UploadAndThumbnailImageAsync` methode maakt u een nieuwe `ComputerVisionClient` instantie, met de opgegeven abonnements-sleutel en het eindpunt-URL. Omdat de voorbeeld-app een lokale installatiekopie analyseren is, heeft de inhoud van die installatiekopie naar de Computer Vision verzenden. Het openen van het lokale bestand opgegeven in `imageFilePath` om te lezen als een `Stream`. Roept de `ComputerVisionClient.GenerateThumbnailInStreamAsync` methode, de breedte, hoogte, waardoor de `Stream` voor het bestand, en of u wilt gebruiken slim bijsnijden, retourneert het resultaat vervolgens als een `Stream`. De methoden die zijn overgenomen van de `ImageScenarioPage` klasse de geretourneerde resultaten presenteren op de pagina scenario.

De `RecognizeUrlAsync` methode maakt u een nieuwe `ComputerVisionClient` instantie, met de opgegeven abonnements-sleutel en het eindpunt-URL. Roept de `ComputerVisionClient.GenerateThumbnailAsync` methode, waarbij de breedte, hoogte, de URL voor de installatiekopie en of u wilt gebruiken slim bijsnijden doorgegeven retourneert het resultaat als vervolgens een `Stream`. De methoden die zijn overgenomen van de `ImageScenarioPage` klasse de geretourneerde resultaten presenteren op de pagina scenario.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de map waarin u hebt gekloond de `Microsoft/Cognitive-Vision-Windows` opslagplaats. Als u ervoor hebt gekozen voor het gebruik van de voorbeeld-installatiekopieën, verwijdert u ook de map waarin u hebt gekloond de `Microsoft/Cognitive-Face-Windows` opslagplaats.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aan de slag met de Face-API](../../Face/Tutorials/FaceAPIinCSharpTutorial.md)