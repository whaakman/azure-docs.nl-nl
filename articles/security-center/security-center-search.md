---
title: Zoeken in Azure Security Center | Microsoft Docs
description: Meer informatie over hoe Azure Security Center maakt gebruik van Azure Monitor zoeken in Logboeken om te halen en je beveiligingsgegevens te analyseren.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: rkarlin
ms.openlocfilehash: 7fae267b74677e93527b4e185c5b52d421ef149e
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57244941"
---
# <a name="azure-security-center-search"></a>Zoeken in Azure Security Center
Azure Security Center maakt gebruik van [Azure Monitor-logboeken zoeken](../log-analytics/log-analytics-log-searches.md) ophalen en uw beveiligingsgegevens te analyseren. Logboeken in Azure Monitor omvat een querytaal snel ophalen en samenvoegen van gegevens. Vanuit Security Center, kunt u gebruikmaken van Azure Monitor logboeken zoeken voor het samenstellen van query's en verzamelde gegevens te analyseren.

Search is beschikbaar in de gratis laag en de prijscategorie Standard van Security Center.  De gegevens beschikbaar zijn in uw zoekopdrachten is afhankelijk van het niveau van de laag toegepast op uw werkruimte.  Zie het Security Center [pagina met prijzen](../security-center/security-center-pricing.md) voor meer informatie.


> [!NOTE]
> Security Center worden beveiligingsgegevens voor een werkruimte onder de gratis laag niet opgeslagen. U kunt verschillende logboeken verzenden naar een werkruimte onder de gratis laag en een zoekopdracht op die gegevens, maar bevatten de zoekresultaten niet gegevens uit Security Center. Security Center worden gegevens alleen opgeslagen naar een werkruimte onder de Standard-laag.
>
>

## <a name="access-search"></a>Toegang zoeken
1. Selecteer in het hoofdmenu van Security Center de optie **zoeken**.

  ![Selecteer zoeken in Logboeken][1]

2. Security Center geeft een lijst van alle werkruimten onder uw Azure-abonnementen. Selecteer een werkruimte. (Als u slechts één werkruimte hebt, deze werkruimteselector niet weergegeven.)

  ![Een werkruimte selecteren][2]

3. **Zoeken in logboeken** wordt geopend. Voer om te vragen voor meer gegevens onder de geselecteerde werkruimte, in dit voorbeeld van een query:

  SecurityEvent | wanneer gebeurtenis-id == 4625 | count() by TargetAccount samenvatten

  Resultaat bevat alle accounts die niet zijn geslaagd om aan te melden (gebeurtenis 4625).

  ![Zoekresultaten][3]

Zie [Kusto-querytaal](../log-analytics/log-analytics-search-reference.md) voor meer informatie over query's uitvoeren voor gegevens onder de geselecteerde werkruimte.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u toegang tot zoekopdracht in Security Center. Security Center maakt gebruik van Azure Monitor logboeken zoeken. Zie voor meer informatie over Azure Monitor logboeken zoeken:

- [Wat is Azure Monitor Logboeken?](../log-analytics/log-analytics-overview.md) – Overzicht van Azure Monitor-Logboeken
- [Understanding zoekopdrachten in Logboeken van Azure Monitor](../log-analytics/log-analytics-log-search-new.md) : hierin wordt beschreven hoe zoekopdrachten in Logboeken van Azure Monitor worden gebruikt en concepten die moet worden geïnterpreteerd voor het maken van een zoeken in Logboeken
- [Zoeken naar gegevens met behulp van zoekopdrachten in Logboeken van Azure Monitor](../log-analytics/log-analytics-log-searches.md) – zelfstudie over het gebruik van zoeken in Logboeken
- [Verwijzing naar de Kusto](../log-analytics/log-analytics-search-reference.md) : Beschrijving van de querytaal in Azure Monitor-Logboeken

Zie de volgende onderwerpen voor meer informatie over Azure Security Center:

- [Overzicht van Security Center](security-center-intro.md) – beschrijft Security Center de belangrijkste mogelijkheden

<!--Image references-->
[1]: ./media/security-center-search/search.png
[2]: ./media/security-center-search/workspace-selector.png
[3]: ./media/security-center-search/log-search.png
