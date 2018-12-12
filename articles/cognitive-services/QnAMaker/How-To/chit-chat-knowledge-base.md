---
title: Chit chat toe te voegen aan een kennisdatabase QnA Maker
titleSuffix: Azure Cognitive Services
description: Persoonlijke chit-chat toe te voegen aan uw bot kunt u meer beschrijvende en aantrekkelijke bij het maken van een KB. QnA Maker kunt u eenvoudig een vooraf gevulde set van de bovenste chit-chat, toevoegen aan uw KB.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/24/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 7c54628faba4a3d7a89c1944e2f434a83035707b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53081759"
---
# <a name="add-chit-chat-to-a-knowledge-base"></a>Chit chat toevoegen aan een kennisdatabase

Chit chat toe te voegen aan uw bot kunt u meer beschrijvende en aantrekkelijke. De functie chit chat in QnA maker kunt u eenvoudig een vooraf gevulde set van de bovenste chit-chat, toevoegen aan uw knowledge base (KB). Dit kan een startpunt voor de persoonlijkheid van uw bot, en u bespaart de tijd en kosten van het schrijven van deze helemaal.  

Deze gegevensset heeft ongeveer 100 scenario's van chit chat in de stem van drie personen - de Professional, de vriend de Comic. Kies de persona die van uw bot stem het beste past. De aanvraag voor een gebruiker worden gegeven, probeert QnA Maker moet deze overeenkomen met de meest bekende chit-chat QnA. 

Enkele voorbeelden van de verschillende wensen: <!-- added quotes so acrolinx doesn't score these sentences -->
|Gebruikersquery|De medewerker|De vriend|De Comic|
|--|--|--|--|
|`You are awesome`|`I aim to serve.`|`That's so nice of you!`|`Flattery. I like it.`|
|`Are you hungry?`|`I don't need to eat.`|`I only do food for thought.`|`Eating would require a lot of things I don't have. Like a digestive system. And silverware.`|
|`Sing a song`|`I'm afraid I'm not musically inclined.`|`La la la, tra la la. I'm awesome at this.`|`You can't handle my dulcet tones.`|
|`Will you marry me?`|`I think it's best if we stick to a professional relationship.`|`Aw, that's sweet.`|`Sure. Take me to city hall. See what happens.`|


> [!NOTE]
> Chit chatsupport is alleen beschikbaar in het Engels. 

## <a name="add-chit-chat-during-kb-creation"></a>Chit chat toevoegen tijdens het maken van KB
Tijdens de ontwikkeling van knowledge base, na het toevoegen van de bron-URL en de bestanden, is er een optie voor het toevoegen van chit chat. Kies de persoonlijkheid die u wilt gebruiken als uw base chit chat. Als u niet wilt toevoegen van chit chatten of als u al chit-chat-ondersteuning in uw gegevensbronnen, kiest u **geen**. 
   
![Chit chat toevoegen tijdens het maken](../media/qnamaker-how-to-chit-chat/create-kb-chit-chat.png)

## <a name="add-chit-chat-to-an-existing-kb"></a>Chit chat toevoegen aan een bestaande KB
Selecteer uw KB, en Ga naar de **instellingen** pagina. Er is een koppeling naar alle chit chat-gegevenssets in de juiste **.tsv** indeling. De persoonlijkheid die u wilt downloaden, en vervolgens uploaden als een bron. Zorg ervoor dat u niet de indeling of de metagegevens bewerken wanneer u downloadt en upload het bestand. 
  
![Chit chat toevoegen aan bestaande KB](../media/qnamaker-how-to-chit-chat/add-chit-chat-dataset.png)

## <a name="edit-your-chit-chat-questions-and-answers"></a>Bewerk uw chit chat vragen en antwoorden
Wanneer u uw KB bewerkt, ziet u een nieuwe bron voor chit-chat, op basis van de persoonlijkheid die u hebt geselecteerd. U kunt nu gewijzigd vragen toevoegen of bewerken van de reacties, net als met een andere bron. 

![Chit chat vragen en antwoorden supereenvoudig bewerken](../media/qnamaker-how-to-chit-chat/edit-chit-chat.png)

## <a name="add-additional-chit-chat-questions-and-answers"></a>Toevoegen van extra chit-chat vragen en antwoorden
U kunt nieuwe chit chat QnA die niet in de vooraf gedefinieerde toevoegen. Zorg ervoor dat u niet een combinatie van QnA die al wordt beschreven in de set chit chat dupliceert. Wanneer u een nieuwe chit chat QnA toevoegt, wordt deze toegevoegd aan uw **redactionele** bron. Om ervoor te zorgen de kerntechnologie zich van bewust dat dit chit chat, toevoegen de metagegevens van sleutel/waarde-paar "redactionele: chit chat ', zoals te zien is in de volgende afbeelding:
   
![Chit chat vragen en antwoorden supereenvoudig toevoegen](../media/qnamaker-how-to-chit-chat/add-new-chit-chat.png)

## <a name="delete-chit-chat-from-an-existing-kb"></a>Chit chat verwijderen uit een bestaande KB
Selecteer uw KB, en Ga naar de **instellingen** pagina. Uw specifieke chit-chat-bron wordt vermeld als een bestand met de naam van de geselecteerde persoonlijkheid. U kunt dit als een bestand verwijderen.

![Chit chat uit KB verwijderen](../media/qnamaker-how-to-chit-chat/delete-chit-chat.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennisdatabase importeren](../Tutorials/migrate-knowledge-base.md)

## <a name="see-also"></a>Zie ook 

[Overzicht van QnA Maker](../Overview/overview.md)
