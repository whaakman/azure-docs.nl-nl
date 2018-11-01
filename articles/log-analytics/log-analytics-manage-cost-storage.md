---
title: Kosten van de gegevens in Azure Log Analytics beheren | Microsoft Docs
description: Informatie over het wijzigen van de prijsstelling en volume en retentie beleid voor uw Log Analytics-werkruimte in Azure beheren.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: 2c52baf47e0b1495a5abfa727ab8bc8d997a4283
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50418888"
---
# <a name="manage-cost-by-controlling-data-volume-and-retention-in-log-analytics"></a>Kosten beheren door het gegevensvolume en retentie in Log Analytics beheren

> [!NOTE]
> In dit artikel wordt beschreven hoe u uw kosten in Log Analytics te beheren door in te stellen de bewaartermijn voor gegevens.  Raadpleeg de volgende artikelen voor meer informatie.
> - [Gegevensgebruik analyseren in Log Analytics](log-analytics-manage-cost-storage.md) wordt beschreven hoe u om te analyseren en ontvang een waarschuwing op het gegevensgebruik van uw.
> - [Gebruik en geschatte kosten bewaken](../monitoring-and-diagnostics/monitoring-usage-and-estimated-costs.md) wordt beschreven hoe u gebruik en geschatte kosten voor meerdere Azure-bewakingsfuncties voor verschillende prijsmodellen. Ook wordt beschreven hoe u kunt wijzigen van het prijsmodel.

Log Analytics is ontworpen om te schalen en ondersteuning voor het verzamelen, indexering en opslaan van grote hoeveelheden gegevens per dag van elke bron in uw onderneming of geïmplementeerd in Azure.  Hoewel dit mogelijk een belangrijkste reden voor uw organisatie, is kostenefficiënt uiteindelijk het onderliggende stuurprogramma. Dit is ook afhankelijk van het geselecteerde plan wat dat betreft, het is belangrijk om te begrijpen dat de kosten van een Log Analytics-werkruimte NET is niet gebaseerd op het volume van gegevens die zijn verzameld, en hoe lang u hebt gekozen voor het opslaan van gegevens die zijn gegenereerd op basis van uw verbonden bronnen.  

In dit artikel bekijken we hoe u proactief bewaken groei voor het volume en de opslag van gegevens, en limieten voor het beheren van de bijbehorende kosten definiëren. 

De kosten van de gegevens zijn aanzienlijk, afhankelijk van de volgende factoren: 

- Aantal systemen, onderdelen van de infrastructuur, cloud-bronnen, enzovoort die u van verzamelt 
- Type van de gegevens die zijn gemaakt met de bron, zoals berichtenwachtrijen, Logboeken, gebeurtenissen, gegevens over beveiliging of metrische gegevens voor prestaties 
- Hoeveelheid gegevens die worden gegenereerd door deze bronnen en die worden opgenomen in de werkruimte 
- De gegevens van de periode wordt bewaard in de werkruimte  
- Aantal oplossingen voor ingeschakeld, gegevensbron en verzamelingsfrequentie 

> [!NOTE]
> Raadpleeg de documentatie voor elke oplossing zoals het biedt een schatting van hoeveel gegevens worden verzameld.   

Klanten met een Enterprise overeenkomst ondertekend voor 1 juli 2018 of die al een Log Analytics-werkruimte in een abonnement hebt gemaakt, u nog steeds toegang hebben tot de *gratis* plan. Als uw abonnement is niet gekoppeld aan een bestaande EA-inschrijving, het *gratis* laag is niet beschikbaar wanneer u een werkruimte in een nieuw abonnement na 2 April 2018 maken.  Gegevens zijn beperkt tot 7 dagen retentie voor de *gratis* laag.  Voor de *zelfstandige* of *betaald* laag, gegevens die zijn verzameld is beschikbaar voor de afgelopen 31 dagen. De *gratis* laag dagelijkse opname-limiet van 500 MB heeft, en als u merkt dat u consistent meer bedragen dan de toegestane volume, kunt u uw werkruimte wijzigen in een betaald abonnement voor het verzamelen van gegevens buiten deze limiet. 

