---
title: Azure Security Center zoeken | Microsoft Docs
description: Meer informatie over het gebruik van Azure Security Center Azure Monitor logboeken zoeken om uw beveiligings gegevens op te halen en te analyseren.
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
ms.openlocfilehash: c88e2025444aa2fd83e05fdff97ea640ceefaa04
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68662912"
---
# <a name="azure-security-center-search-retired"></a>Azure Security Center zoeken (buiten gebruik gesteld)

> [!NOTE]
> Het zoek Dashboard van Security Center is op 31 juli 2019 buiten gebruik gesteld. Zie voor meer informatie en alternatieve Services de [buiten gebruiks telling van Security Center-functies (2019 juli)](security-center-features-retirement-july2019.md#menu_search).

Azure Security Center gebruikt [Azure monitor logboeken zoeken](../log-analytics/log-analytics-log-searches.md) om uw beveiligings gegevens op te halen en te analyseren. Azure Monitor-logboeken bevatten een query taal om snel gegevens op te halen en samen te voegen. Vanuit Security Center kunt u Azure Monitor logboeken zoeken gebruiken om query's te maken en verzamelde gegevens te analyseren.

Zoeken is beschikbaar in de lagen gratis en standaard van Security Center.  De gegevens die beschikbaar zijn in de Zoek opdrachten van het logboek, zijn afhankelijk van het niveau dat op uw werk ruimte wordt toegepast.  Zie de [pagina met prijzen](../security-center/security-center-pricing.md) voor Security Center voor meer informatie.


> [!NOTE]
> Security Center slaat geen beveiligings gegevens op voor een werk ruimte in de gratis laag. U kunt verschillende logboeken naar een werk ruimte in de gratis laag verzenden en op die gegevens zoeken, maar de zoek resultaten bevatten geen gegevens van Security Center. Met Security Center worden alleen gegevens opgeslagen in een werk ruimte in de laag standaard.
>
>

## <a name="access-search"></a>Toegang zoeken
1. Selecteer in het hoofd menu van Security Center **zoeken**.

   ![Selecteer logboek zoeken][1]

2. Security Center worden alle werk ruimten onder uw Azure-abonnementen weer gegeven. Selecteer een werkruimte. (Als u slechts één werk ruimte hebt, wordt deze werkruimte kiezer niet weer gegeven.)

   ![Een werkruimte selecteren][2]

3. **Zoeken** in Logboeken wordt geopend. Voer de volgende voorbeeld query in om een query uit te voeren voor meer gegevens onder de geselecteerde werk ruimte:

   SecurityEvent | waarbij gebeurtenis-naam = = 4625 | aantal samenvatten () op TargetAccount

   Hiermee worden alle accounts weer gegeven waarvoor het aanmelden is mislukt (gebeurtenis 4625).

   ![Zoekresultaten][3]

Zie [Kusto-query taal](../log-analytics/log-analytics-search-reference.md) voor meer informatie over het uitvoeren van een query op gegevens onder de geselecteerde werk ruimte.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u toegang kunt krijgen tot zoeken in Security Center. Security Center gebruikt Azure Monitor logboeken zoeken. Zie voor meer informatie over het zoeken naar Azure Monitor logboeken:

- [Wat is Azure Monitor logboeken?](../log-analytics/log-analytics-overview.md) – Overzicht van Azure Monitor-logboeken
- [Zoek opdrachten in Logboeken in azure monitor logboeken](../log-analytics/log-analytics-log-search-new.md) : beschrijft hoe zoek opdrachten in Logboeken worden gebruikt in azure monitor logboeken en biedt concepten die moeten worden begrepen voordat een logboek zoekactie kan worden gemaakt
- [Gegevens zoeken met Zoek opdrachten in Logboeken in azure monitor](../log-analytics/log-analytics-log-searches.md) -logboeken: zelf studie over het gebruik van zoeken in Logboeken
- [Naslag informatie voor Kusto](../log-analytics/log-analytics-search-reference.md) : hierin wordt de query taal in azure monitor Logboeken beschreven

Zie de volgende onderwerpen voor meer informatie over Azure Security Center:

- [Overzicht van Security Center](security-center-intro.md) : beschrijft de belangrijkste mogelijkheden van Security Center

<!--Image references-->
[1]: ./media/security-center-search/search.png
[2]: ./media/security-center-search/workspace-selector.png
[3]: ./media/security-center-search/log-search.png
