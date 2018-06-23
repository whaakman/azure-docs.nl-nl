---
title: Een classificatie met aangepaste visie Service - cognitieve Azure-Services bouwen | Microsoft Docs
description: Informatie over het gebruik van de aangepaste visie-Service voor het maken van een classificatie die objecten in foto's kan worden onderscheiden.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/02/2018
ms.author: anroth
ms.openlocfilehash: 6dc271c13f53a445c7d1101f5264d890208bd03c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344916"
---
# <a name="how-to-build-a-classifier-with-custom-vision"></a>Het bouwen van een classificatie met aangepaste visie

De aangepaste visie als Service wilt gebruiken, moet u eerst een classificatie maken. Informatie over het bouwen van een classificatie via de webbrowser in dit document.

## <a name="prerequisites"></a>Vereisten

Als u wilt maken van een classificatie, moet u eerst hebben:

- Een geldige [Microsoft-account](https://account.microsoft.com/account) of een Azure Active Directory OrgID ('werk of school-account'), zodat u zich bij customvision.ai aanmelden kunt en aan de slag.

    > [!IMPORTANT] 
    > De aanmelding OrgID voor gebruikers van Azure Active Directory (Azure AD) [nationale clouds](https://www.microsoft.com/en-us/trustcenter/cloudservices/nationalcloud) wordt momenteel niet ondersteund.

- Een serie afbeeldingen training van uw classificatie (met een minimum van 30 afbeeldingen per tag).

- Enkele afbeeldingen voor het testen van uw classificatie nadat de classificatie wordt getraind.

- Optioneel: Een Azure-abonnement is gekoppeld aan uw Microsoft-Account of OrgID. Als u geen Azure-abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

    > [!IMPORTANT]
    > Zonder een Azure-abonnement kunt u alleen zich maken __beperkt proefversie__ projecten. Als u een Azure-abonnement hebt, wordt u gevraagd om te maken van aangepaste visie Service Training en voorspelling resources in de [Azure-portal](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision) tijdens het maken van het project.   

## <a name="create-a-new-project"></a>Een nieuw project maken

Gebruik de volgende stappen uit voor het maken van een nieuw project:

1. Navigeer in uw webbrowser naar de [aangepaste visie webpagina](https://customvision.ai). Selecteer __aanmelden__ om te beginnen met de service.

    ![Afbeelding van de aanmeldingspagina](./media/getting-started-build-a-classifier/custom-vision-web-ui.png)

    > [!NOTE]
    > Nadat u zich bij de aangepaste visie Service aanmelden, krijgt u een lijst met projecten. Buiten-projecten twee 'beperkt proefversie' voor het testen zijn projecten gekoppeld aan een Azure-Resource. Als u een Azure-gebruiker bent, ziet u alle projecten die zijn gekoppeld aan [Azure-Resources](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#grant-access-to-resources) waartoe u toegang hebt. 

2. Selecteer voor het maken van uw eerste project **nieuw Project**. Voor uw eerste project, wordt u gevraagd om de voorwaarden van de Service te accepteren. Schakel het selectievakje in en selecteer vervolgens de **ik ga akkoord** knop. De **nieuw project** dialoogvenster wordt weergegeven.

    ![Het dialoogvenster Nieuw project bevat velden voor de naam, beschrijving en domeinen.](./media/getting-started-build-a-classifier/new-project.png)

3. Voer een naam en beschrijving voor het project. Selecteer een van de beschikbare domeinen. Elk domein optimaliseert de classificatie voor specifieke typen installatiekopieën, zoals beschreven in de volgende tabel:

    |Domein|Doel|
    |---|---|
    |__Algemene__| Geoptimaliseerd voor een breed scala aan installatiekopie classificatie taken. Als geen van de andere domeinen geschikt zijn, of u niet weet welk domein om te kiezen, selecteert u het algemene domein. |
    |__Voeding__|Geoptimaliseerd voor foto's van schalen zoals u zou ze in een restaurant. Als u classificeren foto's van afzonderlijke fruit of fruit wilt, gebruikt u het domein voeding.|
    |__Monumenten__|Geoptimaliseerd voor herkenbare monumenten, natuurlijke en kunstmatige. Dit domein werkt het beste als de kenmerkende duidelijk zichtbaar is in de foto. Dit domein werkt ook als de kenmerkende enigszins vorm door mensen plaatsen.|
    |__Handelsversie__|Geoptimaliseerd voor installatiekopieën die zijn gevonden in een winkelwagen catalogus of de winkelwagen website. Als u classificeren van hoge precisie tussen japonnen, broek en shirt wilt, moet u dit domein gebruiken.|
    |__Volwassene__|Geoptimaliseerd voor de inhoud voor volwassenen en niet-volwassene inhoud beter te definiëren. Bijvoorbeeld, als u blokkeren afbeeldingen van mensen in bad kleuren wilt, kunt dit domein u voor het bouwen van een aangepaste classificatie kunt u doen.|
    |__Compacte domeinen__| Geoptimaliseerd voor de beperkingen van realtime classificatie op mobiele apparaten. De modellen die worden gegenereerd door compact domeinen kunnen worden geëxporteerd om lokaal uitvoeren.|

    Als u wilt, kunt u het domein later wijzigen.

4. Selecteer een resourcegroep. De resourcegroep vervolgkeuzelijst ziet u alle uw Azure-resourcegroepen die een aangepaste visie Service Resource bevatten. U kunt ook selecteren maken __beperkt proefversie__. De vermelding voor beperkte proefversie is de enige resourcegroep is die een niet-Azure-gebruiker kan kiezen uit worden.

    Voor het maken van het project, selecteer __project maken__.

## <a name="upload-and-tag-images"></a>Uploaden en installatiekopieën van het label

1. U kunt installatiekopieën toevoegen aan de classificatie met de __installatiekopieën toevoegen__ knop en selecteer vervolgens __lokale bestanden bladeren__. Selecteer __Open__ verplaatsen naar labels.

    > [!TIP]
    > Na het selecteren van installatiekopieën, moet u ze labelen. Het label wordt toegepast op de groep van installatiekopieën die u hebt geselecteerd om te uploaden, zodat het eenvoudiger om afbeeldingen te uploaden door de labels die u wilt gebruiken. U kunt ook de tag voor de geselecteerde kopieën wijzigen nadat ze zijn gemarkeerd en zijn geüpload.

    > [!TIP]
    > Uploaden van afbeeldingen met verschillende camerahoeken, licht, achtergrond, typen, stijlen, groepen, grootten, enzovoort. Gebruik verschillende typen photo om ervoor te zorgen dat uw classificatie niet is gericht en goed kunt generalize.

    Aangepaste visie Service accepteert training afbeeldingen in jpg en PNG bmp-indeling, maximaal 6 MB per afbeelding. (Voorspelling afbeeldingen kunnen maximaal 4 MB per afbeelding zijn). U wordt aangeraden dat afbeeldingen 256 pixels aan de kortste rand worden. Alle installatiekopieën die korter is dan 256 pixels aan de kortste rand worden uitgebreid door aangepaste visie Service.

    ![Het besturingselement van de installatiekopieën toevoegen wordt weergegeven in de linkerbovenhoek en een knop op middenonder.](./media/getting-started-build-a-classifier/add-images01.png)

    ![De bladerknop lokale bestanden wordt weergegeven in de buurt middenonder.](./media/getting-started-build-a-classifier/add-images02.png)

    >[!NOTE] 
    > De REST-API kan laden van installatiekopieën van training van URL's worden gebruikt.

2. Voer tekst in de tag stelt de __Mijn labels__ veld en gebruik vervolgens de __+__ knop. Om te uploaden van de installatiekopieën en ze labelen, gebruiken de __[aantal] bestanden uploaden__ knop. U kunt meer dan één label toevoegen aan de installatiekopieën. 

    > [!NOTE]
    > De tijd voor uploaden hangt af van het aantal en de grootte van afbeeldingen die u hebt geselecteerd.

    ![Afbeelding van de pagina tag en uploaden](./media/getting-started-build-a-classifier/add-images03.png)

3. Selecteer __gedaan__ zodra de afbeeldingen zijn geüpload.

    ![De voortgangsbalk ziet u alle taken zijn voltooid.](./media/getting-started-build-a-classifier/add-images04.png)

4. Ga terug naar stap 1 voor het uploaden van een andere set installatiekopieën. Bijvoorbeeld, als u wilt onderscheid maken tussen honden en pony's, uploaden en afbeeldingen van pony's labelen.

## <a name="train-and-evaluate-the-classifier"></a>Trainen en evalueren van de classificatie

Voor het trainen van de classificatie, selecteer de **trainen** knop.

![De knop train is rechts boven in het browservenster.](./media/getting-started-build-a-classifier/train01.png)

Het duurt slechts enkele minuten voor het trainen van de classificatie. Gedurende deze tijd wordt informatie over het trainingsproces weergegeven.

![De knop train is rechts boven in het browservenster.](./media/getting-started-build-a-classifier/train02.png)

Na de training, de __prestaties__ wordt weergegeven. De precision en terughalen indicatoren uitgelegd hoe goed die uw classificatie is, op basis van het automatische tests. Aangepaste visie Service gebruikmaakt van de installatiekopieën die je hebt ingediend voor training op deze getallen berekenen met behulp van een proces genaamd [k tweevoudige cross validatie](https://en.wikipedia.org/wiki/Cross-validation_(statistics)).

![De training resultaten weer de algehele precisie en intrekken en de nauwkeurigheid en intrekken voor elk label in de classificatie.](./media/getting-started-build-a-classifier/train03.png)

> [!NOTE] 
> Telkens wanneer u selecteert de **Train** knop, maakt u een nieuwe versie van de classificatie. U kunt uw oude iteraties in weergeven de **prestaties** tabblad, en u kunt alle die mogelijk verouderd verwijderen. Wanneer u een herhaling verwijdert, die u uiteindelijk alle installatiekopieën die gekoppeld aan het zijn verwijderen.

De classificatie wordt alle installatiekopieën gebruikt om een model waarin elke tag te maken. Als u wilt testen van de kwaliteit van het model, probeert de classificatie elke installatiekopie van het model om te zien wat het model wordt gevonden.

De eigenschappen van de classificatie-resultaten worden weergegeven.

|Termijn|Definitie|
|---|---|
|__Precisie__|Wanneer het classificeren van een afbeelding, hoe waarschijnlijk is de classificatie voor het classificeren van de afbeelding correct? Alle installatiekopieën die worden gebruikt voor het trainen van de classificatie (honden en pony's), buiten het welk percentage het model ophalen juist? 99 juiste labels buiten 100 afbeeldingen biedt een precisie van 99%.|
|__Intrekken__|Buiten-alle afbeeldingen die moeten correct ingedeeld, hoeveel is uw classificatie identificatie correct? Terughalen van 100% betekent dat als er 38 aquaduct afbeeldingen in de installatiekopieën die zijn gebruikt voor het trainen van de classificatie, de classificatie 38 honden gevonden.|

## <a name="next-steps"></a>Volgende stappen

[Testen en het model opnieuw trainen](test-your-model.md)

