---
title: Fouten opsporen in uw API's met behulp van de tracering van aanvragen in Azure API Management | Microsoft Docs
description: Volg de stappen van deze zelfstudie voor informatie over het controleren van de verwerkingsstappen van aanvragen in Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 06/15/2018
ms.author: apimpm
ms.openlocfilehash: ff3dde8ac95b678866ba6f5216ba23357b067765
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203578"
---
# <a name="debug-your-apis-using-request-tracing"></a>Fouten opsporen in uw API's met behulp van de tracering van aanvragen

In deze zelfstudie wordt beschreven hoe u aanvraagverwerking inspecteert om u te helpen bij het opsporen van fouten en oplossen van problemen van uw API. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een aanroep traceren

![API-inspector](media/api-management-howto-api-inspector/api-inspector001.PNG)

## <a name="prerequisites"></a>Vereisten

+ Informatie over de [terminologie van Azure API Management](api-management-terminology.md).
+ Voltooi de volgende snelstartgids: [Een Azure API Management-exemplaar maken](get-started-create-service-instance.md).
+ Voltooi ook de volgende zelfstudie: [Uw eerste API importeren en publiceren](import-and-publish.md).

## <a name="trace-a-call"></a>Een aanroep traceren

![API-tracering](media/api-management-howto-api-inspector/06-DebugYourAPIs-01-TraceCall.png)

1. Selecteer **API's**.
2. Klik in de API-lijst op **Demo Conference API**.
3. Ga naar het tabblad **Test**.
4. Selecteer de bewerking **GetSpeakers**.
5. Zorg ervoor dat u een HTTP-header opneemt met de naam **Ocp-Apim-Trace** met de waarde ingesteld op **true**.

    > [!NOTE]
    > Als de Ocp-Apim-Subscription-Key niet automatisch wordt ingevuld, kunt u deze ophalen door naar het Ontwikkelaarsportal te gaan en de sleutels op de profielpagina weer te geven.

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
