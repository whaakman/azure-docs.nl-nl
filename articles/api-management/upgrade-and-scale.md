---
title: Upgraden en schalen van een exemplaar van Azure API Management | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe om te upgraden en schalen van een exemplaar van Azure API Management.
services: api-management
documentationcenter: 
author: vladvino
manager: anneta
editor: 
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/17/2017
ms.author: apimpm
ms.openlocfilehash: 22cc917eb6f296724bf535e48b0dd6ba8927e5d3
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2017
---
# <a name="upgrade-and-scale-an-api-management-instance"></a>Upgraden en schalen van een exemplaar van API Management 

Klanten kunnen een exemplaar van API Management (APIM) schalen door toevoegen en verwijderen van eenheden. Een **eenheid** bestaat uit het toegewezen Azure-resources en heeft een bepaalde dragende capaciteit uitgedrukt als een aantal API-per maand aanroepen. Werkelijke doorvoer en latentie varieert grotendeels afhankelijk van factoren zoals het aantal en het aantal gelijktijdige verbindingen, type en aantal geconfigureerde beleidsregels, aanvraag en antwoord grootten en back-end latentie. 

Capaciteit en prijs per eenheid afhankelijk is van een **laag** in de eenheid bestaat. U kunt kiezen uit drie lagen: **Developer**, **standaard**, **Premium**. Als u vergroot de capaciteit voor een service in een laag wilt, moet u een eenheid toevoegen. Als de laag die momenteel is geselecteerd in uw exemplaar APIM niet toe meer eenheden toe te voegen dat staat, moet u een upgrade uitvoert naar een hogere laag. 

De prijs van elke eenheid is de mogelijkheid voor toevoegen/verwijderen eenheden, of u bepaalde functies (bijvoorbeeld meerdere landen/regio-implementatie) hebt of niet afhankelijk van de laag die u hebt gekozen voor uw APIM-exemplaar. De [prijsinformatie](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) artikel, wordt uitgelegd welke prijs per eenheid en functies die u kunt in elke laag. 

>[!NOTE]
>De [prijsinformatie](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) artikel ziet u geschatte aantal eenheid capaciteit in elke laag. Als u meer nauwkeurige getallen, moet u om te kijken naar een realistische scenario voor uw API's. Zie de volgende sectie 'Het plannen van capaciteit'.

## <a name="prerequisites"></a>Vereisten

Als u de stappen in dit artikel, moet u het volgende hebben:

+ Een actief Azure-abonnement.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Een exemplaar APIM. Zie voor meer informatie [Azure API Management-exemplaar maken](get-started-create-service-instance.md).

## <a name="how-to-plan-for-capacity"></a>Hoe capaciteit plannen?

Als u wilt weten of er voldoende eenheden uw verkeer dan verwerken testen op werkbelastingen die u verwacht. 

Zoals eerder vermeld, het aantal aanvragen per tweede APIM kunt proces hangt af van veel variabelen. Bijvoorbeeld, de verbinding patroon, de grootte van de aanvraag en antwoord-beleidsregels die zijn geconfigureerd op elke API, aantal clients die aanvragen verzendt.

Gebruik **metrische gegevens** (maakt gebruik van Azure Monitor mogelijkheden) om te begrijpen hoeveel capaciteit wordt gebruikt op elk moment.

### <a name="use-the-azure-portal-to-examine-metrics"></a>De Azure portal gebruiken om te onderzoeken metrische gegevens 

1. Navigeer naar uw APIM exemplaar in de [Azure-portal](https://portal.azure.com/).
2. Selecteer **metrische gegevens**.
3. Selecteer **capaciteit** metrische van **beschikbare metrische gegevens**. 

    De metriek capaciteit geeft u een idee van de capaciteit wordt gebruikt in uw tenant. U kunt testen door de gegevens meer en meer laden en zien wat uw load pick is. U kunt instellen dat een metrische waarschuwing te laten u weten wanneer er iets wat wordt niet verwacht. Uw exemplaar APIM toetreedt bijvoorbeeld capaciteit gedurende meer dan 5 minuten. 

    >[!TIP]
    > U kunt de waarschuwing als u wilt laten u weten wanneer uw service onvoldoende capaciteit of maken met deze aanroep doen naar een logische-app wordt automatisch schalen door toevoeging van een eenheid configureren...

## <a name="upgrade-and-scale"></a>Upgrade en schalen 

Zoals eerder vermeld, kunt u kiezen uit drie lagen: **Developer**, **standaard**, **Premium**. De **Developer** laag moet worden gebruikt voor het evalueren van de service; mag niet worden gebruikt voor productie. De **Developer** laag heeft geen SLA en deze laag (eenheden toevoegen of verwijderen) kan niet worden geschaald. 

**Standaard** en **Premium** zijn productie lagen SLA hebben en kunnen worden geschaald. De **standaard** laag kan worden geschaald naar maximaal vier eenheden. U kunt een willekeurig aantal eenheden toevoegen de **Premium** laag. 

De **Premium** laag kunt u slechts één exemplaar van API management verdelen over een willekeurig aantal gewenste Azure-regio's. Wanneer u in eerste instantie een API Management-service maakt, wordt het exemplaar bevat slechts één eenheid en bevindt zich in één Azure-regio. De eerste regio is aangewezen als het **primaire** regio. Als u meer regio's kunnen gemakkelijk worden toegevoegd. Wanneer een regio toevoegt, geeft u het aantal eenheden dat u wilt toewijzen. U kunt bijvoorbeeld één eenheid hebben de **primaire** regio en vijf eenheden in sommige andere regio. U kunt aanpassen voor welke verkeer hebt u in elke regio. Zie voor meer informatie [implementeren van een Azure API Management-service-exemplaar op meerdere Azure-regio's](api-management-howto-deploy-multi-region.md).

U kunt upgraden en downgraden naar en van elke categorie. Houd er rekening mee dat downgraden sommige functies, bijvoorbeeld de VNETs of de implementatie van meerdere landen/regio, verwijderen kunt indien downgraden naar standaard via de Premium-laag.

>[!NOTE]
>De upgrade of schaal kan duren van 15 tot 30 minuten om toe te passen. U krijgt melding wanneer het is voltooid.

### <a name="use-the-azure-portal-to-upgrade-and-scale"></a>De Azure portal gebruiken om te upgraden en schalen

1. Navigeer naar uw APIM exemplaar in de [Azure-portal](https://portal.azure.com/).
2. Selecteer **schaal en prijzen**.
3. Kies de gewenste categorie.
4. Geef het aantal **eenheden** toe te voegen. Gebruik de schuifregelaar of typt u het aantal eenheden.<br/>
    Als u ervoor kiest de **Premium** laag, moet u eerst een regio selecteren.
5. Druk op **opslaan**

## <a name="next-steps"></a>Volgende stappen

[Een Azure API Management-service-exemplaar implementeren op meerdere Azure-regio 's](api-management-howto-deploy-multi-region.md)

