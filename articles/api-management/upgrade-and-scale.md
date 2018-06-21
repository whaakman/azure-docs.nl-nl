---
title: Upgraden en schalen van een exemplaar van Azure API Management | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe om te upgraden en schalen van een exemplaar van Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 06/18/2018
ms.author: apimpm
ms.openlocfilehash: ca32c72b1582b2a09f9f1754ad778cf1b682a1c2
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36293309"
---
# <a name="upgrade-and-scale-an-api-management-instance"></a>Upgraden en schalen van een exemplaar van API Management  

Klanten kunnen een exemplaar van API Management (APIM) schalen door toevoegen en verwijderen van eenheden. Een **eenheid** bestaat uit het toegewezen Azure-resources en heeft een bepaalde dragende capaciteit uitgedrukt als een aantal API-per maand aanroepen. Dit aantal geeft geen een limiet aanroep, maar in plaats daarvan een maximale doorvoer waarde om toe te staan voor de ruwe capaciteitsplanning. Werkelijke doorvoer en latentie variëren grotendeels afhankelijk van factoren zoals het aantal en het aantal gelijktijdige verbindingen, het type en aantal geconfigureerde beleidsregels, aanvraag- en grootten en back-end latentie.

Capaciteit en prijs per eenheid is afhankelijk van de **laag** in de eenheid bestaat. U kunt kiezen uit vier lagen: **Developer**, **Basic**, **standaard**, **Premium**. Als u vergroot de capaciteit voor een service in een laag wilt, moet u een eenheid toevoegen. Als de laag die momenteel is geselecteerd in uw exemplaar APIM niet toe meer eenheden toe te voegen dat staat, moet u een upgrade uitvoert naar een hogere laag.

De prijs van elke eenheid en de beschikbare functies (bijvoorbeeld meerdere landen/regio-implementatie), is afhankelijk van de laag die u hebt gekozen voor uw APIM-exemplaar. De [prijsinformatie](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) artikel, wordt de prijs per eenheid en functies die u in elke laag kunt uitgelegd. 

>[!NOTE]
>De [prijsinformatie](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) artikel ziet u geschatte aantal eenheid capaciteit in elke laag. Als u meer nauwkeurige getallen, moet u om te kijken naar een realistische scenario voor uw API's. Zie de [capaciteit van een exemplaar van Azure API Management](api-management-capacity.md) artikel.

## <a name="prerequisites"></a>Vereisten

Om de stappen in dit artikel, moet u het volgende doen:

+ Een actief Azure-abonnement hebben.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Een exemplaar APIM hebben. Zie voor meer informatie [Azure API Management-exemplaar maken](get-started-create-service-instance.md).

+ Inzicht in het concept van [capaciteit van een exemplaar van Azure API Management] (api management capacity.md).

## <a name="upgrade-and-scale"></a>Upgraden en de schaal aanpassen  

U kunt kiezen uit vier lagen: **Developer**, **Basic**, **standaard** en **Premium**. De **Developer** laag moet worden gebruikt voor het evalueren van de service; mag niet worden gebruikt voor productie. De **Developer** laag heeft geen SLA en deze laag (eenheden toevoegen of verwijderen) kan niet worden geschaald. 

**Basic**, **standaard** en **Premium** zijn productie lagen die SLA hebben en kunnen worden geschaald. De **Basic** laag is de goedkoopste laag waarvoor de SLA en deze kan worden uitgebreid tot 2 eenheden, **standaard** laag kan worden geschaald naar maximaal vier eenheden. U kunt een willekeurig aantal eenheden toevoegen de **Premium** laag.

De **Premium** laag kunt u slechts één exemplaar van API management verdelen over een willekeurig aantal gewenste Azure-regio's. Wanneer u in eerste instantie een API Management-service maakt, wordt het exemplaar bevat slechts één eenheid en bevindt zich in één Azure-regio. De eerste regio is aangewezen als het **primaire** regio. Als u meer regio's kunnen gemakkelijk worden toegevoegd. Wanneer een regio toevoegt, geeft u het aantal eenheden dat u wilt toewijzen. U kunt bijvoorbeeld één eenheid hebben de **primaire** regio en vijf eenheden in sommige andere regio. U kunt het aantal eenheden op het verkeer dat er in elke regio aanpassen. Zie voor meer informatie [implementeren van een Azure API Management-service-exemplaar op meerdere Azure-regio's](api-management-howto-deploy-multi-region.md).

U kunt upgraden en downgraden naar en van elke categorie. Houd er rekening mee dat een upgrade of een downgrade sommige onderdelen - zoals VNETs of implementatie van meerdere landen/regio verwijderen kunt wanneer downgraden naar Standard of Basic uit de laag Premium.

>[!NOTE]
>De upgrade of schaal kan duren van 15 tot 45 minuten om toe te passen. U krijgt melding wanneer het is voltooid.

## <a name="use-the-azure-portal-to-upgrade-and-scale"></a>De Azure portal gebruiken om te upgraden en schalen

![Schaal APIM in Azure-portal](./media/upgrade-and-scale/portal-scale.png)

1. Navigeer naar uw APIM exemplaar in de [Azure-portal](https://portal.azure.com/).
2. Selecteer **schaal en prijzen** in het menu.
3. Kies de gewenste categorie.
4. Geef het aantal **eenheden** toe te voegen. Gebruik de schuifregelaar of typt u het aantal eenheden.  
    Als u ervoor kiest de **Premium** laag, moet u eerst een regio selecteren.
5. Druk op **opslaan**

## <a name="next-steps"></a>Volgende stappen

[Een Azure API Management-service-exemplaar implementeren op meerdere Azure-regio 's](api-management-howto-deploy-multi-region.md)