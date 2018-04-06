---
title: Beheren van de kosten van de gegevens in Azure Log Analytics | Microsoft Docs
description: Informatie over het wijzigen van de prijsstelling en volume en het bewaren van beleid voor uw werkruimte voor logboekanalyse in Azure beheren.
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
ms.topic: article
ms.date: 03/27/2018
ms.author: magoedte
ms.openlocfilehash: 8fb20fc9e6249a2d19d62df1ce331ce873d5fd3d
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="manage-cost-by-controlling-data-volume-and-retention-in-log-analytics"></a>Kosten beheren via Besturingsmechanismen gegevensvolume en te bewaren in Log Analytics
Log Analytics is ontworpen om scale en ondersteuning voor het verzamelen, te indexeren en opslaan van grote hoeveelheden gegevens per dag van elke bron in uw onderneming of geïmplementeerd in Azure.  Hoewel dit kan een primaire stuurprogramma voor uw organisatie, is kostenefficiëntie uiteindelijk het onderliggende stuurprogramma. Het is ook afhankelijk van het plan geselecteerd hiertoe het is belangrijk om te begrijpen dat de kosten van een werkruimte logboek Analytisc NET is niet gebaseerd op het volume van de gegevens die zijn verzameld, en hoe lang u hebt gekozen voor het opslaan van gegevens die zijn gegenereerd op basis van uw verbonden bronnen.  

In dit artikel bekijken we hoe u proactief bewaken Gegevensgroei volume en opslag, en limieten voor het beheren van de bijbehorende kosten definiëren. 

De kosten van de gegevens zijn aanzienlijk afhankelijk van de volgende factoren: 

- Aantal systemen, infrastructuuronderdelen, cloud-bronnen, enz. u van verzamelt 
- Type van de gegevens die zijn gemaakt met de bron, zoals berichtenwachtrijen, Logboeken, gebeurtenissen, gegevens met betrekking tot beveiliging of maatstaven voor prestaties 
- Hoeveelheid gegevens die worden gegenereerd door deze bronnen en ingenomen naar de werkruimte 
- De gegevens van de periode wordt bewaard in de werkruimte  
- Aantal oplossingen voor het beheer ingeschakeld, gegevensbron en frequentie van de verzameling 

> [!NOTE]
> Raadpleeg de documentatie van elke oplossing aangezien deze biedt een schatting maken van hoeveel gegevens worden verzameld.   

Als u op de *vrije* plan, gegevens zijn beperkt tot 7 dagen bewaren. Voor de *zelfstandige* of *betaalde* laag, verzamelde gegevens is beschikbaar voor de afgelopen 31 dagen. De *vrije* plan heeft 500 MB daglimiet opname en als u vindt dat u steeds meer bedragen dan de toegestane volume, kunt u uw werkruimte naar een betaald abonnement voor het verzamelen van gegevens boven deze limiet. 

