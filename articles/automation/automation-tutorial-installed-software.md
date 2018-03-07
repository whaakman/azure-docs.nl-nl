---
title: "Azure Automation gebruiken om uit te vinden welke software is geïnstalleerd op uw computers | Microsoft Docs"
description: "Inventaris gebruiken om uit te vinden welke software op de computers in uw omgeving is geïnstalleerd."
services: automation
keywords: inventaris, automation, wijziging, bijhouden
author: jennyhunter-msft
ms.author: jehunte
ms.date: 02/28/2018
ms.topic: tutorial
ms.service: automation
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 97cd2c91ca2c70b044518c43d49356918202d5ff
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="discover-what-software-is-installed-on-your-azure-and-non-azure-machines"></a>Uitvinden welke software is geïnstalleerd op uw Azure- en niet-Azure-computers

In deze zelfstudie leert u hoe u kunt uitvinden welke software er is geïnstalleerd in uw omgeving. U kunt inventarisgegevens verzamelen en weergeven voor software, bestanden, Linux-daemons, Windows-services en Windows-registersleutels op uw computers. Door de configuraties van uw computer bij te houden, is het voor u gemakkelijker vast te stellen waar in uw omgeving zich operationele problemen voordoen, en kunt u beter beoordelen wat de status van uw computers is.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een virtuele machine onboarden voor Wijzigingen bijhouden en Inventaris
> * Geïnstalleerde software weergeven
> * Inventarislogboeken doorzoeken op geïnstalleerde software

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u het volgende nodig:

* Een Azure-abonnement. Als u nog geen abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of u aanmelden voor een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Een [Automation-account](automation-offering-get-started.md) voor opslag van de Watcher- en actie-runbooks en de Watcher-taak.
* Een [virtuele machine](../virtual-machines/windows/quick-create-portal.md) voor de onboarding.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="enable-change-tracking-and-inventory"></a>Wijzigingen bijhouden en Inventaris inschakelen

U moet voor deze zelfstudie eerst Wijzigingen bijhouden en Inventaris voor uw virtuele machine inschakelen. Als u eerder de oplossing **Wijzigingen bijhouden** voor uw virtuele machine hebt ingeschakeld, is deze stap niet nodig.

1. Selecteer in het menu links **Virtuele machines** en selecteer een virtuele machine in de lijst.
2. Klik in het menu links in het gedeelte **Bewerkingen** op **Inventaris**. De pagina **Wijzigingen bijhouden en Inventaris inschakelen** wordt weergegeven.

![Banner voor configuratie van onboarden Inventaris](./media/automation-tutorial-installed-software/enableinventory.png)

Configureer de locatie, de Log Analytics-werkruimte en het Automation-account dat moet worden gebruikt, en klik op **Inschakelen** om de oplossing in te schakelen. Als de velden lichtgrijs zijn, betekent dit dat een andere automatiseringsoplossing is ingeschakeld voor de virtuele machine en dat dezelfde werkruimte en hetzelfde Automation-account moeten worden gebruikt.

Een [Log Analytics](../log-analytics/log-analytics-overview.md?toc=%2fazure%2fautomation%2ftoc.json)-werkruimte wordt gebruikt om gegevens te verzamelen die worden gegenereerd door functies en services zoals Inventaris.
De werkruimte biedt één locatie om gegevens uit meerdere bronnen te bekijken en te analyseren.

Het inschakelen van de oplossing kan maximaal 15 minuten duren. Gedurende deze tijd mag u het browservenster niet sluiten.
Nadat de oplossing is ingeschakeld, wordt informatie over geïnstalleerde software en wijzigingen op de virtuele machine naar Log Analytics verzonden.
Het duurt tussen 30 minuten en 6 uur voordat de gegevens beschikbaar zijn voor analyse.

## <a name="view-installed-software"></a>Geïnstalleerde software weergeven

Zodra de oplossing Wijzigingen bijhouden en Inventaris is ingeschakeld, kunt u de resultaten op de pagina **Inventaris** bekijken.

Selecteer vanuit de VM de optie **Inventaris** onder **Bewerkingen**.

Op de pagina **Inventaris** klikt u op het tabblad **Software**.

Op het tabblad **Software** wordt een lijst weergegeven met de software die is gevonden. De software is gegroepeerd op de naam en versie van de software.

De algemene details voor elke softwarerecord worden in de tabel weergegeven. Deze gegevens omvatten de naam van de software, de versie, uitgever, tijd laatst vernieuwd (het meest recente tijdstip waarop is vernieuwd, gemeld door een computer in de groep) en de computers (het aantal computers met die software).

![Software-inventaris](./media/automation-tutorial-installed-software/inventory-software.png)

Klik op een rij om de eigenschappen van de softwarerecord te bekijken evenals de namen van de computers met die software.

Als u wilt zoeken naar specifieke software of de softwaregroep, kunt u dat doen via het tekstvak direct boven de lijst met software.
Het filter kunt u gebruiken om te zoeken op basis van de naam van de software, de versie of de uitgever.

Als u bijvoorbeeld zoekt naar 'Contoso', wordt alle software met 'Contoso' in de naam, uitgever of versie geretourneerd.

## <a name="search-inventory-logs-for-installed-software"></a>Inventarislogboeken doorzoeken op geïnstalleerde software

Inventaris genereert logboekgegevens die naar Log Analytics worden verzonden. Als u de logboeken wilt doorzoeken door query's uit te voeren, selecteert u **Log Analytics** boven in het venster **Inventaris**.

Inventarisgegevens worden opgeslagen onder het type **ConfigurationData**.
De volgende voorbeeldquery van Log Analytics retourneert alle uitgevers die 'Microsoft' bevatten, evenals het aantal softwarerecords (gegroepeerd op softwarenaam en op computer) voor elke uitgever.

```
ConfigurationData
| summarize arg_max(TimeGenerated, *) by SoftwareName, Computer
| where ConfigDataType == "Software"
| search Publisher:"Microsoft"
| summarize count() by Publisher
```

Zie [Azure Log Analytics](https://docs.loganalytics.io/index) voor meer informatie over het uitvoeren en doorzoeken van logboekbestanden in Log Analytics.

### <a name="single-machine-inventory"></a>Inventaris van één computer

Als u een overzicht van de software voor één computer wilt zien, kunt u Inventaris openen vanuit de pagina met VM-resources in Azure, of kunt u met behulp van de filterfunctie in Log Analytics naar de overeenkomstige computer gaan. De volgende voorbeeldquery van Log Analytics retourneert de lijst met software voor een computer met de naam ContosoVM.

```
ConfigurationData
| where ConfigDataType == "Software" 
| summarize arg_max(TimeGenerated, *) by SoftwareName, CurrentVersion
| where Computer =="ContosoVM"
| render table
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u software-inventaris kunt weergeven om het volgende te kunnen doen:

> [!div class="checklist"]
> * Een VM onboarden voor Wijzigingen bijhouden en Inventaris
> * Geïnstalleerde software weergeven
> * Inventarislogboeken doorzoeken op geïnstalleerde software

Ga verder naar het overzicht van de oplossing Wijzigingen bijhouden en Inventaris als u er meer over wilt weten.

> [!div class="nextstepaction"]
> [De oplossing Wijzigingen bijhouden en Inventaris](automation-change-tracking.md)