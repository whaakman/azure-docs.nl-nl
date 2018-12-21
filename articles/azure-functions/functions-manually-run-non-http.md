---
title: Handmatig niet door HTTP geactiveerde Azure-functies uitvoeren
description: Een HTTP-aanvraag gebruiken om niet door HTTP geactiveerde Azure-functies uit te voeren
services: functions
keywords: ''
author: craigshoemaker
manager: jeconnoc
ms.service: azure-functions
ms.topic: tutorial
ms.date: 12/12/2018
ms.author: cshoe
ms.openlocfilehash: 00a72c8c7fb42c763a8b0bad1fa3914ac27c496f
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53406927"
---
# <a name="manually-run-a-non-http-triggered-function"></a>Handmatig een niet door HTTP geactiveerde functie uitvoeren

In dit artikel wordt gedemonstreerd hoe u een niet door HTTP geactiveerde functie handmatig kunt uitvoeren via een speciaal opgemaakte HTTP-aanvraag.

In sommige scenario's moet u wellicht een Azure-functie 'op aanvraag' uitvoeren die indirect wordt geactiveerd.  Voorbeelden van indirecte activeringen zijn [geplande functies](./functions-create-scheduled-function.md) of functies die worden uitgevoerd als gevolg van [de actie van een andere resource](./functions-create-storage-blob-triggered-function.md). 

In het volgende voorbeeld wordt [Postman](https://www.getpostman.com/) gebruikt, maar u kunt ook [cURL](https://curl.haxx.se/), [Fiddler](https://www.telerik.com/fiddler) of een soortgelijk hulpprogramma gebruiken HTTP-aanvragen te verzenden.

## <a name="define-the-request-location"></a>Aanvraaglocatie definiëren

Als u een niet door HTTP geactiveerde functie wilt uitvoeren, moet u een manier hebben om een aanvraag naar Azure te verzenden om de functie uit te voeren. De URL om deze aanvraag te doen, heeft een specifieke vorm.

![De aanvraaglocatie definiëren: hostnaam + mappad + functienaam](./media/functions-manually-run-non-http/azure-functions-admin-url-anatomy.png)

- **Hostnaam:** De openbare locatie van de functie-app die bestaat uit de naam van de functie-app plus *azurewebsites.net*van uw aangepast domein.
- **Mappad:** Als u niet door HTTP geactiveerde functies wilt openen via een HTTP-aanvraag, moet u de aanvraag verzenden via de mappen *admin/functions*.
- **Functienaam:** De naam van de functie die u wilt uitvoeren.

Gebruik de aanvraaglocatie in Postman in combinatie met de hoofdsleutel van de functie in de aanvraag voor Azure om de functie uit te voeren.

## <a name="get-the-functions-master-key"></a>De hoofdsleutel van de functie ophalen

Ga naar de functie in de Azure-portal en klik op **Beheren** en zoek de sectie **Hostsleutels**. Klik in de rij *_master* op de knop **Kopiëren** om de hoofdsleutel naar het klembord te kopiëren.

![Hoofdsleutel vanaf scherm Functiebeheer kopiëren](./media/functions-manually-run-non-http/azure-portal-functions-master-key.png)

Nadat u de hoofdsleutel hebt gekopieerd, klikt u op de functienaam om terug te gaan naar het venster met het codebestand. Klik vervolgens op het tabblad **Logboeken**. Hier zijn de berichten van de functie gelogd als u de functie handmatig in Postman uitvoert.

> [!CAUTION]  
> Vanwege verhoogde machtigingen in de functie-app die door de hoofdsleutel zijn verleend, dient u deze sleutel niet te delen met derden of in een toepassing te distribueren.

## <a name="call-the-function"></a>Functie aanroepen

Open Postman en volg deze stappen:

1. Voer de **aanvraaglocatie in het URL-tekstvak in**. 
2. **Klik** op het tabblad **Headers**.
3. Voer **x-functions-key** in als de eerste **sleutel** en plak de hoofdsleutel (vanaf het klembord) in het vak **waarde**.
4. Voer **Content-Type** in als de tweede **sleutel** en voer **application/json** in als de **waarde**.

    ![Instellingen voor de Postman-headers](./media/functions-manually-run-non-http/functions-manually-run-non-http-headers.png)

5. **Klik** op het tabblad **Hoofdtekst**.
6. Voer **{ "input": "test" }** in als de hoofdtekst voor de aanvraag.

    ![Instellingen voor de Postman-hoofdtekst](./media/functions-manually-run-non-http/functions-manually-run-non-http-body.png)

7. Klik op **Verzenden**.

    ![Een aanvraag verzenden met Postman](./media/functions-manually-run-non-http/functions-manually-run-non-http-send.png)

Postman rapporteert vervolgens de status **202 - Geaccepteerd**.

Ga vervolgens terug naar uw functie in de Azure-portal. Ga naar het venster *Logboeken* en u ziet berichten verschijnen die afkomstig zijn van de handmatige aanroep naar de functie.

![Resultaten in het logboek van de functie na handmatige aanroep](./media/functions-manually-run-non-http/azure-portal-function-log.png)

## <a name="next-steps"></a>Volgende stappen

- [Strategieën voor het testen van uw code in Azure Functions](./functions-test-a-function.md)
- [Lokaal opsporen van fouten in Azure-functies voor Event Grid Trigger](./functions-debug-event-grid-trigger-local.md)
