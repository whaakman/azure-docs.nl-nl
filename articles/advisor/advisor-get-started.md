---
title: Aan de slag met Azure Advisor | Microsoft Docs
description: Aan de slag met Azure Advisor.
services: advisor
author: kasparks
ms.service: advisor
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/01/2019
ms.author: kasparks
ms.openlocfilehash: aabb316cf564520e6ea5e8689f1c18c98b94ab3c
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55488472"
---
# <a name="get-started-with-azure-advisor"></a>Aan de slag met Azure Advisor

Leer hoe u toegang tot Advisor via Azure portal, ontvang aanbevelingen en implementeer aanbevelingen.

> [!NOTE]
> Azure Advisor wordt automatisch op de achtergrond uitgevoerd om te zoeken naar nieuwe resources. Duurt maximaal 24 uur voor aanbevelingen op deze resources.

## <a name="get-recommendations"></a>Ontvang aanbevelingen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Klik in het linkerdeelvenster op **Advisor**.  Als u Advisor niet in het linkerdeelvenster ziet, klikt u op **alle services**.  Klik in het menu service onder **bewaking en beheer**, klikt u op **Advisor**. De Advisor-dashboard wordt weergegeven.

   ![Toegang tot Azure Advisor met behulp van de Azure portal](./media/advisor-get-started/advisor-portal-menu.png) 

1. De Advisor-dashboard wordt een overzicht van uw aanbevelingen voor alle geselecteerde abonnementen weergegeven.  U kunt de abonnementen die u aanbevelingen worden weergegeven voor het gebruik van het abonnement wilt filteren vervolgkeuzelijst.

1. Als u aanbevelingen voor een specifieke categorie, klikt u op een van de tabbladen: **Hoge beschikbaarheid**, **Security**, **prestaties**, of **kosten**. 

  ![Azure Advisor-dashboard](./media/advisor-overview/advisor-dashboard.png)

## <a name="get-recommendation-details-and-implement-a-solution"></a>Details van de aanbeveling ophalen en implementeren van een oplossing

U kunt een aanbeveling in Advisor om aanvullende informatie, zoals de aanbevolen acties voor gebruikers en getroffen resources – weer te geven en om de oplossing aan de aanbeveling te implementeren.  

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

1. Selecteer een aanbeveling categorie aan de lijst met aanbevelingen binnen die categorie worden weergegeven, of Selecteer de **alle** tabblad om al uw aanbevelingen weer te geven.

1. Klik op een aanbeveling die u wilt in detail bekijken.

1. Lees de informatie over de aanbeveling en de resources die de aanbeveling is van toepassing op.

1. Klik op de **aanbevolen actie** de aanbeveling kunt implementeren.

## <a name="filter-recommendations"></a>Aanbevelingen voor filteren

U kunt filteren, aanbevelingen om te zoomen op wat u het belangrijkst is.  U kunt filteren op abonnement, resourcetype of de status van de aanbeveling.  

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

1. Gebruik de vervolgkeuzemenu's op de Advisor-dashboard om te filteren op abonnement, resourcetype of de status van de aanbeveling.

    ![Advisor-zoekfilter criteria](./media/advisor-get-started/advisor-filters.png)

## <a name="postpone-or-dismiss-recommendations"></a>Uitstellen of aanbevelingen negeren

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

1. Navigeer naar de aanbeveling die u wilt stellen of te verwijderen.

1. Klik op de aanbeveling.

1. Klik op **uitstellen**. 

1. Geef een uitstellen periode of selecteer **nooit** voor het verwijderen van de aanbeveling.

## <a name="exclude-subscriptions-or-resource-groups"></a>Abonnementen of resourcegroepen uitsluiten

Mogelijk hebt u resourcegroepen of abonnementen voor die u niet wilt ontvangen van de aanbevelingen van Advisor, zoals 'test'-resources.  U kunt Advisor voor het genereren van alleen aanbevelingen voor specifieke abonnementen en resourcegroepen configureren.

