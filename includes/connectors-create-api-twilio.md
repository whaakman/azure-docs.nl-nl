---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/03/2016
ms.author: estfan
ms.openlocfilehash: 845b27b8efd66de87ec08e0b5b81bcc332dffdfb
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50094766"
---
### <a name="prerequisites"></a>Vereisten
* Een Twilio-account
* Een geverifieerde Twilio-telefoonnummer dat SMS-berichten kunt ontvangen
* Een geverifieerde Twilio-telefoonnummer dat kan SMS-bericht verzenden

> [!NOTE]
> Als u van een Twilio-proefaccount gebruikmaakt, kunt u alleen SMS te verzenden **geverifieerd** telefoonnummers.  
> 
> 

Voordat u uw Twilio-account in een logische app gebruiken kunt, verleent u de logische app verbinding maken met uw Twilio-account. Gelukkig kunt u dit eenvoudig vanuit doen binnen uw logische app in de Azure Portal. 

Hier volgen de stappen voor het autoriseren van uw logische app verbinding maken met uw Twilio-account:

1. Selecteer een verbinding wilt maken met Twilio, in de Logic app designer, **beheerde API's van Microsoft weergeven** Voer in de vervolgkeuzelijst *Twilio* in het zoekvak in. Selecteer de trigger of actie wilt u zult gebruiken:  
   ![](./media/connectors-create-api-twilio/twilio-0.png)
2. Als u alle verbindingen met Twilio voordat u dit nog niet hebt gemaakt, zult u wordt gevraagd uw Twilio-referenties op te geven. Deze referenties worden gebruikt voor het autoriseren van uw logische app verbinding maken met en toegang tot gegevens van uw Twilio-account:  
   ![](./media/connectors-create-api-twilio/twilio-1.png)  
3. U moet de **Twilio-account-id** en **Twilio-toegangstoken** vanuit het dashboard in Twilio, dus op uw Twilio-account nu aanmelden voor deze twee soorten informatie:  
   ![](./media/connectors-create-api-twilio/twilio-2.png)  
4. Twilio en Logic apps gebruiken verschillende namen voor het identificeren van deze twee soorten informatie. Hier ziet u hoe u ze moet toewijzen aan het dialoogvenster van Logic apps: ![](./media/connectors-create-api-twilio/twilio-3.png)  
5. Selecteer de **verbinding maken** knop:  
   ![](./media/connectors-create-api-twilio/twilio-4.png)
6. U ziet dat de verbinding is gemaakt en u bent nu vrij om door te gaan met de overige stappen in uw logische app:  
   ![](./media/connectors-create-api-twilio/twilio-5.png)

