---
title: Aan de slag met Azure Advisor | Microsoft Docs
description: Aan de slag met Azure Advisor.
services: advisor
documentationcenter: NA
author: manbeenkohli
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/10/2017
ms.author: makohli
ms.openlocfilehash: cd64515beabec43a5209d62dccf2b55b21702c16
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="get-started-with-azure-advisor"></a>Aan de slag met Azure Advisor

Informatie over het openen van Advisor via de Azure portal, aanbevelingen krijgen, en implementatie van de aanbevelingen.

## <a name="get-advisor-recommendations"></a>Ontvang aanbevelingen van Advisor

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Klik in het linkerdeelvenster op **Advisor**.  Als u Advisor niet in het linkerdeelvenster ziet, klikt u op **alle services**.  Klik in het deelvenster service menu onder **bewaking en beheer**, klikt u op **Advisor**.
 De Advisor-dashboard wordt weergegeven.

   ![Toegang tot Azure Advisor met de Azure portal](./media/advisor-get-started/advisor-portal-menu.png) 

4. Het dashboard Advisor wordt een samenvatting weergeven van de aanbevelingen voor alle geselecteerde abonnementen.  U kunt de abonnementen die u wilt dat de aanbevelingen voor het gebruik van het abonnement moet worden weergegeven filteren vervolgkeuzelijst.

5. Als u de aanbevelingen voor een specifieke categorie, klikt u op een van de tabbladen: **hoge beschikbaarheid**, **beveiliging**, **prestaties**, of **kosten**. 

  ![Azure Advisor-dashboard](./media/advisor-overview/advisor-dashboard.png)

## <a name="get-advisor-recommendation-details-and-implement-a-solution"></a>Details van de Advisor-aanbeveling ophalen en implementeren van een oplossing

U kunt een aanbeveling selecteren in Advisor om aanvullende informatie, zoals de aanbevolen acties en betrokken resources – weer te geven en om de oplossing naar de aanbeveling te implementeren.  

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

2. Selecteer een categorie aanbeveling in de lijst met aanbevelingen in die categorie weergeven of Selecteer de **alle** tabblad om alle uw aanbevelingen.

3. Klik op een aanbeveling die u wilt controleren in detail.

4. Lees de informatie over de aanbeveling en de resources die de aanbeveling is van toepassing op.

5. Klik op de **aanbevolen actie** voor het implementeren van de aanbeveling.

## <a name="filter-advisor-recommendations"></a>Filter aanbevelingen Advisor te ontvangen

Aanbevelingen voor inzoomen op wat voor u het belangrijkst is, kunt u filteren.  U kunt filteren op abonnement, resourcetype of aanbeveling status.  

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

2.  De vervolgkeuzelijsten op de Advisor-dashboard gebruiken om te filteren op abonnement, resourcetype of aanbeveling status.

    ![Advisor zoekfilter criteria](./media/advisor-get-started/advisor-filters.png)

## <a name="postpone-or-dismiss-advisor-recommendations"></a>Uitstellen of negeren van Advisor aanbevelingen

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

2. Navigeer naar de aanbeveling die u wilt stellen of te verwijderen.

3. Klik op de aanbeveling.

4. Klik op **uitstellen**. 

5. Geef een uitstellen periode of selecteer **nooit** naar de aanbeveling negeren.

## <a name="exclude-subscriptions-or-resource-groups-from-advisor"></a>Abonnementen of resourcegroepen uitsluiten van Advisor

Mogelijk hebt u resourcegroepen of abonnementen voor die u niet wilt ontvangen van de aanbevelingen van Advisor – zoals 'test'.  U kunt Advisor alleen om aanbevelingen te genereren voor specifieke abonnementen en resourcegroepen configureren.

> [!NOTE]
> Als u wilt opnemen of uitsluiten van een abonnement of resourcegroep van Advisor, moet u de eigenaar van een abonnement.  Als u niet de vereiste machtigingen voor een abonnement of resourcegroep hebt, wordt de optie voor het opnemen of uitsluiten van deze is uitgeschakeld in de gebruikersinterface.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

2. Klik op **configureren** in de actiebalk.

3. Schakel alle abonnementen of de resourcegroepen die u niet wilt ontvangen van aanbevelingen voor Advisor.

    ![Advisor resources voorbeeld configureren](./media/advisor-get-started/advisor-configure-resources.png)

4. Klik op de **toepassen** knop.

## <a name="configure-the-average-cpu-utilization-rule-for-the-low-usage-virtual-machine-recommendation"></a>De gemiddelde CPU-gebruik regel voor de virtuele machine geringe gebruiksduur aanbeveling configureren

Advisor bewaakt het gebruik van uw virtuele machine 14 dagen en identificeert dan laag gebruik virtuele machines. Virtuele machines waarvan gemiddelde CPU-gebruik 5 is % of minder en netwerkgebruik is 7 MB of minder voor vier of meer dagen worden beschouwd als laag gebruik virtuele machines.

Als u worden agressievere wilt op geringe gebruiksduur virtuele machines te identificeren, kunt u het gemiddelde CPU-gebruik regel op basis van per abonnement kunt aanpassen.  De gemiddelde CPU-gebruik regel kan worden ingesteld op % 5, 10%, 15% of 20%.

> [!NOTE]
> Om aan te passen de gemiddelde CPU-gebruik regel voor het identificeren van geringe gebruiksduur virtuele machines, moet u een abonnement *eigenaar*.  Als u niet de vereiste machtigingen voor een abonnement of resourcegroep hebt, wordt de optie voor het opnemen of uitsluiten van deze uitgeschakeld in de gebruikersinterface. 

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

2. Klik op **configureren** in de actiebalk.

3. Klik op de **regels** tabblad.

4. Selecteer de abonnementen die u wilt de gemiddelde CPU-gebruik regel voor het aanpassen en klik vervolgens op **bewerken**.

5. Selecteer de gewenste waarde voor gemiddelde CPU-gebruik en op **toepassen**.

6. Klik op **vernieuwen aanbevelingen** bijwerken van uw bestaande aanbevelingen voor het gebruik van de nieuwe regel voor gemiddelde CPU-gebruik. 

   ![Advisor aanbeveling regels voorbeeld configureren](./media/advisor-get-started/advisor-configure-rules.png)

## <a name="download-your-advisor-recommendations"></a>De aanbevelingen van Advisor downloaden

Advisor kunt u een overzicht van uw aanbevelingen downloaden.  U kunt uw aanbevelingen als een PDF-bestand of een CSV-bestand downloaden.  Uw aanbevelingen downloaden, kunt u eenvoudig delen met uw collega's of uw eigen analyse boven op de aanbeveling gegevens uit te voeren.

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

2. Klik op **downloaden als CSV** of **downloaden als PDF** op de actiebalk.

De downloadoptie respecteert alle filters die u hebt toegepast op de Advisor-dashboard.  Als u de downloadoptie tijdens het bekijken van een specifieke aanbeveling categorie of de aanbeveling selecteert, bevat de gedownloade samenvatting alleen informatie voor die categorie of de aanbeveling. 

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over Advisor:
* [Inleiding tot Azure Advisor](advisor-overview.md)
* [Hoge beschikbaarheid aanbevelingen Advisor te ontvangen](advisor-high-availability-recommendations.md)
* [Aanbevelingen voor beveiliging van Advisor](advisor-security-recommendations.md)
-  [Aanbevelingen van Advisor-prestaties](advisor-performance-recommendations.md)
* [Advisor kosten aanbevelingen](advisor-performance-recommendations.md)
