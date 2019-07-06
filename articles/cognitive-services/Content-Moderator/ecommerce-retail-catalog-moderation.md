---
title: 'Zelfstudie: E-commerce-productafbeeldingen beheren - Content Moderator'
titlesuffix: Azure Cognitive Services
description: Instellen van een toepassing om te analyseren en productafbeeldingen classificeren met de opgegeven labels (met behulp van Azure Computer Vision en aangepaste Vision). Tag ongewenste afbeeldingen verder kunnen worden gecontroleerd (met behulp van Azure Content Moderator).
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: ec17f9f0206ef639bd47d694880c064a012ea1cf
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604196"
---
# <a name="tutorial-moderate-e-commerce-product-images-with-azure-content-moderator"></a>Zelfstudie: E-commerce-productafbeeldingen beheren met Azure Content Moderator

In deze zelfstudie leert u hoe u Azure Cognitive Services, met inbegrip van Content Moderator, om te classificeren en gemiddeld productafbeeldingen voor een e-commerce-scenario. U Computer Vision en aangepaste Vision tags (labels) op afbeeldingen wilt toepassen en vervolgens maakt u een revisie door het team, dat op basis van machine-learning-technologieën voor Content Moderator met teams voor menselijke beoordeling combineert voor een systeem intelligent beheer.

In deze zelfstudie ontdekt u hoe u:

> [!div class="checklist"]
> * Aanmelden voor Content Moderator en een beoordelingsteam maken.
> * De Image-API van Content Moderator gebruiken om te scannen op mogelijke ongepaste inhoud of inhoud voor volwassenen.
> * De Computer Vision-service gebruiken om te scannen op beroemdheden (of andere tags die Computer Vision kan detecteren).
> * De Custom Vision service gebruiken om te scannen op de aanwezigheid van vlaggen, speelgoed en pennen (of andere aangepaste tags).
> * De gecombineerde scanresultaten presenteren voor menselijke beoordeling en uiteindelijke besluitvorming.

De volledige voorbeeldcode is beschikbaar in de opslagplaats [Samples eCommerce Catalog Moderation](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration) op GitHub.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

