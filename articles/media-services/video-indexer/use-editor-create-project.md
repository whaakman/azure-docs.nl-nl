---
title: De Video Indexer-editor gebruiken om projecten te maken
titlesuffix: Azure Media Services
description: In dit onderwerp ziet u hoe u de Video Indexer-editor gebruiken om projecten te maken.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 04/02/2019
ms.author: juliako
ms.openlocfilehash: a9d6396cab560a201b98497e787af4b6c7c2dabb
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896461"
---
# <a name="use-the-video-indexer-editor-to-create-projects"></a>De Video Indexer-editor gebruiken om projecten te maken

Video Indexer-website, kunt u veel inzicht uw video's te gebruiken: de juiste media-inhoud zoeken, zoekt u de onderdelen dat u geïnteresseerd bent in, en de resultaten gebruiken om een volledig nieuw project te maken. Nadat u hebt gemaakt, het project kan worden weergegeven en van Video Indexer gedownload en in uw eigen bewerken van toepassingen of downstream werkstromen worden gebruikt.

Sommige scenario's waar mogelijk nuttig voor u deze functie zijn: 

* Het maken van de film gemarkeerd voor aanhangwagen.
* Met behulp van oude korte clips van video's in nieuws webcasts.
* Het maken van kortere inhoud voor sociale media.

Dit artikel leest over het maken van een project maken en ook hoe u een project maken van een video in uw account.

## <a name="create-new-project-and-manage-videos"></a>Nieuw project maken en beheren van video 's

