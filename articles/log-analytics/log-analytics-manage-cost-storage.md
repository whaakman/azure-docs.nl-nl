---
title: Kosten voor het bewaren van gegevens in Azure-logboekanalyse beheren | Microsoft Docs
description: Informatie over het wijzigen van de prijscategorie plannings- en bewaarbeleid voor uw werkruimte voor logboekanalyse in de Azure portal.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/27/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 35064c792b72222d59b1d3f0913a92a4a2b34612
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="manage-cost-of-data-retention-with-your-log-analytics-workspace"></a>Kosten voor het bewaren van gegevens met de werkruimte voor logboekanalyse beheren
Wanneer u zich aanmeldt voor logboekanalyse, afhankelijk van het plan dat u selecteert, is er een limiet aan hoe lang de gegevens die worden gegenereerd door uw verbonden bronnen in uw werkruimte is opgeslagen.  In dit artikel worden de overwegingen die van invloed kunnen hebben op de kosten voor het bewaren van deze gegevens voor verschillende periodes en het configureren van deze limiet gemarkeerd.   

Omdat logboekanalyse grote hoeveelheden gegevens van bronnen on-premises gebruiken kan, zijn cloud en hybride omgevingen, de kosten van die gegevens op te slaan voor een bepaalde periode aanzienlijk is afhankelijk van de volgende factoren:

* Aantal systemen, infrastructuuronderdelen, cloud-bronnen, enz. u van verzamelt
* Type van de gegevens die zijn gemaakt met de bron, zoals berichtenwachtrijen, Logboeken, gebeurtenissen, gegevens met betrekking tot beveiliging of maatstaven voor prestaties
* Hoeveelheid gegevens die worden gegenereerd door deze bronnen 
* Aantal beheeroplossing ingeschakeld, gegevensbron en frequentie van de verzameling

> [!NOTE]
> Raadpleeg de documentatie van elke oplossing aangezien deze biedt een schatting maken van hoeveel gegevens worden verzameld.   

Als u op de *vrije* plan, gegevens zijn beperkt tot zeven dagen bewaren.  Voor de *zelfstandige* of *betaalde* laag, verzamelde gegevens vindt u de afgelopen 31 dagen.  

Tijdens het gebruik van de *vrije* plan, als u vindt u consistent meer bedragen dan de toegestane, kunt u uw werkruimte wijzigen naar een betaald abonnement voor het verzamelen van gegevens boven deze limiet. 

> [!NOTE]
> Als u wilt selecteren van een langere bewaartermijn voor de betaalde laag gelden. U kunt op elk gewenst moment en voor meer informatie over prijzen wijzigen van het type van uw abonnement, Zie [prijsinformatie](https://azure.microsoft.com/pricing/details/log-analytics/). 

## <a name="change-the-data-retention-period"></a>De bewaartermijn voor gegevens wijzigen 

1. Meld u aan bij de [Azure Portal](http://portal.azure.com). 
2. Klik op **meer services** gevonden in de linkerbenedenhoek. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Meld Analytics**.
3. Selecteer in het deelvenster abonnementen van logboekanalyse uw werkruimte te wijzigen in de lijst.
4. Klik op de pagina voor werkruimte **bewaren** vanuit het linkerdeelvenster.
5. Verplaats de schuifregelaar vergroten of verkleinen het aantal dagen en klik vervolgens op op het deelvenster met bewaren **opslaan**.  Als u op de *gratis* laag, u zich niet wijzigen van de bewaartermijn voor gegevens en u wilt upgraden naar de laag betaald om te bepalen van deze instelling.<br><br> ![Werkruimte bewaren instelling wijzigen](media/log-analytics-manage-cost/manage-cost-change-retention.png)

## <a name="next-steps"></a>Volgende stappen  

Om te bepalen hoeveel gegevens worden verzameld, welke bronnen verzendt en de verschillende typen gegevens die worden verzonden om te helpen beheren en de kosten, Zie [analyseren van gegevens in Log Analytics](log-analytics-usage.md)