> [!NOTE]
> Als u wilt opnemen of uitsluiten van een abonnement of resourcegroep van de Advisor, moet u de eigenaar van een abonnement zijn.  Als u de vereiste machtigingen voor een abonnement of resourcegroep hebt, wordt de optie wilt opnemen of uitsluiten van deze is uitgeschakeld in de gebruikersinterface.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

1. Klik op **configureren** in de actiebalk.

1. Schakel het selectievakje geen abonnementen of resourcegroepen die u niet wilt ontvangen van de aanbevelingen van Advisor voor.

    ![Advisor resources voorbeeld configureren](./media/advisor-get-started/advisor-configure-resources.png)

1. Klik op de knop **Toepassen**.

## <a name="configure-low-usage-vm-recommendation"></a>Beperkt gebruikte VM-aanbeveling configureren

Deze procedure configureert u de gemiddelde CPU-gebruik regel voor de aanbeveling voor beperkt gebruikte virtuele machines.

Advisor bewaakt uw gebruik van virtuele machines gedurende 14 dagen en vervolgens laag gebruik virtuele machines identificeert. Virtuele machines waarvan gemiddelde CPU-gebruik 5 is % of minder en netwerkgebruik is 7 MB of minder voor vier of meer dagen worden beschouwd als laag gebruik virtuele machines.

Als u wilt worden agressiever ten grondslag ligt beperkt gebruikte virtuele machines, kunt u de gemiddelde CPU-gebruik regel op basis van een abonnement aanpassen.  De gemiddelde CPU-gebruik regel kan worden ingesteld op 5%, % 10, 15% of % 20.

> [!NOTE]
> Om aan te passen de gemiddelde CPU-gebruik regel voor het identificeren van beperkt gebruikte virtuele machines, moet u een abonnement zijn *eigenaar*.  Als u de vereiste machtigingen voor een abonnement of resourcegroep hebt, wordt de optie wilt opnemen of uitsluiten van deze is uitgeschakeld in de gebruikersinterface. 

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

1. Klik op **configureren** in de actiebalk.

1. Klik op de **regels** tabblad.

1. Selecteer de abonnementen die u wilt de gemiddelde CPU-gebruik regel voor het aanpassen en klik vervolgens op **bewerken**.

1. Selecteer de gewenste waarde voor gemiddelde CPU-gebruik, en klik op **toepassen**.

1. Klik op **aanbevelingen vernieuwen** om bij te werken van uw bestaande aanbevelingen voor het gebruik van de nieuwe regel voor gemiddelde CPU-gebruik. 

   ![Advisor aanbeveling regels voorbeeld configureren](./media/advisor-get-started/advisor-configure-rules.png)

## <a name="download-recommendations"></a>Aanbevelingen downloaden

Advisor kunt u een overzicht van uw aanbevelingen downloaden.  U kunt uw aanbevelingen als een PDF-bestand of een CSV-bestand downloaden.  Uw aanbevelingen te downloaden, kunt u eenvoudig kunt delen met uw collega's en uw eigen analyses boven op de aanbeveling gegevens uitvoeren.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

1. Klik op **downloaden als CSV-bestand** of **downloaden als PDF-bestand** in de actiebalk.

De downloadoptie rekening gehouden met eventuele filters die u hebt toegepast op de Advisor-dashboard.  Als u de downloadoptie tijdens het bekijken van een specifieke aanbeveling categorie of een aanbeveling selecteert, zijn de gedownloade samenvatting bevat alleen informatie voor de categorie of aanbeveling. 

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Advisor:

- [Inleiding tot Azure Advisor](advisor-overview.md)
- [Advisor-aanbevelingen voor hoge beschikbaarheid](advisor-high-availability-recommendations.md)
- [Advisor-aanbevelingen voor beveiliging](advisor-security-recommendations.md)
- [Advisor-aanbevelingen voor prestaties](advisor-performance-recommendations.md)
- [Aanbevelingen van Advisor-kosten](advisor-performance-recommendations.md)
