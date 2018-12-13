---
title: De website van de Video Indexer gebruiken voor het aanpassen van een persoon model - Azure
titlesuffix: Azure Media Services
description: In dit artikel laat zien hoe een persoon-model met de Video Indexer-website aanpassen.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 12/03/2018
ms.author: anzaman
ms.openlocfilehash: 321db75b1a286e7de1e38ed3b382ee7dc9fe5902
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53285380"
---
# <a name="customize-a-person-model-with-the-video-indexer-website"></a>Een model van de persoon met de Video Indexer-website aanpassen

Video Indexer biedt ondersteuning voor gezichtsdetectie en herkenning van beroemdheden voor video-inhoud. De functie van de herkenning van beroemdheden bevat informatie over ongeveer één miljoen gezichten op basis van veelvuldig opgevraagde gegevensbron zoals IMDB Wikipedia en bovenste LinkedIn testteam. Gezichten die niet worden herkend door de functie voor de herkenning van beroemdheden worden gevonden. ze blijven echter naamloze. Nadat u uw video met Video Indexer uploaden en resultaten van de vorige ophalen, kunt u teruggaan en de naam van de gezichten die niet zijn herkend. Wanneer u een gezicht met de naam van een label, het gezicht en de naam toegevoegd naar persoon-model van uw account. Video Indexer herkent vervolgens deze gezicht in uw toekomstige video's en de afgelopen video's.

De website Video Indexer kunt u gezichten die zijn gedetecteerd in een video bewerken, zoals beschreven in dit onderwerp. U kunt ook de API, zoals beschreven in [aanpassen persoon model met behulp van API's](customize-person-model-with-api.md).

## <a name="edit-a-face"></a>Een gezicht bewerken

> [!NOTE]
> Namen zijn uniek is voor in de modellen van een persoon, dus als u de naam van twee verschillende dezelfde naam vlakken, Video Indexer weergaven van de gezichten als dezelfde persoon en ze nadat u uw video opnieuw indexeren convergeert wel ligt. Als u ziet dat Video Indexer meerdere verschillende exemplaren van de dezelfde gezichten gedetecteerd, zodat ze dezelfde naam en indexeren van uw video.
> U kunt een gezicht die Video Indexer wordt herkend als een beroemdheden met een nieuwe naam bijwerken. De nieuwe naam die u toewijst voorrang krijgt boven de ingebouwde beroemdheden-herkenning.

1. Ga naar de [Video Indexer](https://www.videoindexer.ai/)-website en meld u aan.
2. Zoeken naar een video die u wilt weergeven en bewerken in uw account.
3. Als u wilt een gezicht in uw video bewerken, gaat u naar de **Insights** tabblad en klik op het potloodpictogram in de rechterbovenhoek van het venster.

    ![Persoon model aanpassen](./media/customize-face-model/customize-face-model.png)

4. Klik op een van de gedetecteerde gezichten en wijzig de namen van 'Onbekende #X' (of de naam die eerder was toegewezen aan het gezicht).
5. Nadat u hebt ingevoerd in de nieuwe naam, klik op het vinkje naast de nieuwe naam. Dit wordt de nieuwe naam opslaan en herkennen en alle exemplaren van deze gezicht in de andere huidige video's en in de toekomst video's die u uploadt een naam. Herkenning van het gezicht in de andere huidige video's kan enige tijd in te voeren, omdat dit een batchproces duren. 

    ![Sla de update](./media/customize-face-model/save-update.png)

## <a name="delete-a-face"></a>Verwijderen van een gezicht

Als u wilt een gedetecteerde gezicht in de video verwijderen, gaat u naar de **Insights** deelvenster en klik op het potloodpictogram in de rechterbovenhoek van het deelvenster. Klik op de **verwijderen** optie onder de naam van het gezicht. Hiermee verwijdert u die face van de video worden gedetecteerd. Het gezicht wordt nog steeds worden gedetecteerd in de andere video's waarin ze worden weergegeven, maar u kunt het gezicht uit video's ook verwijderen nadat ze zijn geïndexeerd. Het gezicht blijft ook aanwezig zijn in uw account persoon model als u de naam voordat deze worden verwijderd.

## <a name="next-steps"></a>Volgende stappen

[Persoon model met behulp van API's aanpassen](customize-person-model-with-api.md)
