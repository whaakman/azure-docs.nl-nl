---
title: Het bouwen van een classificatie - Custom Vision Service
titlesuffix: Azure Cognitive Services
description: Informatie over het gebruik van de Custom Vision Service om te maken van een classificatie voor die objecten in foto's kan onderscheiden.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 05/02/2018
ms.author: anroth
ms.openlocfilehash: 998900e72511a95336e4a94289c794e2a8e59feb
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364233"
---
# <a name="how-to-build-a-classifier-with-custom-vision"></a>Over het bouwen van een classificatie met Custom Vision

Voor het gebruik van de Custom Vision Service, moet u eerst een classificatie maken. In dit document, informatie over het bouwen van een classificatie via uw webbrowser.

## <a name="prerequisites"></a>Vereisten

Als u wilt een classificatie maken, moet u eerst hebben:

- Een geldige [Microsoft-account](https://account.microsoft.com/account) of een Azure Active Directory OrgID ('account werk of school '), zodat u zich bij customvision.ai aanmelden kunt en aan de slag.

    > [!IMPORTANT] 
    > De aanmelding OrgID voor gebruikers van Azure Active Directory (Azure AD) [nationale clouds](https://www.microsoft.com/en-us/trustcenter/cloudservices/nationalcloud) wordt momenteel niet ondersteund.

- Een reeks afbeeldingen met het trainen van uw classificatie (met een minimum van 30 afbeeldingen per tag).

- Een paar afbeeldingen voor het testen van uw classificatie nadat de classificatie wordt getraind.

- Optioneel: Een Azure-abonnement is gekoppeld aan uw Microsoft-Account of OrgID. Als u geen Azure-abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

    > [!IMPORTANT]
    > Zonder een Azure-abonnement, worden alleen maken __beperkte proefversie__ projecten. Als u een Azure-abonnement hebt, wordt u gevraagd om te maken van Custom Vision Service trainen en voorspellen resources in de [Azure-portal](https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision) tijdens het maken van het project.   

## <a name="create-a-new-project"></a>Een nieuw project maken

Als u wilt een nieuw project maakt, gebruikt u de volgende stappen uit:

1. Navigeer in uw webbrowser naar de [Custom Vision webpagina](https://customvision.ai). Selecteer __aanmelden__ om te beginnen met gebruik van de service.

    ![Afbeelding van de aanmeldingspagina opgeven](./media/getting-started-build-a-classifier/custom-vision-web-ui.png)

    > [!NOTE]
    > Nadat u zich bij Custom Vision Service aanmelden, krijgt u een lijst met projecten. Buiten-projecten twee "beperkte proefversie' voor het testen zijn projecten gekoppeld aan een Azure-Resource. Als u een Azure-gebruiker bent, ziet u alle projecten die zijn gekoppeld aan [Azure-Resources](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#grant-access-to-resources) waartoe u toegang hebt. 

2. Voor het maken van uw eerste project, selecteer **nieuw Project**. U wordt gevraagd voor uw eerste project akkoord gaan met de voorwaarden van de Service. Schakel het selectievakje in en selecteer vervolgens de **ik ga akkoord** knop. De **nieuw project** in het dialoogvenster wordt weergegeven.

    ![Het dialoogvenster Nieuw project bevat de velden voor de naam, beschrijving en domeinen.](./media/getting-started-build-a-classifier/new-project.png)

3. Voer een naam en een beschrijving op voor het project. Selecteer vervolgens een van de beschikbare domeinen. Elk domein optimaliseert de classificatie voor specifieke typen van installatiekopieën, zoals beschreven in de volgende tabel:

    |Domain|Doel|
    |---|---|
    |__Algemene__| Geoptimaliseerd voor een grote verscheidenheid aan taken voor bestandsclassificatie installatiekopie. Als geen van de andere domeinen geschikt zijn, of u niet zeker weet welk domein om te kiezen, selecteert u het algemene domein. |
    |__Voedsel__|Geoptimaliseerd voor foto's van maaltijden zoals u ze in een restaurant zou zien. Als u wilt voor het classificeren van foto's van individuele vruchten of fruit, gebruikt u het domein Food.|
    |__Oriëntatiepunten__|Geoptimaliseerd voor het herkennen van oriëntatiepunten, natuurlijke en kunstmatige. Dit domein werkt het beste als de oriëntatiepunt duidelijk zichtbaar zijn in de foto is. Dit domein werkt, zelfs als de oriëntatiepunt iets door mensen in het zicht van deze vorm.|
    |__Detailhandel__|Geoptimaliseerd voor afbeeldingen die in een winkelwagen catalogus of webwinkels zijn gevonden. Als u classificeren van hoge precisie tussen japonnen, broeken en shirt wilt, gebruikt u dit domein.|
    |__Volwassene__|Geoptimaliseerd voor de inhoud voor volwassenen en niet-volwassene inhoud beter definiëren. Bijvoorbeeld, als u blokkeren van installatiekopieën van mensen in bad kleuren wilt, kunt dit domein u een aangepaste classificatie om dit te doen maken.|
    |__Compact domeinen__| Geoptimaliseerd voor de beperkingen van realtime classificatie op mobiele apparaten. De modellen die worden gegenereerd door compact domeinen kunnen worden geëxporteerd als lokaal wilt uitvoeren.|

    Als u wilt, kunt u het domein later wijzigen.

4. Selecteer een resourcegroep. De resourcegroep vervolgkeuzelijst ziet u alle van de Azure-resourcegroepen die een Resource Custom Vision Service bevatten. U kunt ook selecteren maken __beperkte proefversie__. De beperkte proefversie vermelding is de enige resourcegroep is die een niet-Azure-gebruiker is mogelijk om uit te kiezen.

    Voor het maken van het project, selecteer __project maken__.

## <a name="upload-and-tag-images"></a>Uploaden en tag installatiekopieën

1. U kunt afbeeldingen toevoegen aan de classificatie met de __afbeeldingen toevoegen__ knop en selecteer vervolgens __door lokale bestanden bladeren__. Selecteer __Open__ naar tagging.

    > [!TIP]
    > Na het selecteren van installatiekopieën, moet u deze taggen. Het label wordt toegepast op de groep van installatiekopieën die u hebt geselecteerd om te uploaden, zodat het misschien eenvoudiger om afbeeldingen te uploaden door de labels die u wilt gebruiken. U kunt ook de code voor de geselecteerde kopieën wijzigen nadat ze zijn gelabeld en geüpload.

    > [!TIP]
    > Uploaden van afbeeldingen met verschillende camerahoeken, belichting, achtergrond, typen, stijlen, groepen, grootten, enzovoort. Verschillende typen foto's gebruiken om ervoor te zorgen dat uw classificatie niet is gericht en goed kunt generaliseren.

    Custom Vision Service accepteert trainingsafbeeldingen in JPG, PNG, en BMP-indeling, maximaal 6 MB per afbeelding. (Voorspelling installatiekopieën kunnen maximaal 4 MB per afbeelding zijn). U wordt aangeraden dat installatiekopieën 256 pixels aan de rand van het kortste zijn. Alle installatiekopieën die korter zijn dan 256 pixels aan de rand van het kortste worden opgeschaald door Custom Vision Service.

    ![Het besturingselement van de installatiekopieën toevoegen wordt weergegeven in de linkerbovenhoek en als een knop op onderaan in het midden.](./media/getting-started-build-a-classifier/add-images01.png)

    >[!NOTE] 
    > De REST-API kan worden gebruikt voor de trainingsafbeeldingen laden van URL's.

2. Voer tekst in om de code, de __mijn Tags__ veld en gebruik vervolgens de __+__ knop. Als u wilt de installatiekopieën uploaden en het taggen hiervan, gebruikt u de __[aantal] bestanden uploaden__ knop. U kunt meer dan één label toevoegen aan de installatiekopieën. 

    > [!NOTE]
    > Het uploaden van afhankelijk van het aantal en de grootte van afbeeldingen die u hebt geselecteerd.

    ![Afbeelding van de pagina tag en uploaden](./media/getting-started-build-a-classifier/add-images03.png)

3. Selecteer __gedaan__ zodra de afbeeldingen zijn geüpload.

    ![De voortgangsbalk wordt aangegeven dat alle taken zijn voltooid.](./media/getting-started-build-a-classifier/add-images04.png)

4. Als u wilt een andere set installatiekopieën uploaden, Ga terug naar stap 1. Bijvoorbeeld, als u wilt onderscheid maken tussen honden en pony's, uploaden en afbeeldingen van pony's taggen.

## <a name="train-and-evaluate-the-classifier"></a>Trainen en evalueren van de classificatie

De classificatie trainen, selecteer de **trainen** knop.

![De knop van de trein wordt rechts boven aan het browservenster.](./media/getting-started-build-a-classifier/train01.png)

Het duurt slechts enkele minuten met het trainen van de classificatie. Informatie over het trainingsproces wordt gedurende deze periode worden weergegeven.

![De knop van de trein wordt rechts boven aan het browservenster.](./media/getting-started-build-a-classifier/train02.png)

Na de training, de __prestaties__ wordt weergegeven. De precisie en terughalen indicatoren vertellen u hoe goed die uw classificatie is, op basis van het automatische testen. Custom Vision Service maakt gebruik van de installatiekopieën die u hebt ingediend voor training voor het berekenen van deze getallen met behulp van een proces met de naam [k-Vouw cross validatie](https://en.wikipedia.org/wiki/Cross-validation_(statistics)).

![De resultaten van de training tonen de algehele precisie en intrekken en de nauwkeurigheid en intrekken voor elk label in de classificatie.](./media/getting-started-build-a-classifier/train03.png)

> [!NOTE] 
> Telkens wanneer u selecteert de **Train** knop, maakt u een nieuwe versie van de classificatie. U vindt uw oude iteraties in de **prestaties** tabblad, en u kunt verwijderen waarvan mogelijk verouderd. Wanneer u een iteratie verwijdert, krijgt u uiteindelijk alle installatiekopieën die gekoppeld aan deze zijn verwijderen.

De classificatie die gebruikmaakt van alle installatiekopieën om een model waarin elke tag te maken. Als u wilt testen van de kwaliteit van het model, probeert de classificatie elke installatiekopie van het model om te zien wat het model wordt gevonden.

De eigenschappen van de classificatie-resultaten worden weergegeven.

|Termijn|Definitie|
|---|---|
|__Precisie__|Wanneer het classificeren van een afbeelding, hoe waarschijnlijk is uw classificatie voor het classificeren van de installatiekopie goed? Uit alle installatiekopieën die worden gebruikt voor het trainen van de classificatie (honden en pony's), welk percentage het model krijg correct? 99 juiste tags uit 100 afbeeldingen biedt een precisie van 99%.|
|__Intrekken__|Uit alle installatiekopieën die moeten correct ingedeeld, hoeveel uw classificeerder leren herkennen correct? Een terugroepingspercentage van 100% betekent dat als er 38 gebruiken graag onze eigen installatiekopieën in de installatiekopieën die zijn gebruikt voor het trainen van de classificatie, classificatie 38 honden gevonden.|

## <a name="next-steps"></a>Volgende stappen

[Testen en het model opnieuw trainen](test-your-model.md)

