---
title: Fouten opsporen in uw API's met behulp van de tracering van aanvragen in Azure API Management | Microsoft Docs
description: Volg de stappen van deze zelfstudie voor informatie over het controleren van de verwerking van de stappen in Azure API Management-aanvragen.
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 7b9bec7927169b9d820c095a7d11705264e7dcfe
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="debug-your-apis-using-request-tracing"></a>Fouten opsporen in uw API's met behulp van de tracering van aanvragen

Deze zelfstudie wordt beschreven hoe inspecteren aanvraagverwerking om u te helpen bij het opsporen en oplossen van uw API. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een aanroep van traceren

![API-inspector](media/api-management-howto-api-inspector/api-inspector001.PNG)

## <a name="prerequisites"></a>Vereisten

+ Voltooi de volgende Snelstartgids: [Azure API Management-exemplaar maken](get-started-create-service-instance.md).
+ Ook de volgende zelfstudie te voltooien: [importeren en publiceren van uw eerste API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="trace-a-call"></a>Een aanroep van traceren

1. Selecteer **API's**.
2. Klik op **Demo conferentie API** uit de lijst met API.
3. Selecteer **GetSpeakers** bewerking.
4. Overschakelen naar de **Test** tabblad.
5. Zorg ervoor dat u een HTTP-header met de naam **Ocp-Apim-Trace** met de waarde die is ingesteld op **true**.
6. Klik op **'Verzenden'** om te maken van een API-aanroep. 
7. Wacht totdat de aanroep te voltooien. 
8. Ga naar de **Trace** tabblad de **API console**. U kunt klikken op een van de volgende koppelingen naar gedetailleerde tracering info: **inkomende**, **back-end**, **uitgaande**.

    In de **inkomende** sectie ziet u de oorspronkelijke aanvraag API Management ontvangen van de oproepende functie en alle beleidsregels die worden toegepast op de aanvraag met inbegrip van de set-header beleidsregels voor frequentielimiet en we toegevoegd in stap 2.

    In de **back-end** sectie ziet u de verzoeken API Management wordt verzonden naar de API-back-end en het antwoord.
    
    In de **uitgaande** sectie ziet u alle beleidsregels die worden toegepast op het antwoord voor het verzenden van terug naar de aanroeper.
 
    > [!TIP]
    > Elke stap wordt ook de verstreken tijd weergegeven omdat de aanvraag wordt ontvangen door de API Management.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een aanroep van traceren

Ga naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Revisies gebruiken](api-management-get-started-revise-api.md)
