---
title: Uitvoer fout beleidsregels in Azure Stream Analytics
description: Meer informatie over de uitvoer-beleid beschikbaar in Azure Stream Analytics voor foutafhandeling.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: b31530966d2c5ca9a3f82f3e74ba349e66053a83
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53109786"
---
# <a name="azure-stream-analytics-output-error-policy"></a>Azure Stream Analytics uitvoer foutbeleid
Dit artikel beschrijft de uitvoer gegevens foutafhandeling beleid die kunnen worden geconfigureerd in Azure Stream Analytics.

Uitvoer foutafhandeling beleid alleen voor gegevensconversiefouten die zich voordoen geldt wanneer de uitvoergebeurtenis geproduceerd door een Stream Analytics-taak komt niet overeen met het schema van de doel-sink. U kunt dit beleid configureren door het kiezen van een **opnieuw** of **neerzetten**. In de Azure-portal, terwijl in een Stream Analytics-taak, onder **configureren**, selecteer **Foutbeleid** uw selectie te maken.

![Azure Stream Analytics de uitvoerlocatie voor fout beleid](./media/stream-analytics-output-error-policy/stream-analytics-error-policy-locate.png)


## <a name="retry"></a>Opnieuw proberen
Wanneer een fout optreedt, Azure Stream Analytics nieuwe pogingen voor het schrijven van de gebeurtenis voor onbepaalde tijd totdat de schrijfbewerking is geslaagd. Er is geen time-out voor nieuwe pogingen. Uiteindelijk alle volgende gebeurtenissen hebben geen toegang tot verwerking door de gebeurtenis die probeert het opnieuw. Deze optie is de standaardfout van de uitvoer verwerking van beleid.

## <a name="drop"></a>Verwijderen
Azure Stream Analytics wordt een uitvoergebeurtenis die in een conversiefout van gegevens resulteert verwijderen. De verwijderde gebeurtenissen kunnen niet worden hersteld voor het later opnieuw.


Alle tijdelijke fouten (bijvoorbeeld netwerkfouten) opnieuw kunnen worden uitgevoerd, ongeacht de configuratie van beleid voor foutafhandeling van uitvoer.


## <a name="next-steps"></a>Volgende stappen
[Gids voor probleemoplossing voor Azure Stream Analytics](stream-analytics-troubleshooting-guide.md)
