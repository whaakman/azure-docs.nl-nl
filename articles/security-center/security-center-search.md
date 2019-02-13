---
title: Zoeken in Azure Security Center | Microsoft Docs
description: Meer informatie over hoe Azure Security Center maakt gebruik van Log Analytics search ophalen en uw beveiligingsgegevens te analyseren.
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
ms.openlocfilehash: c02a9f61a4a8b88f8b6c4d861f1a6cbe904ad70d
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56110537"
---
# <a name="azure-security-center-search"></a>Zoeken in Azure Security Center
Azure Security Center maakt gebruik van [zoeken in Log Analytics](../log-analytics/log-analytics-log-searches.md) ophalen en uw beveiligingsgegevens te analyseren. Log Analytics bevat een querytaal voor het snel ophalen en samenvoegen van gegevens. Vanuit Security Center, kunt u gebruikmaken van Log Analytics search om query's samenstelt en verzamelde gegevens te analyseren.

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

Zie [querytaal van Log Analytics](../log-analytics/log-analytics-search-reference.md) voor meer informatie over query's uitvoeren voor gegevens onder de geselecteerde werkruimte.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u toegang tot zoekopdracht in Security Center. Security Center maakt gebruik van Log Analytics search. Zie voor meer informatie over Log Analytics search:

- [Wat is Log Analytics?](../log-analytics/log-analytics-overview.md) – Overzicht van Log Analytics
- [Understanding zoekopdrachten in Logboeken in Log Analytics](../log-analytics/log-analytics-log-search-new.md) : hierin wordt beschreven hoe zoekopdrachten in Logboeken in Log Analytics worden gebruikt en concepten die moet worden geïnterpreteerd voor het maken van een zoeken in Logboeken
- [Zoeken naar gegevens met behulp van zoekopdrachten in Logboeken in Log Analytics](../log-analytics/log-analytics-log-searches.md) – zelfstudie over het gebruik van zoeken in Logboeken
- [Log Analytics zoeken verwijzing](../log-analytics/log-analytics-search-reference.md) : Beschrijving van de querytaal in Log Analytics

Zie de volgende onderwerpen voor meer informatie over Azure Security Center:

- [Overzicht van Security Center](security-center-intro.md) – beschrijft Security Center de belangrijkste mogelijkheden

<!--Image references-->
[1]: ./media/security-center-search/search.png
[2]: ./media/security-center-search/workspace-selector.png
[3]: ./media/security-center-search/log-search.png
