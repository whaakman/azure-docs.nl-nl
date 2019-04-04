---
title: De website van de Video Indexer gebruiken voor het aanpassen van een persoon model - Azure
titlesuffix: Azure Media Services
description: In dit artikel laat zien hoe een persoon-model met de Video Indexer-website aanpassen.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 03/19/2019
ms.author: anzaman
ms.openlocfilehash: 8dd535d97e40fe1dd4358d782db60940af1dd95d
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58892833"
---
# <a name="customize-a-person-model-with-the-video-indexer-website"></a>Een model van de persoon met de Video Indexer-website aanpassen

Video Indexer biedt ondersteuning voor herkenning van beroemdheden voor video-inhoud. De functie van de herkenning van beroemdheden bevat informatie over ongeveer één miljoen gezichten op basis van veelvuldig opgevraagde gegevensbron zoals IMDB Wikipedia en bovenste LinkedIn testteam. Zie voor een gedetailleerd overzicht [aanpassen van een persoon-model in Video Indexer](customize-person-model-overview.md).

De website Video Indexer kunt u gezichten die zijn gedetecteerd in een video bewerken, zoals beschreven in dit onderwerp. U kunt ook de API, zoals beschreven in [aanpassen van een persoon-model met behulp van API's](customize-person-model-with-api.md).

## <a name="central-management-of-person-models-in-your-account"></a>Centraal beheer van modellen van de persoon in uw account

1. Als u wilt weergeven, bewerken, en de modellen persoon in uw account verwijdert, bladert u naar de Video Indexer-website en meld u aan.
2. Klik op de knop voor het aanpassen van het inhoudsmodel in de rechterbovenhoek van de pagina.

    ![Aanpassing van inhoudsmodel](./media/customize-face-model/content-model-customization.png)
3. Selecteer het tabblad gebruikers.

    Het model van de persoon die standaard ziet u in uw account. De persoon die standaard-model bevat gezichten die u mogelijk hebt bewerkt of gewijzigd in het inzicht van uw video's waarvoor dat u een aangepast model van de persoon niet hebt opgegeven tijdens het indexeren. 

    Als u een andere persoon modellen gemaakt, worden deze weergegeven op deze pagina ook. 

    ![Aanpassing van inhoudsmodel](./media/customize-face-model/content-model-customization-people-tab.png)

## <a name="create-a-new-person-model"></a>Een nieuwe persoon-model maken

1. Klik op de **+ toevoegen model** knop.

    ![Een nieuwe persoon toevoegen](./media/customize-face-model/add-new-person.png)
2. Voer de naam van het model en klik op de knop met het vinkje naast de naam.

    ![Een nieuwe persoon toevoegen](./media/customize-face-model/add-new-person2.png)

    U kunt een nieuwe persoon-model hebt gemaakt. U kunt nu gezichten toevoegen aan het nieuwe model van de persoon.
3. Klik op de menuknop lijst en kies **+ toevoegen persoon**.

    ![Een nieuwe persoon toevoegen](./media/customize-face-model/add-new-person3.png)
    
## <a name="add-a-new-person-to-a-person-model"></a>Een nieuwe persoon toevoegen aan een persoon-model

> [!NOTE]
> Video Indexer kunt u het toevoegen van meerdere personen met dezelfde naam in het model van een persoon. Het is echter raadzaam dat u unque namen geven aan elke persoon in uw model voor gebruiksgemak en duidelijkheid.

1. Als u wilt een nieuw gezicht toevoegen aan een persoon-model, klikt u op de menuknop lijst naast het model persoon die u wilt toevoegen van het gezicht.
1. Klik op **+ toevoegen persoon** in het menu.

    ![Een nieuw gezicht toevoegen aan de persoon](./media/customize-face-model/add-new-face.png)
 
    Een pop-upvenster vraagt u om de details van de persoon in te vullen. Typ de naam van de persoon en klik op de knop controleren.

    ![Een nieuw gezicht toevoegen aan de persoon](./media/customize-face-model/add-new-face2.png)
 
Vervolgens kunt u kiezen in de Verkenner of slepen en neerzetten van de face-installatiekopieën van het gezicht. Video Indexer neemt alle standard-installatiekopie bestandstypen (ex: JPG, PNG, en meer).

Video Indexer zou het mogelijk voor het detecteren van exemplaren van deze persoon in de toekomst video's die u index en de huidige video's die u tot nu toe was geïndexeerd, samen met behulp van de persoon model waaraan u deze nieuwe gezicht toegevoegd. Herkenning van de persoon in uw huidige video's kan even om te worden uitgevoerd, omdat dit een batchproces.


## <a name="rename-a-person-model"></a>Wijzig de naam van een persoon-model

U kunt een model persoon in uw account met inbegrip van het model van de persoon die standaard wijzigen. Zelfs als u uw model van de persoon die standaard de naam, wordt deze nog steeds dienen als het model van de persoon die standaard in uw account.

1. Klik op de lijst met menu-knop naast de persoon-model dat u wilt wijzigen.
2. Klik op **naam** in het menu.

    ![Wijzig de naam van een persoon](./media/customize-face-model/rename-person.png)
3. Klik op de huidige naam van het model en typ de naam van uw nieuwe.

    ![Wijzig de naam van een persoon](./media/customize-face-model/rename-person2.png)
4. Klik op de knop controleren voor uw model moet worden gewijzigd.

## <a name="delete-a-person-model"></a>Het model van een persoon verwijderen

U kunt elke persoon-model dat u hebt gemaakt in uw account verwijderen. U kunt uw model van de persoon die standaard echter niet verwijderen.

1. Klik op **verwijderen** in het menu.

    ![Een persoon verwijderen](./media/customize-face-model/delete-person.png)
    
    Een pop-upvenster wordt weergegeven en wordt u gewaarschuwd dat deze actie verwijdert het persoon-model en alle mensen en bestanden die deze bevat. Deze actie kan niet ongedaan worden gemaakt. 

    ![Een persoon verwijderen](./media/customize-face-model/delete-person2.png)
1. Als u zeker weet, klikt u nogmaals op verwijderen.

> [!NOTE]
> De mogelijkheid om u bij het bijwerken van de namen van de gezichten die worden weergegeven in de video wordt geen ondersteuning voor de bestaande video's die zijn geïndexeerd met behulp van deze persoon-model (nu verwijderd). U kunt zich de namen van gezichten wordt uitgevoerd in deze video's bewerken nadat u deze met behulp van een andere persoon model indexeren. Als u opnieuw zonder een model persoon op te geven, wordt het standaardmodel worden gebruikt. 

## <a name="manage-existing-people-in-a-person-model"></a>Bestaande gebruikers in een persoon model beheren

Om te kijken naar de inhoud van een van uw modellen persoon, klikt u op de pijl naast de naam van het model van de persoon.
De vervolgkeuzelijst ziet u alle personen in die specifieke persoon-model. Als u op de knop naast elk van de mensen die lijst klikt, ziet u beheren, wijzig de naam en opties voor verwijderen.  

![Een nieuw gezicht toevoegen aan de persoon](./media/customize-face-model/manage-people.png)

### <a name="rename-a-person"></a>Wijzig de naam van een persoon

1. Wijzig de naam van een persoon in uw model persoon, klik op de menuknop lijst en kies **naam** in het lijstmenu.
1. Klik op de huidige naam van de persoon en voert u de nieuwe naam.
1. Klik op de knop controleren en de naam van de persoon die zal worden gewijzigd.

### <a name="delete-a-person"></a>Een persoon verwijderen

1. Als u wilt een persoon verwijderen uit het model persoon, klik op de menuknop lijst en kies **verwijderen** in het lijstmenu.
1. Een pop-upvenster ziet u dat de persoon die door deze actie worden verwijderd en dat deze actie kan niet ongedaan gemaakt worden.
1. Klik nogmaals op verwijderen en Hiermee wordt de persoon verwijderd uit het model van de persoon.

### <a name="manage-a-person"></a>Beheren van een persoon

Als u klikt op **beheren**, ziet u de gezichten die van deze persoon-model wordt getraind. Deze gezichten afkomstig van instanties van die persoon in video's die gebruikmaken van dit model van de persoon of installatiekopieën die u handmatig hebt geüpload. 

U kunt meer gezichten toevoegen aan de persoon die door te klikken op installatiekopieën toevoegen.

U kunt het deelvenster beheren gebruiken om te wijzigen van de persoon en de persoon verwijderen uit het model van de persoon.

## <a name="use-a-person-model-to-index-a-video"></a>Een persoon-model gebruiken om te indexeren van een video

Een persoon-model kunt u uw nieuwe video kunt indexeren het model van de persoon die tijdens het uploaden van de video toe te wijzen.

Voor het gebruik van het model van de persoon die op een nieuwe video, het volgende doen:

1. Klik op de **uploaden** knop boven aan de pagina.

    ![Uploaden](./media/customize-face-model/upload.png)
1. Verwijderen van uw video-bestand in de cirkel of bladert u naar het bestand.
1. Klik op de pijl van de geavanceerde opties.

    ![Uploaden](./media/customize-face-model/upload2.png)
1. Klik op de vervolgkeuzelijst en selecteer het model voor de persoon die u hebt gemaakt.

    ![Uploaden](./media/customize-face-model/upload3.png)
1. Klik op de optie uploaden in de onderaan de pagina en uw nieuwe video worden geïndexeerd met behulp van uw model persoon.

Als u een model persoon niet tijdens het uploaden opgeeft, wordt de video die gebruikmaakt van het model van de persoon die standaard in uw account indexeren in Video Indexer.

## <a name="use-a-person-model-to-reindex-a-video"></a>Indexeren van een video met behulp van een persoon-gegevensmodel 

Als u een model persoon wilt indexeren van een video in uw verzameling, gaat u naar uw Account-video's op de startpagina van de Video Indexer en plaats de muisaanwijzer boven de naam van de video die u wilt opnieuw indexeren.

Ziet u opties om te bewerken, verwijderen en opnieuw indexeren van uw video. 

1. Klik op de optie voor het indexeren van uw video.

    ![Reindex](./media/customize-face-model/reindex.png)

    U kunt nu de persoon-model voor het indexeren van uw video selecteren.
1. Klik op de vervolgkeuzelijst en selecteer het model voor de persoon die u wilt gebruiken. 

    ![Reindex](./media/customize-face-model/reindex2.png)

1. Klik op de **indexeren** knop en de video wordt worden geïndexeerd met behulp van uw model persoon.

Alle nieuwe wijzigingen die u aanbrengt in de gezichten gedetecteerd en in de video die u zojuist hebt opnieuw geïndexeerd wordt herkend worden, opgeslagen in het model persoon die u gebruikt voor het indexeren van de video.

## <a name="managing-people-in-your-videos"></a>Personen in uw video's beheren

U kunt de gezichten die worden gedetecteerd en de mensen die zijn herkend in de video's die u met de index door te bewerken en verwijderen van gezichten beheren.

Een gezicht, verwijdert, wordt een specifieke vlak van het inzicht van de video.

Een gezicht bewerkt, wijzigt de naam van een gezicht die is gedetecteerd en mogelijk worden herkend in uw video. Wanneer u een gezicht in uw video bewerkt, wordt deze naam wordt opgeslagen als een persoon-vermelding in het model persoon die is toegewezen aan de video tijdens het uploaden en te indexeren.

Als u een persoon-model niet naar de video tijdens het uploaden toewijst wel, wordt de bewerking wordt opgeslagen in van uw account standaard persoon model.

### <a name="edit-a-face"></a>Een gezicht bewerken


> [!NOTE]
> Als een persoon-model twee of meer verschillende mensen met dezelfde naam heeft, wordt het niet mogelijk om te taggen die naam binnen de video's die gebruikmaken van die persoon-model. Alleen mogelijk wijzigingen aanbrengen in de mensen die deze naam op het tabblad gebruikers van de pagina voor het aanpassen van het inhoudsmodel in Video Indexer delen. Het verdient daarom unieke namen te geven aan elke persoon in uw model persoon.

1. Blader naar de Video Indexer-website en meld u aan.
1. Zoeken naar een video die u wilt weergeven en bewerken in uw account.
1. Als u wilt een gezicht in uw video bewerken, gaat u naar het tabblad inzichten en klik op het potloodpictogram in de rechterbovenhoek van het venster.

    ![Een gezicht in uw video bewerken](./media/customize-face-model/edit-face.png)
1. Klik op een van de gedetecteerde gezichten en wijzig de namen van 'Onbekende #X' (of de naam die eerder was toegewezen aan het gezicht). 
1. Nadat u hebt ingevoerd in de nieuwe naam, klik op het vinkje naast de nieuwe naam. Dit wordt de nieuwe naam en herkent en namen van alle instanties van deze gezicht in de andere huidige video's en in de toekomst video's die u uploadt. Herkenning van het gezicht in uw huidige video kan even om te worden uitgevoerd, omdat dit een batchproces.

Als u een gezicht naam met de naam van een bestaande persoon in het model persoon die de video wordt gebruikt, wordt dit de gedetecteerde face afbeeldingen in deze video van die persoon met wat al in het model bestaat samenvoegen. Als u een gezicht naam met een volledig nieuwe naam, maakt Hiermee u een nieuwe persoon-vermelding in het model persoon die de video wordt gebruikt. 

    ![Edit a face in your video](./media/customize-face-model/edit-face2.png)

### <a name="delete-a-face"></a>Verwijderen van een gezicht

Als u wilt een gedetecteerde gezicht in de video verwijderen, gaat u naar de Insights-deelvenster en klik op het potloodpictogram in de rechterbovenhoek van het deelvenster. Klik op de optie verwijderen onder de naam van het gezicht. Hiermee verwijdert u die face van de video worden gedetecteerd. Van de persoon face nog steeds worden gedetecteerd in de andere video's waarin deze wordt weergegeven, maar u kunt het gezicht uit video's ook verwijderen nadat ze zijn geïndexeerd. De persoon, blijft als dit probleem had genoemd, ook bestaan in het model persoon die is gebruikt om te indexeren van de video van waaruit u het gezicht verwijderd tenzij u specifiek de persoon uit het model van de persoon verwijderen.

![Verwijderen van een gezicht in de video](./media/customize-face-model/delete-face.png)


## <a name="next-steps"></a>Volgende stappen

[Persoon model met behulp van API's aanpassen](customize-person-model-with-api.md)
