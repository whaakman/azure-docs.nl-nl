---
title: Gebruik Emoji emoticons binnen Azure Mobile Engagement
description: Het gebruik van Emoji emoticons binnen uw pushmeldingen
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''
ms.assetid: 663317d7-3c93-4e8f-b13d-c6fb342124ee
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: b5b0e7bfe07054d093dc164cb5f72bde4ba28170
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2018
---
# <a name="use-emoji-emoticon-within-push-notifications"></a>Gebruik Emoji emoticon binnen Pushmeldingen
> [!IMPORTANT]
> Azure Mobile Engagement beëindigen op 3/31/2018. Deze pagina wordt kort na worden verwijderd.
> 

U kunt opnemen Emoji emoticons in u push-melding in een paar eenvoudige stappen: 

1. Ten eerste moet u de Emoji zoeken u in het bericht te verzenden. Zorg ervoor dat de Emoji die u selecteert worden ondersteund door het doelapparaat als apparaat fabrikanten enige tijd duren om nieuwe goedgekeurde Emojis toevoegen aan de apparaatplatforms. 
2. Op **Windows** -u kunt navigeren naar deze [koppeling](http://apps.timwhitlock.info/emoji/tables/unicode) en kopieer het pictogram 'Native'.
   
    ![][7] 
3. Op **Mac** -vindt u de Emojis in woordenboek toepassing onder Edit -> Emoji & symbolen.
   
    ![][6] 
4. Nu gaat u naar de **bereiken** tabblad op de de Azure Mobile Engagement-portal. Selecteer het type van de push-melding (aankondiging, peilt enzovoort). In dit voorbeeld kiest u een aankondiging push.
5. Geef de verschillende velden van de melding totdat u de tekst van de melding. Dit is waar u uw Emoji Emoticon toevoegt. U kunt kiezen in de titel en/of het bericht te plaatsen. U moet slepen en neerzetten of kopiëren van de Emoji die u in de bovenstaande locaties vinden. 
   
    ![][1]
6. Vul de overige velden voor de melding en opslaan. 
7. Als u een test uitvoert of activeren van de aankondiging ziet u een melding met de emoticon zoals opgegeven.   
   
    ![][3] ![][4] ![][5]

<!-- Images. -->
[1]: ./media/mobile-engagement-use-emoji-with-push/notification_input.png
[3]: ./media/mobile-engagement-use-emoji-with-push/iOS_Emoji.png
[4]: ./media/mobile-engagement-use-emoji-with-push/Android_Emoji.png
[5]: ./media/mobile-engagement-use-emoji-with-push/WindowsPhone_Emoji.png
[6]: ./media/mobile-engagement-use-emoji-with-push/Mac_SelectEmoji.png
[7]: ./media/mobile-engagement-use-emoji-with-push/Windows_SelectEmoji.png

