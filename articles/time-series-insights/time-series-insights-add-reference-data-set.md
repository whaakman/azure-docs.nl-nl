---
title: Een gegevensset verwijzing toevoegen aan uw omgeving Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe een gegevensset verwijzing toevoegen aan uw Azure Time Series Insights-omgeving. Referentiegegevens is handig voor het koppelen aan de brongegevens aan de waarden te verbeteren.
services: time-series-insights
ms.service: time-series-insights
author: venkatgct
ms.author: venkatja
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: 7cdcefbd0daec3b7bab59680afa1470624583c74
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="create-a-reference-data-set-for-your-time-series-insights-environment-using-the-azure-portal"></a>Een verwijzing gegevensset voor uw Time Series Insights-omgeving met de Azure portal maken

In dit artikel wordt beschreven hoe een gegevensset verwijzing toevoegen aan uw Azure Time Series Insights-omgeving. Referentiegegevens is handig voor het koppelen aan de brongegevens aan de waarden te verbeteren.

Een referentiegegevensset is een verzameling items die is uitgebreid met de gebeurtenissen uit uw gebeurtenisbron. De engine voor inkomende gebeurtenissen van Time Series Insights koppelt een gebeurtenis van uw gebeurtenisbron aan een item in uw referentiegegevensset. Deze uitgebreide gebeurtenis is vervolgens beschikbaar voor query’s. Deze koppeling is gebaseerd op de sleutels die zijn gedefinieerd in uw referentiegegevensset.

## <a name="add-a-reference-data-set"></a>Een gegevensset verwijzing toevoegen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Zoek uw bestaande Time Series Insights-omgeving. Klik op **alle resources** in het menu aan de linkerkant van de Azure-portal. Selecteer uw Time Series Insights-omgeving.

3. Onder de **omgeving topologie** kop, selecteer **Sets van verwijzingsgegevens**.

    ![De Time Series Insights-referentiegegevensset maken](media/add-reference-data-set/getstarted-create-reference-data-set-1.png)

4. Selecteer **+ toevoegen** toevoegen van een nieuwe referentie-gegevensset.

5. Geef een unieke **verwijzing gegevenssetnaam**.

    ![De Time Series Insights-referentiegegevensset maken - details](media/add-reference-data-set/getstarted-create-reference-data-set-2.png)

6. Geef de **sleutelnaam** in het veld leeg en selecteer de **Type**. Deze naam en type worden gebruikt voor de juiste eigenschap van de gebeurtenissen in uw gegevensbron voor het samenvoegen met de referentiegegevens kiezen. 

   Bijvoorbeeld, als u de naam van de sleutel als opgeven **DeviceId** en typ als **tekenreeks**, en vervolgens de Time Series Insights ingress-engine naar een eigenschap met de naam zoekt **DeviceId** van het type **Tekenreeks** omhoog in elke inkomende gebeurtenis te zoeken en koppelen met. U kunt meer meerdere sleutels opgeven om samen te voegen met de gebeurtenis. De naamsovereenkomst van de sleutel is hoofdlettergevoelig.

7. Selecteer **Maken**.

## <a name="next-steps"></a>Volgende stappen
* Programmatisch [referentiegegevens beheren](time-series-insights-manage-reference-data-csharp.md).
* Zie voor de volledige API-verwijzing het document [Reference Data API](/rest/api/time-series-insights/time-series-insights-reference-reference-data-api).