1. Ga naar de [Video Indexer](https://www.videoindexer.ai/)-website en meld u aan.
1. Selecteer de **projecten** tabblad. Als u projecten voordat u hebt gemaakt, ziet u al uw andere projecten hier.
1. Klik op **nieuw project maken**.  

    ![Nieuw project](./media/video-indexer-view-edit/new-project.png)
1. Geef uw project een naam door te klikken op het potloodpictogram. De tekst "Naamloos project" vervangen door de projectnaam van uw en klik op de controle.

    ![Nieuw project](./media/video-indexer-view-edit/new-project3.png)
    
### <a name="add-videos-to-the-project"></a>Video's toevoegen aan het project

> [!NOTE]
> Projecten kunnen op dit moment alleen video's die zijn geïndexeerd in dezelfde taal bevatten. Wanneer u een video in één taal selecteert, kunt u de video's in uw account die zich in een andere taal niet toevoegen.

1. Video's die u werken wilt met dit project hiervoor toevoegen **video's toevoegen**.

    U ziet alle video's in uw account en een zoekvak met de tekst "Zoeken voor tekst, trefwoorden of visuele inhoud". Om te zoeken naar video's waarbij een bepaalde persoon, label, merk, trefwoord of gebeurtenis in het transcript en OCR.
    
    Bijvoorbeeld, in de onderstaande afbeelding, we zijn op zoek naar video's waarin het woord 'GitHub'.
    
    ![GitHub](./media/video-indexer-view-edit/github.png)

    U kunt uw resultaten verder filteren door te selecteren **resultaten filteren**. U kunt filteren om video's waarbij een bepaalde persoon weer te geven of om op te geven dat u alleen wilt zien, videoresultaten die een in een bepaalde taal of een specifieke eigenaar hebben. <br/> U kunt ook het bereik van uw query opgeven. Bijvoorbeeld, als u zoeken naar 'GitHub' in de OCR wilt, selecteer **Visual tekst**.

    ![Filteren](./media/video-indexer-view-edit/visual-text.png)

    U kunt meerdere filters van de laag aan uw query. Gebruik de **+** / **-** knoppen aan filters toevoegen/verwijderen. Gebruik **filters wissen** alle filters te verwijderen.
1. Video's toevoegen, selecteert u deze en selecteer vervolgens **toevoegen**.
1. Nu ziet u alle van de video's die u hebt gekozen. Dit zijn de video's van waaruit u wilt selecteren korte clips van uw project.

    U kunt de volgorde van de video's door slepen en neer te zetten of door de menuknop lijst selecteren en klikken op **omlaag** of **omhoog**. In het lijstmenu zich kunt u ook de video verwijderen uit dit project. 

    ![Opnieuw rangschikken](./media/video-indexer-view-edit/rearrange.png)
    
    U hebt de mogelijkheid om toe te voegen aan dit project meer video's op elk gewenst moment door te selecteren **video's toevoegen**. U kunt ook meerdere exemplaren van dezelfde video toevoegen aan uw project. Het is raadzaam om dit te doen als u wilt dat een illustratie van een video en vervolgens een videoclip uit een andere en vervolgens op een andere illustratie van de eerste video weer te geven. 

### <a name="select-clips-to-use-in-your-project"></a>Selecteer clips te gebruiken in uw project

Als u op de pijl-omlaag aan de rechterkant van elke video klikt, wordt u opent u de inzichten in de video die op basis van tijdstempels (korte clips van de video). 

1. Selecteer **inzichten weergeven** om aan te passen welke inzichten die u wilt zien en die u niet wilt zien. 

    ![Inzichten weergeven](./media/video-indexer-view-edit/insights.png)
1. Gebruik het zoekvak met de tekst "Zoeken in het transcript, visual tekst, mensen en labels" voor het maken van query's voor specifieke clips.
1. Filters toevoegen om te verdere details opgeven voor wat u zoekt door het selecteren van schermen **filteropties**.

    ![Filteropties](./media/video-indexer-view-edit/filter-options.png)

    U wilt bijvoorbeeld Zie clips waar GitHub terwijl Donovan Brown op het scherm is wordt genoemd. Hiervoor moet u een filter voor "bevatten" waarvoor 'Gebruikers' toe te voegen als het type inzicht. Vervolgens moet u typt in 'Donovan Brown' in het zoekvak voor het filter.
    
    ![Opnemen](./media/video-indexer-view-edit/include.png)
    
    Als u korte clips waar GitHub wordt vermeld als Donovan Brown is _niet_ op het scherm, zou u alleen het filter "bevatten" wijzigen in een ' ' Uitsluitingsfilter met behulp van de vervolgkeuzelijst. 

1. Een illustratie toevoegen aan uw project door het selecteren van het segment dat u wilt toevoegen. U kunt deze clip Hef de selectie door te klikken op het segment opnieuw uit.
    
    Alle segmenten van een video toevoegen door te klikken op de menuoptie lijst naast de video en vervolgens **selecteert u alle segmenten**. 

    ![Allemaal toevoegen](./media/video-indexer-view-edit/add-all.png)

    U kunt alle van de selectie hiervoor selectie wissen wissen.

> [!TIP]
> Als u te selecteren en uw clips bestellen, kunt u de video in de speler aan de rechterkant van de pagina bekijken. 

![Preview](./media/video-indexer-view-edit/preview.png)

Houd er rekening mee om op te slaan van het project wanneer u wijzigingen door te selecteren **opslaan project**. 

### <a name="render-and-download-the-project"></a>Weergeven en downloaden van het project

> [!NOTE]
> Video Indexer betaalde accounts, heeft het weergeven van uw project coderingskosten kunt terugdringen. Video Indexer-proefaccounts worden beperkt tot 5 uur rendering.

1. Wanneer u klaar bent, zorg ervoor dat uw project is opgeslagen. U kunt nu dit project renderen. Selecteer **weergeven en downloaden**. 

    ![Opslaan](./media/video-indexer-view-edit/save.png)

    Er is een pop-upvenster dat aangeeft dat een bestand worden weergegeven door Video indexer en vervolgens de downloadkoppeling verzenden naar uw e-mailadres is. Selecteer Doorgaan. 
    
    U ziet ook een melding dat het project wordt weergegeven boven op de pagina. Zodra dit is voltooid wordt weergegeven, ziet u een nieuwe melding dat het project is gemaakt. Klik op de melding voor het downloaden van het project. Het project in mp4-indeling worden gedownload.

    ![Rendering gedaan](./media/video-indexer-view-edit/rendering-done.png)

1. U hebt toegang tot opgeslagen projecten uit de **projecten** tabblad. 

    Als u dit project selecteert, ziet u alle inzichten en de tijdlijn van dit project. Als u selecteert **Video-editor**, u kunt wijzigingen aanbrengen in dit project. Bewerkingen omvatten toevoegen of verwijderen van video's en clips of naam van het project.

    ![Video-editor](./media/video-indexer-view-edit/video-editor.png)
     
## <a name="create-a-project-from-your-video"></a>Maak een project van uw video

U kunt een nieuw project maken rechtstreeks vanuit een video in uw account. 

1. Ga naar de **bibliotheek** tabblad van de Video Indexer-website.
1. Open de video die u wilt gebruiken om uw project te maken. Selecteer op de inzichten en de tijdlijn pagina, de **Video-editor** knop.

    Hiermee gaat u naar de pagina die u gebruikt voor het maken van een nieuw project. In tegenstelling tot het nieuwe project ziet u de segmenten van de inzichten voorzien van een tijdstempel van de video die u eerder hebt bewerkt is gestart.

## <a name="see-also"></a>Zie ook

[Video Indexer-overzicht](video-indexer-overview.md)