> [!NOTE]
> In rekening gebracht als u ervoor kiest om te selecteren voor de laag betaald geldt een langere retentieperiode. U kunt uw abonnementtype wijzigen op elk gewenst moment en voor meer informatie over prijzen, Zie [prijsinformatie](https://azure.microsoft.com/pricing/details/log-analytics/). 

Er zijn twee manieren waarin de hoeveelheid gegevens kan worden beperkt en helpen uw kosten beheren en dagelijkse volumelimiet en Gegevensretentie betreft.  

## <a name="review-estimated-cost"></a>Geschatte kosten controleren
Log Analytics maakt het gemakkelijk te begrijpen wat de kosten zijn waarschijnlijk worden op basis van recente gebruikspatronen.  U doet dit door de volgende stappen uitvoeren.  

1. Meld u aan bij de [Azure Portal](http://portal.azure.com). 
2. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.<br><br> ![Azure Portal](media/log-analytics-manage-cost-storage/azure-portal-01.png)<br><br>  
3. In het deelvenster voor abonnementen van Log Analytics, selecteert u uw werkruimte en klik vervolgens op **gebruik en geraamde kosten** in het linkerdeelvenster.<br><br> ![Pagina gebruik en geschatte kosten](media/log-analytics-manage-cost-storage/usage-estimated-cost-dashboard-01.png)<br>

Hier kunt u uw gegevensvolume bekijken voor de maand. Dit omvat alle gegevens ontvangen en opgeslagen in uw Log Analytics-werkruimte.  Klik op **informatie over het gebruik** vanaf de bovenkant van de pagina om het dashboard met gebruiksgegevens met informatie over trends van volume door de bron, computers en aanbieding weer te geven. Klik op weergeven en een limiet instellen of wijzigen van de bewaarperiode **gegevensvolumebeheer**.
 
Log Analytics kosten worden toegevoegd aan uw Azure-factuur. U kunt details van uw Azure onder de sectie facturering van Azure portal of in een factuur bekijken de [Azure Billing Portal](https://account.windowsazure.com/Subscriptions).  

## <a name="daily-cap"></a>Daglimiet
Wanneer het maken van een Log Analytics-werkruimte in de Azure-portal, waarna u kiest de *gratis* -abonnement, dat is ingesteld op een 500 MB per daglimiet. Er is geen limiet voor de andere abonnementen. Een dagelijkse limiet configureren en de dagelijkse opname van gegevens voor uw werkruimte beperken, maar wees voorzichtig als het doel mag geen aan de dagelijkse limiet bereikt.  Anders verliest u de gegevens voor de rest van de dag die invloed kan zijn op andere Azure-services en oplossingen waarvan functionaliteit mogelijk afhankelijk van de meest recente gegevens beschikbaar worden gesteld in de werkruimte.  Als gevolg hiervan de mogelijkheid om te zien en krijg een waarschuwing wanneer de voorwaarden van de status van resources ondersteuning van IT-services worden beïnvloed.  De dagelijkse limiet is bedoeld om te worden gebruikt als een manier voor het beheren van de onverwachte toename in aantal gegevens van beheerde resources en blijf binnen uw limiet, of als u wilt beperken gewoon ongeplande charges voor uw werkruimte.  

Wanneer de dagelijkse limiet is bereikt, stopt het verzamelen van factureerbare gegevenstypen voor de rest van de dag. De banner van een waarschuwing wordt weergegeven aan de bovenkant van de pagina voor de geselecteerde Log Analytics-werkruimte en een bewerkingsgebeurtenis wordt verzonden naar de *bewerking* tabel onder **LogManagement** categorie. Het verzamelen van gegevens wordt hervat nadat de tijd voor opnieuw instellen die zijn gedefinieerd onder *dagelijkse limiet wordt ingesteld op*. Het is raadzaam om het definiëren van een waarschuwingsregel op basis van deze bewerkingsgebeurtenis, geconfigureerd om te melden wanneer de dagelijkse limiet is bereikt. 

### <a name="identify-what-daily-data-limit-to-define"></a>Identificeren welke dagelijkse limiet voor gegevens definiëren 
Beoordeling [Log Analytics-gebruik en geraamde kosten](log-analytics-usage.md) voor informatie over de trend voor het opnemen van gegevens en wat is de dagelijkse volumelimiet om te definiëren. Deze moet worden overwogen zorgvuldig, omdat het niet mogelijk om te controleren van uw bronnen nadat de limiet is bereikt. 

### <a name="manage-the-maximum-daily-data-volume"></a>Het maximale dagelijkse gegevensvolume beheren 
De volgende stappen wordt beschreven hoe u een limiet voor het beheren van de hoeveelheid gegevens die Log Analytics wordt per dag opnemen configureren.  

1. Selecteer in uw werkruimte **gebruik en geraamde kosten** in het linkerdeelvenster.
2. Op de **gebruik en geraamde kosten** pagina voor de geselecteerde werkruimte, klikt u op **gegevensvolumebeheer** vanaf de bovenkant van de pagina. 
5. Dagelijkse limiet is **OFF** standaard – klikt u op **ON** wilt inschakelen, en stelt de limiet voor het volume van gegevens in GB per dag.<br><br> ![Log Analytics configureren gegevenslimiet](media/log-analytics-manage-cost-storage/set-daily-volume-cap-01.png)

### <a name="alert-when-limit-reached"></a>Ontvang een waarschuwing wanneer de limiet is bereikt
Terwijl we een visuele hint aanwezig in Azure portal wanneer uw limiet drempelwaarde wordt voldaan, wordt dit gedrag niet per se uitlijnen voor het beheren van operationele problemen die onmiddellijke aandacht.  Voor het ontvangen van een waarschuwingsmelding, kunt u een nieuwe waarschuwingsregel maken in Azure Monitor.  Zie voor meer informatie, [maken, weergeven en beheren van waarschuwingen](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).      

Als u aan de slag te gaan, moet u hier de aanbevolen instellingen voor de waarschuwing zijn:

* Doel: Selecteer uw Log Analytics-resource
* De criteria: 
   * Signaalnaam: aangepast zoeken in Logboeken
   * Zoekquery: bewerking | waar Details 'overschrijding ' van het quotum heeft
   * Op basis van: aantal resultaten
   * Voorwaarde: Groter is dan
   * Drempelwaarde: 0
   * Periode: 5 (minuten)
   * Frequentie: 5 (minuten)
* Naam waarschuwingsregel: dagelijkse gegevenslimiet bereikt
* Ernst: Waarschuwing (Sev 1)

Zodra de waarschuwing is gedefinieerd en de limiet is bereikt, wordt een waarschuwing wordt geactiveerd en voert het antwoord dat is gedefinieerd in de actiegroep. Deze kennis van uw team via e-mail en SMS-berichten of acties met behulp van webhooks, Automation-runbooks automatiseren of [integreren met een externe ITSM-oplossing](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts). 

## <a name="change-the-data-retention-period"></a>Wijzigen van de bewaartermijn voor gegevens 
De volgende stappen wordt beschreven hoe u configureren hoe lang logboek gegevens worden bewaard door in uw werkruimte.
 
1. Selecteer in uw werkruimte **gebruik en geraamde kosten** in het linkerdeelvenster.
2. Op de **gebruik en geraamde kosten** pagina, klikt u op **gegevensvolumebeheer** vanaf de bovenkant van de pagina.
5. Verplaats de schuifregelaar om te vergroten of verkleinen het aantal dagen en klik vervolgens op in het deelvenster **OK**.  Als u van gebruikmaakt de *gratis* laag, kunt u zich niet wijzigen van de bewaartermijn voor gegevens en moet u upgraden naar de prijscategorie betaald als u wilt beheren met deze instelling.<br><br> ![Werkruimte behoud instelling wijzigen](media/log-analytics-manage-cost-storage/manage-cost-change-retention-01.png)

## <a name="troubleshooting"></a>Problemen oplossen
**Vraag**: hoe los ik als Log Analytics is niet meer gegevens worden verzameld? 
**Antwoord**: als u op de gratis laag prijzen zijn en meer dan 500 MB aan gegevens op een dag hebt verzonden, stopt met het verzamelen van gegevens voor de rest van de dag. De dagelijkse limiet wordt bereikt, is een veelvoorkomende reden die Log Analytics stopt het verzamelen van gegevens of gegevens lijkt te ontbreken.  
Log Analytics maakt een gebeurtenis van het type bewerking wanneer het verzamelen van gegevens wordt gestart en gestopt.  
Voer de volgende query in het zoekvak om te controleren als u de dagelijkse limiet is bereikt en er gegevens ontbreken: bewerking | waar OperationCategory == 'Status voor het verzamelen van gegevens'   
Wanneer het verzamelen van gegevens stopt, wordt de OperationStatus waarschuwing. Wanneer het verzamelen van gegevens wordt gestart, wordt de OperationStatus is voltooid.  
De volgende tabel beschrijft de redenen die het verzamelen van gegevens gestopt en een voorgestelde actie voor het verzamelen van gegevens hervatten:  

|Reden verzameling stopt| Oplossing| 
|-----------------------|---------|
|Dagelijkse limiet voor gratis gegevens bereikt<sup>1</sup>|Wachten tot de volgende dag voor de verzameling automatisch opnieuw opstarten of wijzigen in een betaalde prijscategorie.|
|U hebt gedefinieerd in het gegevensvolumebeheer de dagelijkse limiet is bereikt|Wacht totdat de volgende dag voor de verzameling automatisch opnieuw wordt gestart of vergroot de dagelijkse gegevenslimiet volume dat wordt beschreven in [het maximale dagelijkse gegevensvolume beheren](#manage-the-maximum-daily-volume)|
|Azure-abonnement is de status onderbroken vanwege:<br> Gratis proefversie is beëindigd<br> Azure geslaagd is verlopen<br> Maandelijkse bestedingslimiet bereikt (bijvoorbeeld op een MSDN of Visual Studio-abonnement)|Converteren naar een betaald abonnement<br> Limiet verwijderen of wacht u totdat de limiet wordt opnieuw ingesteld|

<sup>1</sup> als uw werkruimte in de gratis prijscategorie is, kunt u maximaal 500 MB aan gegevens per dag verzenden naar de service. Wanneer u de dagelijkse limiet bereikt, stopt het verzamelen van gegevens tot de volgende dag. Gegevens die worden verzonden tijdens het verzamelen van gegevens is gestopt, is niet geïndexeerd en is niet beschikbaar voor het zoeken. Wanneer het verzamelen van gegevens wordt hervat, treedt alleen voor nieuwe verzonden gegevens verwerken op. 

Log Analytics maakt gebruik van UTC-tijd. De tijd voor opnieuw instellen varieert tussen werkruimten om te voorkomen dat alle maximale werkruimten start ophalen van gegevens op hetzelfde moment. Als de werkruimte de dagelijkse limiet bereikt, verwerking wordt hervat nadat de tijd voor opnieuw instellen die zijn gedefinieerd in **dagelijkse limiet wordt ingesteld op**.<br><br> ![Log Analytics beperken UTC-tijdzone](media/log-analytics-manage-cost-storage/data-volume-mgmt-limit-utc.png)

**Vraag**: hoe kan ik een melding wanneer het verzamelen van gegevens gestopt? 
**Antwoord**: Gebruik de stappen *maken dagelijkse gegevenslimiet* waarschuwing wilt worden gewaarschuwd als het verzamelen van gegevens gestopt en volg de stappen gebruikt u de stappen bewerkingen voor regels voor waarschuwingen configureren van een e-mail, een webhook of een runbook toevoegen actie voor de waarschuwingsregel. 

## <a name="next-steps"></a>Volgende stappen  

Om te bepalen hoeveel gegevens worden verzameld, welke bronnen worden verzonden en de verschillende typen gegevens worden verzonden naar het gebruik en kosten te beheren, Zie [gegevensgebruik analyseren in Log Analytics](log-analytics-usage.md).
