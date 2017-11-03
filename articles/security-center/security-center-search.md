---
title: Zoeken in Azure Security Center | Microsoft Docs
description: Meer informatie over hoe Azure Security Center logboekanalyse zoeken op te halen en analyseren van uw beveiligingsgegevens gebruikt.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 45b9756b-6449-49ec-950b-5ed1e7c56daa
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: terrylan
ms.openlocfilehash: 513c98237a322dabd6b2bf13443e8998ca843b1d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-security-center-search"></a>Azure Security Center zoeken
Maakt gebruik van Azure Security Center [logboekanalyse zoeken](../log-analytics/log-analytics-log-searches.md) ophalen en analyseren van uw beveiligingsgegevens. Log Analytics bevat een querytaal om snel te ophalen en samenvoegen van gegevens. Vanuit Security Center kunt u gebruikmaken van logboekanalyse zoeken om query's maken en analyseren van de verzamelde gegevens.

Zoeken is beschikbaar in de laag gratis en de Standard-laag van het Beveiligingscentrum.  De gegevens beschikbaar zijn in uw zoekopdrachten logboek is afhankelijk van het niveau van laag toegepast op uw werkruimte.  Zie het Beveiligingscentrum [pagina met prijzen](../security-center/security-center-pricing.md) voor meer informatie.


> [!NOTE]
> Security Center beveiligingsgegevens voor een werkruimte onder de laag gratis niet opgeslagen. U kunt verschillende logboeken verzenden naar een werkruimte onder de laag gratis en zoeken van die gegevens maar zoekresultaten bevatten geen gegevens van Security Center. Security Center slaat alleen gegevens met een werkruimte onder de prijscategorie Standard.
>
>

## <a name="access-search"></a>Toegang zoeken
1. Selecteer onder het hoofdmenu Security Center **Search**.

  ![Selecteer logboek zoeken][1]

2. Security Center geeft een lijst van alle werkruimten onder uw Azure-abonnementen. Selecteer een werkruimte. (Als u hebt slechts één werkruimte deze werkruimte selector wordt niet weergegeven.)

  ![Selecteer een werkruimte][2]

3. **Meld u zoekopdracht** wordt geopend. Om te vragen voor meer gegevens onder de geselecteerde werkruimte, voer deze voorbeeldquery:

  SecurityEvent | wanneer gebeurtenis-id == 4625 | count() door TargetAccount samenvatten

  Resultaat toont alle accounts die het aanmelden (gebeurtenis 4625) is mislukt.

  ![Zoekresultaten][3]

Zie [querytaal van logboekanalyse](../log-analytics/log-analytics-search-reference.md) voor meer informatie over de query voor gegevens in de geselecteerde werkruimte.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u toegang tot zoekopdracht in Security Center. Security Center gebruikt logboekanalyse zoeken. Zie voor meer informatie over logboekanalyse zoeken:

- [Wat is Log Analytics?](../log-analytics/log-analytics-overview.md) – Overzicht op Log Analytics
- [Understanding logboek zoekt in logboekanalyse](../log-analytics/log-analytics-log-search-new.md) - beschrijving van hoe logboek zoekopdrachten worden gebruikt in Log Analytics en concepten die worden begrepen voordat het maken van een zoekopdracht logboek
- [Gegevens met behulp van logboek zoekopdrachten in logboekanalyse zoeken](../log-analytics/log-analytics-log-searches.md) – zelfstudie over het gebruik van logboek zoeken
- [Log Analytics zoeken verwijzing](../log-analytics/log-analytics-search-reference.md) – beschrijft de querytaal in Log Analytics

Zie voor meer informatie over Security Center:

- [Security Center overzicht](security-center-intro.md) – Describes Security Center-belangrijkste mogelijkheden

<!--Image references-->
[1]: ./media/security-center-search/search.png
[2]: ./media/security-center-search/workspace-selector.png
[3]: ./media/security-center-search/log-search.png