> [!NOTE]
> Als u wilt selecteren van een langere bewaartermijn voor de betaalde laag gelden. U kunt op elk gewenst moment en voor meer informatie over prijzen wijzigen van het type van uw abonnement, Zie [prijsinformatie](https://azure.microsoft.com/pricing/details/log-analytics/). 

Er zijn twee manieren waarbij de hoeveelheid gegevens kan worden beperkt en helpen de kosten bepalen, zijn dit dagelijkse initiaal en gegevens bewaren.  

## <a name="review-estimated-cost"></a>Geschatte kosten controleren
Log Analytics maakt het gemakkelijker te begrijpen wat de kosten zijn waarschijnlijk worden op basis van recente gebruikspatronen.  U doet dit door de volgende stappen uitvoeren.  

1. Meld u aan bij de [Azure Portal](http://portal.azure.com). 
2. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.<br><br> ![Azure Portal](media/log-analytics-quick-collect-azurevm/azure-portal-01.png)<br><br>  
3. In het deelvenster abonnementen van logboekanalyse uw werkruimte selecteren en klik vervolgens op **gebruik en de geschatte kosten** vanuit het linkerdeelvenster.<br><br> ![Geschatte kosten pagina en gebruik](media/log-analytics-manage-cost-storage/usage-estimated-cost-dashboard-01.png)<br>

Hier kunt u het gegevensvolume van uw bekijken voor de maand. Dit omvat alle gegevens ontvangen en bewaard in de werkruimte voor logboekanalyse.  Klik op **informatie over het gebruik** vanaf de bovenkant van de pagina dashboard weer te geven de gebruik met informatie over trends van volume door bron, computers en aanbieding. Om te bekijken en instellen van een dagelijkse limiet of voor de bewaarperiode wijzigen, klikt u **volume gegevensbeheer**.
 
Log Analytics kosten worden toegevoegd aan uw Azure-factuur. Ziet u details van uw Azure factureren onder de sectie facturering van de Azure portal of in de [Azure Billing Portal](https://account.windowsazure.com/Subscriptions).  

## <a name="daily-cap"></a>Dagelijkse limiet
Wanneer een werkruimte voor logboekanalyse maken van de Azure portal en u kiest voor de *vrije* plan wordt ingesteld op een 500 MB per dag limiet. Er is geen limiet voor de prijscategorie plannen. Een dagelijkse limiet configureren en te beperken tot de dagelijkse opname van uw werkruimte, maar wees voorzichtig als het doel mag geen dagelijkse limiet.  Anders op dat punt, verliest u gegevens voor de rest van de dag en is van invloed op de mogelijkheden tot de voorwaarden van de gezondheid van ondersteuningsservices voor IT-resources.  Het dagelijkse kapje is moet worden gebruikt als een manier voor het beheren van het onverwachte toename van het gegevensvolume van uw beheerde resources en blijven binnen de limiet, of wanneer u wilt beperken gewoon niet-geplande kosten voor uw werkruimte bedoeld.  

Wanneer de dagelijkse limiet is bereikt, stopt het verzamelen van factureerbare gegevenstypen voor de rest van de dag.  Een waarschuwing weergegeven dat aan de bovenkant van de pagina voor de geselecteerde werkruimte voor logboekanalyse en wordt een bewerkingsgebeurtenis verzonden naar de *bewerking* tabel onder **LogManagement** categorie. Verzamelen van gegevens wordt hervat nadat de tijd reset gedefinieerd onder *dagelijkse limiet wordt ingesteld op*. Het is raadzaam om het definiëren van een waarschuwingsregel op basis van deze bewerkingsgebeurtenis, geconfigureerd om te waarschuwen wanneer de daglimiet voor gegevens is bereikt. 

### <a name="identify-what-daily-data-limit-to-define"></a>Identificeren welke daglimiet gegevens definiëren 
Bekijk [Log Analytics gebruik en de geschatte kosten](log-analytics-usage.md) leert de trend opname van gegevens en wat is het dagelijkse volume kapje definiëren. Het moet worden overwogen zorgvuldig, omdat het niet mogelijk uw resources bewaken nadat de limiet is bereikt. 

### <a name="manage-the-maximum-daily-data-volume"></a>Het maximum aantal dagelijkse gegevensvolume beheren 
De volgende stappen beschrijven het configureren van een limiet voor het beheren van de hoeveelheid gegevens die Log Analytics wordt opnemen per dag.  

1. Selecteer in de werkruimte **gebruik en de geschatte kosten** in het linkerdeelvenster.
2. Op de **gebruik en de geschatte kosten** pagina voor de geselecteerde werkruimte, klikt u op **volume gegevensbeheer** vanaf de bovenkant van de pagina. 
5. Dagelijkse kapje wordt **OFF** standaard – klikt u op **ON** inschakelen en vervolgens stelt u de limiet van volume in GB per dag.<br><br> ![Log Analytics configureren limiet van gegevens](media/log-analytics-manage-cost-storage/set-daily-volume-cap-01.png)

### <a name="alert-when-limit-reached"></a>Waarschuwing als de limiet bereikt
Terwijl we visueel aanwezig in de Azure portal wanneer uw gegevens limiet drempelwaarde wordt voldaan, uitlijnen niet per se dit gedrag voor het beheren van operationele problemen die onmiddellijke aandacht.  U ontvangt een melding van waarschuwingen, kunt u een nieuwe waarschuwingsregel in de Azure-Monitor.  Zie voor meer informatie, [maken, weergeven en beheren van waarschuwingen](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).      

Om u aan de slag, moet u hier de aanbevolen instellingen voor de waarschuwing zijn:

* Doel: Selecteer de resource Log Analytics
* De criteria: 
   * De signaalnaam van het: aangepaste logboek zoekactie
   * Zoekquery: bewerking | waar Detail 'OverQuota' heeft
   * Op basis van: aantal resultaten
   * Voorwaarde: Groter zijn dan
   * Drempelwaarde: 0
   * Periode: 5 (minuten)
   * Frequentie: 5 (minuten)
* De naam van de waarschuwingsregel: dagelijkse gegevenslimiet bereikt
* Ernst: Waarschuwing (ernst 1)

Zodra de waarschuwing is gedefinieerd en de limiet is bereikt, wordt een waarschuwing wordt geactiveerd en de reactie die is gedefinieerd in de groep actie uitvoert. Het kan informeren via e-mail en SMS-berichten van uw team of acties die met behulp van webhooks, Automation-runbooks automatiseren of [integreren met een externe ITSM oplossing](log-analytics-itsmc-overview.md#create-itsm-work-items-from-azure-alerts). 

## <a name="change-the-data-retention-period"></a>De bewaartermijn voor gegevens wijzigen 
De volgende stappen beschrijven hoe lang logboek gegevens wordt opgeslagen door in uw werkruimte configureren.
 
1. Selecteer in de werkruimte **gebruik en de geschatte kosten** in het linkerdeelvenster.
2. Op de **gebruik en de geschatte kosten** pagina, klikt u op **volume gegevensbeheer** vanaf de bovenkant van de pagina.
5. Verplaats de schuifregelaar vergroten of verkleinen het aantal dagen en klik vervolgens op in het deelvenster **OK**.  Als u op de *gratis* laag, u zich niet wijzigen van de bewaartermijn voor gegevens en u wilt upgraden naar de laag betaald om te bepalen van deze instelling.<br><br> ![Werkruimte bewaren instelling wijzigen](media/log-analytics-manage-cost-storage/manage-cost-change-retention-01.png)

## <a name="troubleshooting"></a>Problemen oplossen
**Vraag**: hoe kan ik oplossen als logboekanalyse is niet meer gegevens worden verzameld? 
**Antwoord**: als u zijn op de gratis prijscategorie en meer dan 500 MB aan gegevens in een dag hebt verzonden, verzamelen van gegevens voor de rest van de dag wordt gestopt. De dagelijkse limiet bereikt, is een veelvoorkomende reden die logboekanalyse stopt het verzamelen van gegevens of gegevens lijkt te ontbreken.  
Log Analytics maakt een gebeurtenis van het type bewerking wanneer gegevensverzameling wordt gestart en gestopt.  
Voer de volgende query in de zoekopdracht om te controleren als u de dagelijkse limiet bereikt en er gegevens ontbreken: bewerking | waar OperationCategory == 'Status voor het verzamelen van gegevens'   
Wanneer het verzamelen van gegevens stopt, wordt de OperationStatus waarschuwing. Als gegevensverzameling wordt gestart, wordt de OperationStatus is voltooid.  
De volgende tabel beschrijft oorzaken waardoor het verzamelen van gegevens stopt en een voorgestelde actie om het verzamelen van gegevens hervatten:  

|Reden verzameling stopt| Oplossing| 
|-----------------------|---------|
|Dagelijkse limiet van beschikbare gegevens bereikt<sup>1</sup>|Wachten tot de volgende dag voor de verzameling automatisch opnieuw wordt gestart of wijzig in een betaald prijscategorie.|
|U hebt gedefinieerd in volume gegevensbeheer daglimiet bereikt|Wachten tot de volgende dag voor de verzameling automatisch opnieuw wordt gestart of vergroot de gegevens volume daglimiet beschreven in [maximale dagelijkse gegevensvolume beheren](#manage-the-maximum-daily-volume)|
|Azure-abonnement is een onderbroken vanwege:<br> Gratis proefversie is beëindigd<br> Verlopen van Azure op te geven<br> Maandelijks uitgavenlimiet bereikt (bijvoorbeeld op een abonnement met MSDN of Visual Studio)|Converteren naar een betaald abonnement<br> Limiet verwijderen of wacht u totdat de limiet wordt opnieuw ingesteld|

<sup>1</sup> als uw werkruimte op de gratis prijscategorie is, bent u beperkt tot 500 MB aan gegevens per dag verzenden naar de service. Wanneer u de dagelijkse limiet bereikt, stopt het verzamelen van gegevens tot de volgende dag. Gegevens die worden verzonden tijdens het verzamelen van gegevens is gestopt, is niet geïndexeerd en is niet beschikbaar voor het zoeken. Als gegevensverzameling wordt hervat, vindt de verwerking alleen voor nieuwe verzonden gegevens. 

Log Analytics maakt gebruik van UTC-tijd. De tijd opnieuw instellen is tussen werkruimten om te voorkomen dat alle werkruimten beperkt start ophalen van gegevens op hetzelfde moment. Als de werkruimte de dagelijkse limiet bereikt, verwerking wordt hervat nadat de tijd voor opnieuw instellen is gedefinieerd **dagelijkse limiet wordt ingesteld op**.<br><br> ![Log Analytics beperken UTC-tijdzone](media/log-analytics-manage-cost-storage/data-volume-mgmt-limit-utc.png)

**Vraag**: hoe kan ik gewaarschuwd wanneer de gegevens verzamelen stoppen? 
**Antwoord**: Gebruik de stappen in *maken dagelijkse gegevens cap* waarschuwing wilt worden gewaarschuwd als het verzamelen van gegevens stopt en volg de stappen gebruikt u de stappen in bewerkingen voor regels voor waarschuwingen configureren een e-mail, webhook of runbook toevoegen actie voor de waarschuwingsregel. 

## <a name="next-steps"></a>Volgende stappen  

Om te bepalen hoeveel gegevens worden verzameld, welke bronnen verzendt en de verschillende typen gegevens die worden verzonden om te helpen beheren en de kosten, Zie [analyseren van gegevens in logboekanalyse](log-analytics-usage.md).