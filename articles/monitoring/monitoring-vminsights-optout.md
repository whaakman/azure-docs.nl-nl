---
title: Het uitschakelen van controleren met Azure Monitor voor virtuele machines (Preview) | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt niet meer controleren van uw virtuele machines met Azure Monitor voor virtuele machines.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/05/2018
ms.author: magoedte
ms.openlocfilehash: c8eb191daaf48793e4a412fdfa7d7199673a8539
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51220047"
---
# <a name="how-to-disable-monitoring-of-your-virtual-machines-with-azure-monitor-for-vms-preview"></a>Het uitschakelen van de bewaking van uw virtuele machines met Azure Monitor voor virtuele machines (Preview)

Als u dat u niet meer wilt ze te controleren met Azure Monitor voor virtuele machines besluit nadat u bewaking van uw virtuele machines hebt ingeschakeld, kunt u de bewaking kunt uitschakelen. In dit artikel laat zien hoe om dit te doen voor een of meer virtuele machines.  

Azure Monitor voor virtuele machines ondersteunt op dit moment niet selectief uitschakelen van de bewaking van uw VM's. Als uw Log Analytics-werkruimte is geconfigureerd voor ondersteuning van deze oplossing en andere oplossingen, evenals andere bewakingsgegevens verzamelen, is het belangrijk dat u begrijpt de impact en de methoden die hieronder worden beschreven voordat u doorgaat.

Azure Monitor voor virtuele machines, is afhankelijk van de volgende onderdelen om de ervaring te leveren:

* Een Log Analytics-werkruimte, waarin gegevens worden opgeslagen die worden verzameld van VM's en andere bronnen.
* Verzamelen van prestatiemeteritems die zijn geconfigureerd in de werkruimte, die updates van de configuratie van de bewaking op alle virtuele machines verbonden met de werkruimte.
* Twee bewakingsoplossingen geconfigureerd in de werkruimte - **InfrastructureInsights** en **ServiceMap**, welke configuratie van de bewaking update op alle virtuele machines die zijn verbonden met de werkruimte.
* Twee virtuele Azure-machine-uitbreidingen, de **MicrosoftMonitoringAgent** en de **DepenendencyAgent**, die verzamelen en verzenden van gegevens naar de werkruimte.

Overweeg het volgende bij de voorbereiding op het controleren van uw virtuele machines met virtuele machines met Azure Monitor uitschakelen:

* Als u met een enkele virtuele machine wordt geëvalueerd en u de vooraf geselecteerde Log Analytics-werkruimte hebt geaccepteerd, kunt u de bewaking in door te verwijderen van de agent voor afhankelijkheden van de virtuele machine en het verbreken van de Log Analytics-agent uit deze werkruimte kunt uitschakelen. Deze methode is geschikt als u van plan bent over het gebruik van de virtuele machine voor andere doeleinden en later besluit het opnieuw verbinding maken met een andere werkruimte.
* Als u van de Log Analytics-werkruimte gebruikmaakt voor de ondersteuning van andere oplossingen en verzamelen van gegevens uit andere bronnen, kunt u Azure Monitor voor virtuele machines oplossingsonderdelen vanuit de werkruimte zonder onderbreking of gevolgen voor uw werkruimte verwijderen.  

>[!NOTE]
> Na het verwijderen van de onderdelen van de oplossing uit uw werkruimte, kunt u doorgaan met de status van uw Azure-VM's, Zie specifiek prestaties en de kaart gegevens wanneer u gaat u naar de weergave in de portal. Gegevens wordt uiteindelijk niet meer weergegeven in de weergave prestaties en de kaart na enige tijd; de statusweergave blijven echter wel om de integriteitsstatus van de voor uw VM's weer te geven. De **Probeer nu** optie is beschikbaar in de geselecteerde Azure-virtuele machine waarmee u het opnieuw inschakelen bewaking in de toekomst.  

## <a name="complete-removal-of-azure-monitor-for-vms"></a>Verwijderen van Azure Monitor voor virtuele machines

De volgende stappen wordt beschreven hoe u voor het verwijderen van Azure Monitor voor virtuele machines als u nog steeds de Log Analytics-werkruimte nodig hebt. U gaat verwijderen de **InfastructureInsights** en **ServiceMap** oplossingen van de werkruimte.  

>[!NOTE]
>Als u het Serviceoverzicht bewakingsoplossing voor het inschakelen van Azure Monitor voor virtuele machines en u nog steeds afhankelijk van deze zijn, zijn niet verwijderd die oplossing zoals beschreven in stap 6 hieronder.  
>

1. Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).
2. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources Log Analytics. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.
3. In de lijst met Log Analytics-werkruimten, selecteer de werkruimte die u hebt gekozen wanneer onboarding van Azure Monitor voor virtuele machines.
4. Selecteer in het linkerdeelvenster **oplossingen**.  
5. Selecteer in de lijst met oplossingen **InfrastructureInsights (Werkruimtenaam)**, en klik vervolgens op de **overzicht** pagina voor de oplossing, klikt u op **verwijderen**.  Wanneer u hierom wordt gevraagd om te bevestigen, klikt u op **Ja**.  
6. Selecteer in de lijst met oplossingen **ServiceMap (Werkruimtenaam)**, en klik vervolgens op de **overzicht** pagina voor de oplossing, klikt u op **verwijderen**.  Wanneer u hierom wordt gevraagd om te bevestigen, klikt u op **Ja**.  

Voordat u onboarding van Azure Monitor voor virtuele machines, zijn als u niet [verzamelen van de prestatiemeteritems ingeschakeld](monitoring-vminsights-onboard.md?toc=/azure/azure-monitor/toc.json#performance-counters-enabled) voor de Windows- of Linux gebaseerde VM's in uw werkruimte, moet u deze regels uitschakelen door de stappen die worden beschreven [hier](../log-analytics/log-analytics-data-sources-performance-counters.md?toc=/azure/azure-monitor/toc.json#configuring-performance-counters) voor Windows en Linux.

## <a name="disable-monitoring-for-an-azure-vm-and-retain-workspace"></a>Schakel bewaking voor een Azure-VM en bewaren van werkruimte  

De volgende stappen wordt beschreven hoe u bewaking voor een virtuele machine die is ingeschakeld voor de evaluatie van Azure Monitor voor virtuele machines uitschakelen, maar de Log Analytics-werkruimte is nog steeds vereist zijn ter ondersteuning van de bewaking van andere bronnen. Als dit een virtuele Azure-machine, gaat u naar de agent voor afhankelijkheden VM-extensie en de Log Analytics-agent VM-extensie voor Windows/Linux rechtstreeks vanuit de virtuele machine verwijderen. 

>[!NOTE]
>Als de virtuele machine wordt beheerd door Azure Automation voor het indelen van processen, configuratie, beheren of beheren van updates, of beheerd door Azure Security Center voor security management en detectie van bedreigingen, de Log Analytics-agent mogen niet worden verwijderd. Anders u wordt voorkomen dat deze services en oplossingen proactief beheren van uw virtuele machine. 

1. Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com). 
2. Selecteer in de Azure portal, **virtuele Machines**. 
3. Selecteer een VM in de lijst. 
4. Selecteer in het linkerdeelvenster **extensies** en klik op de **extensies** weergeeft, schakelt **DependencyAgent**.
5. Klik op de pagina van de extensie-eigenschappen, **verwijderen**.
6. Op de **extensies** weergeeft, schakelt **MicrosoftMonitoringAgent** en klik op de pagina van de extensie-eigenschappen, **verwijderen**.  