- Een abonnementssleutel voor Content Moderator. Volg de instructies in [Een Cognitive Services-account maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) om u te abonneren op Content Moderator en een sleutel op te halen.
- Een abonnementssleutel voor Computer Vision (dezelfde instructies als hierboven).
- Een versie van [Visual Studio 2015 of 2017](https://www.visualstudio.com/downloads/).
- Een reeks afbeeldingen voor elk label dat door de Custom Vision-classificatie wordt gebruikt (in dit geval van speelgoed, pennen en vlaggen).

## <a name="create-a-review-team"></a>Een beoordelingsteam maken

Verwijzen naar de [Content Moderator proberen op het web](quick-start.md) Quick Start voor instructies over hoe u zich aanmelden voor de [Content Moderator bekijken hulpprogramma](https://contentmoderator.cognitive.microsoft.com/) en maken van een beoordelingsteam. Noteer de waarde voor **Team Id** op de pagina **Create review team**.

## <a name="create-custom-moderation-tags"></a>Aangepaste controletags maken

Maak vervolgens aangepaste labels in het beoordelingsprogramma (Zie de [Tags](https://docs.microsoft.com/azure/cognitive-services/content-moderator/review-tool-user-guide/tags) artikel als u hulp nodig met dit proces). In dit geval voegen we deze tags toe: **celebrity**, **USA**, **flag**, **toy** en **pen**. Niet alle labels moeten worden opgespoord categorieën in Computer Vision (zoals **beroemdheid**); u kunt uw eigen aangepaste tags toevoegen als u de classificatie Custom Vision voor het detecteren van deze later trainen.

![Aangepaste tags configureren](images/tutorial-ecommerce-tags2.PNG)

## <a name="create-visual-studio-project"></a>Een Visual Studio-project maken

1. Open het dialoogvenster Nieuw project in Visual Studio. Vouw **Geïnstalleerd** en vervolgens **Visual C#** uit en selecteer **Console App (.NET Framework)** .
1. Geef de toepassing de naam **EcommerceModeration** en klik op **OK**.
1. Als u dit project aan een bestaande oplossing toevoegt, selecteert u dit project als opstartproject één.

In deze zelfstudie leest de code die centraal staat in het project, maar deze wordt niet betrekking hebben op elke regel code. Kopieer de volledige inhoud van _Program.cs_ uit het voorbeeldproject ([Samples eCommerce Catalog Moderation](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration)) naar het bestand _Program.cs_ van het nieuwe project. Neem vervolgens de onderstaande gedeelten door voor meer informatie over de werking van het project en hoe u dit zelf kunt gebruiken.

## <a name="define-api-keys-and-endpoints"></a>API-sleutels en -eindpunten definiëren

Deze zelfstudie wordt gebruikgemaakt van drie cognitive services. Daarom vereist drie bijbehorende sleutels en API-eindpunten. Bekijk de volgende velden in de klasse **Program**:

[!code-csharp[define API keys and endpoint URIs](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=21-29)]

Moet u bijwerken de `___Key` velden met de waarden van de abonnementssleutels van uw (u krijgt de `CustomVisionKey` later), en u wilt wijzigen de `___Uri` velden, zodat ze de juiste regio-id's bevatten. Voer in het gedeelte `YOURTEAMID` van het veld `ReviewUri` de id in van het beoordelingsteam dat u eerder hebt gemaakt. U kunt vullen in het laatste gedeelte van de `CustomVisionUri` veld later op.

## <a name="primary-method-calls"></a>Aanroepen van primaire methode

Bekijk de volgende code in de methode **Main**, waarmee een lijst met afbeeldings-URL's wordt doorlopen. Deze analyseert elke installatiekopie met de drie verschillende services, registreert de toegepaste labels in de **ReviewTags** matrix en maakt vervolgens een beoordeling voor menselijke moderators door afbeeldingen te sturen naar het Content Moderator controleren-hulpprogramma. Deze methoden gaan we in de volgende gedeelten bespreken. Als u wilt, kunt u bepalen welke installatiekopieën worden verzonden om te controleren, met behulp van de **ReviewTags** -matrix in een voorwaarde om te controleren welke labels zijn toegepast.

[!code-csharp[Main: evaluate each image and create review](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=53-70)]

## <a name="evaluateadultracy-method"></a>EvaluateAdultRacy-methode

Bekijk de methode **EvaluateAdultRacy** in de klasse **Program**. Deze methode accepteert een afbeeldings-URL en een matrix van sleutel-waardeparen als parameters. De methode roept de Image API van Content Moderator aan (met behulp van REST) om de waarden voor AdultScore en RacyScore op te vragen voor de afbeelding. Als de score voor een groter is dan 0.4 (het bereik ligt tussen 0 en 1), wordt de overeenkomstige waarde in de **ReviewTags** naar een matrix **waar**.

[!code-csharp[define EvaluateAdultRacy method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=73-113)]

## <a name="evaluatecomputervisiontags-method"></a>Methode EvaluateComputerVisionTags

De volgende methode verwerkt een afbeeldings-URL en de gegevens van uw Computer Vision-abonnement en analyseert de afbeelding op de aanwezigheid van beroemdheden. Als er een of meer beroemdheden worden gevonden in de afbeelding, wordt de bijbehorende waarde in de matrix **ReviewTags** ingesteld op **True**.

[!code-csharp[define EvaluateCustomVisionTags method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=115-146)]

## <a name="evaluatecustomvisiontags-method"></a>EvaluateComputerVisionTags-methode

Bekijk nu de methode **EvaluateCustomVisionTags**, waarmee de daadwerkelijke producten worden geclassificeerd&mdash;in dit geval vlaggen, speelgoed en pennen. Volg de instructies in de [over het bouwen van een classificatie](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) handleiding voor het bouwen van uw eigen aangepaste installatiekopie-classificatie en vlaggen, toys, en pennen (of die u hebt gekozen als uw aangepaste labels) in afbeeldingen detecteren. U kunt de afbeeldingen in de **voorbeeldafbeeldingen** map van de [GitHub-opslagplaats](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration) te trainen snel enkele van de categorieën in dit voorbeeld.

![Webpagina van Custom Vision met trainingsafbeeldingen van pennen, speelgoed en vlaggen](images/tutorial-ecommerce-custom-vision.PNG)

Nadat u uw classificatie hebt getraind, worden de voorspelling van sleutel en de voorspelling eindpunt-URL ophalen (Zie [ophalen van de sleutel-URL en voorspelling](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/use-prediction-api#get-the-url-and-prediction-key) als u hulp nodig bij het ophalen van deze), en het toewijzen van deze waarden aan uw `CustomVisionKey` en `CustomVisionUri` de velden, respectievelijk. De methode gebruikt deze waarden om query's uit te voeren op de classificatie. Als de classificatie een of meer van de aangepaste tags vindt in de afbeelding, worden de bijbehorende waarden in de matrix **ReviewTags** ingesteld op **True**.

[!code-csharp[define EvaluateCustomVisionTags method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=148-171)]

## <a name="create-reviews-for-review-tool"></a>Beoordelingen maken voor het beoordelingsprogramma

In de vorige secties, kunt u verkend hoe binnenkomende afbeeldingen voor erotische en ongepaste inhoud (Content Moderator), beroemdheden (Computer-Vision) en verschillende andere objecten (Custom Vision) in de app worden gescand. Hieronder staan de **CreateReview** methode uploadt hij de installatiekopieën met al hun toegepaste labels (doorgegeven als _metagegevens_) naar de Content Moderator-controlehulpprogramma.

[!code-csharp[define CreateReview method](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=173-196)]

De afbeeldingen worden weergegeven op het tabblad Review van het [beoordelingsprogramma Content Moderator](https://contentmoderator.cognitive.microsoft.com/).

![Schermafbeelding van het beoordelingsprogramma Content Moderator met verschillende afbeeldingen en de bijbehorende gemarkeerde tags](images/tutorial-ecommerce-content-moderator.PNG)

## <a name="submit-a-list-of-test-images"></a>Een lijst met testafbeeldingen aanbieden

Zoals u kunt zien in de methode **Main**, zoekt dit programma naar een map 'C:Test' met daarin een bestand _Urls.txt_ dat een lijst met afbeeldings-URL's bevat. Deze bestanden en mappen maken, of wijzig het pad om te verwijzen naar het tekstbestand. Vul dit bestand met de URL's van installatiekopieën die u wilt testen.

[!code-csharp[Main: set up test directory, read lines](~/samples-eCommerceCatalogModeration/Fusion/Program.cs?range=38-51)]

## <a name="run-the-program"></a>Het programma uitvoeren

Als u alle bovenstaande stappen hebt gevolgd, moet het programma voor het verwerken van elke afbeelding (alle drie de services voor hun betreffende labels uitvoeren van query's) en vervolgens de installatiekopieën met informatie over de assettag uploaden naar de Content Moderator-controlehulpprogramma.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een programma instellen voor het analyseren van productafbeeldingen, ze te labelen door producttype en een beoordelingsteam om geïnformeerde beslissingen over inhoudstoezicht toestaan. In de volgende zelfstudie wordt meer aandacht besteed aan het beoordelen van gecontroleerde afbeeldingen.

> [!div class="nextstepaction"]
> [Gecontroleerde afbeeldingen beoordelen](./review-tool-user-guide/review-moderated-images.md)
