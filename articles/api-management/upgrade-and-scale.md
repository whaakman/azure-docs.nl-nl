---
title: Bijwerken en schalen van exemplaar van Azure API Management | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u bijwerken en schalen van een Azure API Management-exemplaar.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: anneta
editor: ''
ms.service: api-management
ms.workload: integration
ms.topic: article
ms.date: 08/18/2018
ms.author: apimpm
ms.openlocfilehash: 1b681b4344512f26f1edc4a5264e718deac569ae
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52441686"
---
# <a name="upgrade-and-scale-an-azure-api-management-instance"></a>Bijwerken en schalen van een Azure API Management-exemplaar  

Klanten kunnen een exemplaar van Azure API Management (APIM) door het toevoegen en verwijderen van eenheden schalen. Een **eenheid** bestaat uit het toegewezen Azure-resources en heeft een bepaalde dragende capaciteit uitgedrukt als een aantal API-per maand aanroepen. Dit getal staat niet voor een aanroep limiet, maar in plaats daarvan een maximale doorvoer waarde om toe te staan voor de ruwe capaciteitsplanning. Werkelijke doorvoer en latentie verschilt grotendeels afhankelijk van factoren zoals aantal en de snelheid van gelijktijdige verbindingen, het soort en aantal geconfigureerde beleidsregels, aanvragen en antwoordgroottes en backend-latentie.

Capaciteit en de prijs van elke eenheid is afhankelijk van de **laag** in dat de eenheid bestaat. U kunt kiezen uit vier lagen: **Developer**, **Basic**, **Standard**, **Premium**. Als u nodig hebt om de capaciteit voor een service binnen een laag te vergroten, moet u een eenheid toevoegen. Als de laag die in de APIM-instantie is geselecteerd, niet toe meer eenheden toe te voegen dat staat, moet u een upgrade naar een hogere laag.

De prijs van elke eenheid en de beschikbare functies (bijvoorbeeld meerdere regio's implementatie), is afhankelijk van de laag die u hebt gekozen voor de APIM-instantie. De [prijsinformatie](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) artikel, wordt de prijs per eenheid en functies die u in elke laag uitgelegd. 

>[!NOTE]
>De [prijsinformatie](https://azure.microsoft.com/pricing/details/api-management/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) geschatte aantal eenheid capaciteit wordt uitgelegd in elke laag. Als u wilt meer nauwkeurige nummers ophalen, moet u kijken naar een realistische scenario voor uw API's. Zie de [capaciteit van een Azure API Management-exemplaar](api-management-capacity.md) artikel.

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen de stappen in dit artikel, moet u:

+ Een actief Azure-abonnement hebben.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Een APIM-instantie hebt. Zie voor meer informatie, [maken van een Azure API Management-exemplaar](get-started-create-service-instance.md).

+ Informatie over het concept van [capaciteit van een Azure API Management-exemplaar](api-management-capacity.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="upgrade-and-scale"></a>Upgraden en de schaal aanpassen  

U kunt kiezen uit vier lagen: **Developer**, **Basic**, **Standard** en **Premium**. De **Developer** laag moet worden gebruikt om de service evalueren; mag niet worden gebruikt voor productie. De **Developer** laag heeft geen SLA en u deze laag (eenheden toevoegen of verwijderen) kan niet schalen. 

**Basic**, **Standard** en **Premium** zijn productie-lagen die SLA hebben en kunnen worden geschaald. De **Basic** -laag is de goedkoopste laag met SLA-geschaalde maximaal 2 eenheden, kan het zijn **Standard** laag kan worden geschaald naar maximaal vier eenheden. U kunt een willekeurig aantal eenheden toevoegen de **Premium** laag.

De **Premium** laag kunt u voor het distribueren van één Azure API Management-exemplaar naar een willekeurig aantal gewenste Azure-regio's. Wanneer u in eerste instantie een Azure API Management-service maakt, wordt het exemplaar bevat slechts één eenheid en bevindt zich in één Azure-regio. De eerste regio wordt aangewezen als het **primaire** regio. Extra regio's kunnen eenvoudig worden toegevoegd. Wanneer u een regio toevoegt, geeft u het aantal eenheden dat u wilt toewijzen. U kunt bijvoorbeeld één eenheid hebben de **primaire** regio en vijf eenheden in een andere regio. U kunt het aantal eenheden op het verkeer die u in elke regio hebt aanpassen. Zie voor meer informatie, [over het implementeren van een Azure API Management service-exemplaar naar meerdere Azure-regio's](api-management-howto-deploy-multi-region.md).

U kunt upgraden en downgraden naar en van elke laag. Houd er rekening mee dat het upgraden of downgraden van sommige functies - bijvoorbeeld VNETs of implementatie in meerdere regio's, verwijderen kunt wanneer downgraden naar Standard of Basic van de Premium-laag.

>[!NOTE]
>De upgrade of schaal kan duren vanaf 15 tot 45 minuten om toe te passen. U krijgt bericht wanneer dit is voltooid.

## <a name="use-the-azure-portal-to-upgrade-and-scale"></a>De Azure portal gebruiken voor het bijwerken en schalen

![Schaal APIM in Azure portal](./media/upgrade-and-scale/portal-scale.png)

1. Navigeer naar de APIM-instantie in de [Azure-portal](https://portal.azure.com/).
2. Selecteer **schalen en prijzen** in het menu.
3. Kies de gewenste laag.
4. Geef het aantal **eenheden** toe te voegen. U kunt de schuifregelaar of typ het aantal eenheden.  
    Als u ervoor kiest de **Premium** laag, moet u eerst een regio selecteren.
5. Klik op **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

- [Over het implementeren van een Azure API Management service-exemplaar naar meerdere Azure-regio 's](api-management-howto-deploy-multi-region.md)
- [Het automatisch schalen van een Azure API Management service-exemplaar](api-management-howto-autoscale.md)
