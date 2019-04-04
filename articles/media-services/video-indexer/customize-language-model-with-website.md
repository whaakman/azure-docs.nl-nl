---
title: De website van de Video Indexer gebruiken om aan te passen een taalmodel - Azure
titlesuffix: Azure Media Services
description: In dit artikel laat zien hoe een taalmodel met de Video Indexer-website aanpassen.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 02/10/2019
ms.author: anzaman
ms.openlocfilehash: 7b81b53c03104023823bef75beb4ac6077feede7
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58918685"
---
# <a name="customize-a-language-model-with-the-video-indexer-website"></a>Een taalmodel met de Video Indexer-website aanpassen

Video Indexer kunt u aangepaste taalmodellen voor het aanpassen van spraakherkenning door te uploaden aanpassing van woordenlijsten tekst, namelijk de tekst uit het domein waarvan u de engine om aan te passen dat wilt aan de woordenlijst maken. Nadat u uw model te trainen, kunt u nieuwe woorden die voorkomen in de aanpassing van woordenlijsten tekst wordt herkend. 

Zie voor een gedetailleerd overzicht en aanbevolen procedures voor het aangepaste taalmodellen [aanpassen van een taalmodel met Video Indexer](customize-language-model-overview.md).

U kunt de Video Indexer-website maken en bewerken van aangepaste taalmodellen in uw account, zoals beschreven in dit onderwerp. U kunt ook de API, zoals beschreven in [aanpassen taalmodel met behulp van API's](customize-language-model-with-api.md).

## <a name="create-a-language-model"></a>Een taalmodel maken

1. Ga naar de [Video Indexer](https://www.videoindexer.ai/)-website en meld u aan.
2. Voor het aanpassen van een model in uw account, klikt u op de **modelaanpassing inhoud** knop in de rechterbovenhoek van de pagina.

   ![Inhoudsmodel aanpassen](./media/content-model-customization/content-model-customization.png)

3. Selecteer de **taal** tabblad.

    U ziet een lijst met ondersteunde talen. 

    ![Taalmodel aanpassen](./media/customize-language-model/customize-language-model.png)

4. Onder de taal die u wilt, klikt u op **toevoegen model**.
5. Typ in de naam voor het taalmodel en druk op enter.

    Hiermee maakt u het model en biedt de mogelijkheid om tekstbestanden uploaden naar het model.
6. Als u wilt toevoegen een tekstbestand, klikt u op **Add file**. Hiermee opent u de Verkenner.

7. Navigeer naar en selecteer het tekstbestand. U kunt meerdere bestanden toevoegen aan een taalmodel.

    U kunt ook een tekstbestand toevoegen door te klikken op de **...**  selecteren en de knop aan de rechterkant van het taalmodel **Add file**.
8. Wanneer u klaar bent voor het uploaden van de tekstbestanden, klikt u op de groene **Train** optie.

    ![Taalmodel van de trein](./media/customize-language-model/train-model.png)

De trainingsproces kan enkele minuten duren. Nadat de training is voltooid, ziet u **Trained** naast het model. U kunt bekijken, downloaden en het bestand verwijderen uit het model.

![Getrainde taalmodel](./media/customize-language-model/preview-model.png)

### <a name="using-a-language-model-on-a-new-video"></a>Met behulp van een taalmodel op een nieuwe video

Voor het gebruik van uw taalmodel op een nieuwe video, moet u een van de volgende handelingen:

* Klik op de **uploaden** knop boven aan de pagina 

    ![Uploaden](./media/customize-language-model/upload.png)
* Uw audio- of -bestand in de cirkel te verwijderen of bladert u naar het bestand

    ![Uploaden](./media/customize-language-model/upload2.png)

Zodoende kunt u de optie te selecteren de **Video source-taal**. Klik op de vervolgkeuzelijst en selecteer een taalmodel die u hebt gemaakt in de lijst. Er moet de taal van uw taalmodel en de naam die u hebt opgegeven tussen haakjes.

Klik op de **uploaden** onder aan de pagina en uw nieuwe video worden geïndexeerd met behulp van uw taalmodel.

### <a name="using-a-language-model-to-reindex"></a>Met behulp van een taalmodel opnieuw indexeren

Als u uw taalmodel wilt indexeren van een video in uw verzameling, gaat u naar uw **Account video's** op de [Video Indexer](https://www.videoindexer.ai/) startpagina en houd de muis boven de naam van de video die u wilt opnieuw indexeren.

Ziet u opties voor het bewerken van uw video, verwijdert u video en indexeren van uw video. Klik op de optie voor het indexeren van uw video.

![Reindex](./media/customize-language-model/reindex1.png)

Dit biedt u de optie te selecteren de **Video source-taal** opnieuw indexeren van uw video. Klik op de vervolgkeuzelijst en selecteer een taalmodel die u hebt gemaakt in de lijst. Er moet de taal van uw taalmodel en de naam die u hebt opgegeven tussen haakjes.

![Reindex](./media/customize-language-model/reindex.png)

Klik op de **opnieuw indexeren** knop en de video wordt worden geïndexeerd met u taalmodel.

## <a name="edit-a-language-model"></a>Een taalmodel bewerken

U kunt een taalmodel bewerken door de naam ervan wijzigen en verwijderen van bestanden uit het toevoegen van bestanden aan het.

Als u bestanden toevoegen of uit het taalmodel verwijderen, hebt u met het trainen van het model opnieuw door te klikken in het groen **trainen** optie.

### <a name="rename-the-language-model"></a>Wijzig de naam van het taalmodel

U kunt de naam van het taalmodel wijzigen door te klikken op **...**  model aan de rechterkant van de taal en selecteer **naam**. 

Type in de nieuwe naam en hits invoeren.

### <a name="add-files"></a>Bestanden toevoegen

Als u wilt toevoegen een tekstbestand, klikt u op **Add file**. Hiermee opent u de Verkenner.

Navigeer naar en selecteer het tekstbestand. U kunt meerdere bestanden toevoegen aan een taalmodel.

U kunt ook een tekstbestand toevoegen door te klikken op de **...**  selecteren en de knop aan de rechterkant van het taalmodel **Add file**.

### <a name="delete-files"></a>Bestanden verwijderen

Als u wilt een bestand verwijderen uit het taalmodel, klikt u op de **...**  aan de rechterkant van het tekstbestand en selecteer **verwijderen**. Hiermee wordt een nieuw venster aangegeven dat de verwijdering niet ongedaan worden gemaakt. Klik op de **verwijderen** optie in het nieuwe venster.

Deze actie wordt het bestand volledig verwijderd uit het taalmodel.

## <a name="delete-a-language-model"></a>Een taalmodel verwijderen

Als een taalmodel verwijderen uit uw account, klikt u op de **...**  aan de rechterkant van het taalmodel en selecteer **verwijderen**.

Hiermee wordt een nieuw venster aangegeven dat de verwijdering niet ongedaan worden gemaakt. Klik op de **verwijderen** optie in het nieuwe venster.

Deze actie verwijdert het taalmodel volledig uit uw account. Een video die is met behulp van het verwijderde taalmodel blijft de dezelfde index totdat u de video opnieuw indexeren. Als u de video opnieuw indexeren, kunt u een nieuwe taalmodel toewijzen aan de video. Video Indexer wordt gebruik anders de standaardmodel opnieuw indexeren van de video. 

## <a name="customize-language-models-by-correcting-transcripts"></a>Taal-modellen aanpassen door te corrigeren transcripties

Video Indexer biedt ondersteuning voor automatische aanpassing van taal modellen op basis van de gebruikers van de werkelijke correcties in de transcripties van hun video's aanbrengen.

1. Als u een transcript correcties, opent u de video die u wilt bewerken uit uw Account-video's. Selecteer de **tijdlijn** tabblad.

    ![Taalmodel aanpassen](./media/customize-language-model/timeline.png)
1. Klik op het potloodpictogram om te bewerken van het transcript van uw transcriptie. 

    ![Taalmodel aanpassen](./media/customize-language-model/edits.png)

    Video Indexer bevat alle regels die door u worden opgelost in de schrijffouten van uw video en automatisch naar een tekstbestand met de naam 'van transcript bewerkingen' toegevoegd. Deze wijzigingen worden gebruikt voor het specifieke taal-model dat is gebruikt in deze video kunt indexeren om opnieuw te trainen. 
    
    Als u een taalmodel opgeeft hebt bij het indexeren van deze video, worden alle bewerkingen voor deze video worden opgeslagen in een standaard taalmodel met de naam Account aanpassingen binnen de gedetecteerde taal van de video. 
    
    Als meerdere wijzigingen zijn aangebracht aan dezelfde regel, wordt alleen de laatste versie van de herstelde regel worden gebruikt voor het bijwerken van het taalmodel.  
    
    > [!NOTE]
    > Alleen tekstuele correcties worden gebruikt voor het aanpassen. Dit betekent dat correcties die geen betrekking op werkelijke woorden (bijvoorbeeld leestekens of spaties) zijn niet opgenomen. 
    
1. Hier ziet u transcript correcties weergegeven in het tabblad van de taal van de pagina inhoudsmodel aanpassen.

    ![Taalmodel aanpassen](./media/customize-language-model/customize.png)

   Als u wilt zoeken in het bestand 'van transcript bewerkingen' voor elk van uw taalmodellen, klikt u op om dit te openen. 

    ![Uit transcriptbewerkingen](./media/customize-language-model/from-transcript-edits.png)

## <a name="next-steps"></a>Volgende stappen

[Taalmodel met behulp van API's aanpassen](customize-language-model-with-api.md)
