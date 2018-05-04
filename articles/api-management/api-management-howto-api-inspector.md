---
title: Fouten opsporen in uw API's met behulp van de tracering van aanvragen in Azure API Management | Microsoft Docs
description: Volg de stappen van deze zelfstudie voor informatie over het controleren van de verwerkingsstappen van aanvragen in Azure API Management.
services: api-management
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: 4db89b52a3fa9585572322d546d87513da41c3f0
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="debug-your-apis-using-request-tracing"></a>Fouten opsporen in uw API's met behulp van de tracering van aanvragen

In deze zelfstudie wordt beschreven hoe u aanvraagverwerking inspecteert om u te helpen bij het opsporen van fouten en oplossen van problemen van uw API. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een aanroep traceren

![API-inspector](media/api-management-howto-api-inspector/api-inspector001.PNG)

## <a name="prerequisites"></a>Vereisten

+ Lees de volgende snelstartgids: [Een Azure API Management-exemplaar maken](get-started-create-service-instance.md).
+ Voltooi tevens de volgende zelfstudie: [Uw eerste API importeren en publiceren](import-and-publish.md).

## <a name="trace-a-call"></a>Een aanroep traceren

1. Selecteer **API's**.
2. Klik in de API-lijst op **Demo Conference API**.
3. Selecteer de bewerking **GetSpeakers**.
4. Ga naar het tabblad **Test**.
5. Zorg ervoor dat u een HTTP-header opneemt met de naam **Ocp-Apim-Trace** met de waarde ingesteld op **true**.
6. Klik op **Verzenden** om een API-aanroep te maken. 
7. Wacht totdat de aanroep voltooid is. 
8. Ga naar het tabblad **Traceren** in de **API-console**. U kunt klikken op een van de volgende koppelingen naar gedetailleerde traceringsinformatie: **inkomende**, **back-end**, **uitgaande**.

    In de **inkomende** sectie ziet u de oorspronkelijke aanvraag die API Management heeft ontvangen van de aanroeper en alle beleidsregels die worden toegepast op de aanvraag met inbegrip van de beleidsregels voor frequentielimiet en set-header die we hebben toegevoegd in stap 2.

    In de **back-end**-sectie ziet u de verzoeken die API Management heeft verzonden naar de API-back-end en het antwoord daarop.
    
    In de **uitgaande** sectie ziet u alle beleidsregels die worden toegepast op het antwoord voor het terugzenden naar de aanroeper.
 
    > [!TIP]
    > Bij elke stap wordt ook de verstreken tijd weergegeven vanaf dat de aanvraag is ontvangen door de API Management.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een aanroep traceren

Ga door naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Revisies gebruiken](api-management-get-started-revise-api.md)
