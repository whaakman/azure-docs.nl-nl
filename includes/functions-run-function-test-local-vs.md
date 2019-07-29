---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 07/05/2019
ms.author: glenga
ms.openlocfilehash: 0493de7374cffcc40f0434d84facf50b6a708c7b
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68592897"
---
1. Druk op **F5**om de functie uit te voeren. Mogelijk moet u een firewall-uitzonde ring inschakelen zodat de hulpprogram ma's HTTP-aanvragen kunnen afhandelen. Autorisatie niveaus worden nooit afgedwongen wanneer lokaal wordt uitgevoerd.

2. Kopieer de URL van uw functie vanuit de uitvoer van de Azure Functions-runtime.

    ![Lokale Azure-runtime](./media/functions-run-function-test-local-vs/functions-debug-local-vs.png)

3. Plak de URL van de HTTP-aanvraag in de adresbalk van uw browser. Voeg de queryreeks `?name=<YOUR_NAME>` toe aan de URL en voer de aanvraag uit. Hieronder ziet u de reactie op de lokale GET-aanvraag die door de functie wordt geretourneerd, weergegeven in de browser: 

    ![De reactie van de lokale host van de functie in de browser](./media/functions-run-function-test-local-vs/functions-run-browser-local-vs.png)

4. Als u wilt stoppen met fouten opsporen, drukt u op **Shift + F5**.