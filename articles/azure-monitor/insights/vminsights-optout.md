---
title: Schakel bewaking in Azure Monitor voor virtuele machines (preview) | Microsoft Docs
description: In dit artikel wordt beschreven hoe u bewaking van uw virtuele machines in Azure Monitor voor virtuele machines stoppen.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/05/2018
ms.author: magoedte
ms.openlocfilehash: eb667486a6e3279cb78fefe02723f14d9f7c9b4f
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67155684"
---
# <a name="disable-monitoring-of-your-vms-in-azure-monitor-for-vms-preview"></a>Schakel bewaking van uw virtuele machines in Azure Monitor voor virtuele machines (preview)

Nadat u de bewaking van uw virtuele machines (VM's) hebt ingeschakeld, kunt u later uitschakelen bewaking in Azure Monitor voor virtuele machines. In dit artikel laat zien hoe om uit te schakelen voor een of meer virtuele machines bewaken.  

Op dit moment biedt geen Azure Monitor voor virtuele machines ondersteuning voor selectief uit te schakelen van de VM-bewaking. Uw Log Analytics-werkruimte kan ondersteuning voor Azure Monitor voor VM's en andere oplossingen. Het kan ook andere bewakingsgegevens verzamelen. Als uw Log Analytics-werkruimte deze services biedt, moet u informatie over de kracht en methoden voor het uitschakelen van de bewaking voordat u begint.

Azure Monitor voor virtuele machines, is afhankelijk van de volgende onderdelen om de ervaring te leveren:

* Een Log Analytics-werkruimte, waarin de bewakingsgegevens van VM's en andere bronnen worden opgeslagen.
* Een verzameling van prestatiemeteritems die zijn geconfigureerd in de werkruimte. De verzameling updates voor de configuratie van de bewaking op alle virtuele machines die zijn verbonden met de werkruimte.
* `InfrastructureInsights` en `ServiceMap`, die zijn oplossingen die zijn geconfigureerd in de werkruimte bewaking. Deze oplossingen werk de configuratie van de bewaking op alle virtuele machines die zijn verbonden met de werkruimte.
* `MicrosoftMonitoringAgent` en `DependencyAgent`, welke Azure-VM-extensies zijn. Deze uitbreidingen verzamelen en verzenden van gegevens naar de werkruimte.

Bij het uitschakelen van de bewaking van uw virtuele machines voorbereiden, houd rekening met deze overwegingen:

* Als u met een enkele virtuele machine geëvalueerd en gebruikt de vooraf geselecteerde standaard Log Analytics-werkruimte, kunt u de bewaking in door te verwijderen van de agent voor afhankelijkheden van de virtuele machine en het verbreken van de Log Analytics-agent uit deze werkruimte kunt uitschakelen. Deze aanpak is geschikt als u van plan bent te gebruiken van de virtuele machine voor andere doeleinden en later besluit het opnieuw verbinding maken met een andere werkruimte.
* Als u een bestaande Log Analytics-werkruimte die ondersteuning biedt voor andere bewakingsoplossingen en verzamelen van gegevens uit andere bronnen hebt geselecteerd, kunt u oplossingsonderdelen vanuit de werkruimte verwijderen zonder dat wordt onderbroken of die betrekking hebben op uw werkruimte.  

>[!NOTE]
> Na het verwijderen van de onderdelen van de oplossing uit uw werkruimte, blijft u mogelijk de status van uw Azure-VM's; zien specifiek, u prestaties zien en gegevens toewijzen wanneer u gaat u naar een van beide weergaven in de portal. Gegevens wordt uiteindelijk niet meer weergegeven in de **prestaties** en **kaart** weergaven. Maar de **Health** weergave blijft om de integriteitsstatus van de voor uw VM's weer te geven. De **Probeer nu** optie is beschikbaar via de geselecteerde Azure-VM, zodat u kunt opnieuw inschakelen voor bewaking in de toekomst.  

## <a name="remove-azure-monitor-for-vms-completely"></a>Azure Monitor voor virtuele machines geheel verwijderen

Als u nog steeds de Log Analytics-werkruimte, volg deze stappen om volledig te verwijderen Azure Monitor voor virtuele machines. Verwijdert u de `InfrastructureInsights` en `ServiceMap` oplossingen van de werkruimte.  

>[!NOTE]
>Als u de oplossing voor de controle voordat u Azure Monitor ingeschakeld voor virtuele machines en u nog steeds afhankelijk van het zijn Serviceoverzicht gebruikt, geen oplossing te verwijderen die zoals beschreven in de laatste stap van de volgende procedure.  
>

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in de Azure-portal de optie **Alle services**. Typ in de lijst met resources **Log Analytics**. Als u te typen begint, filtert de lijst met suggesties op basis van uw invoer. Selecteer **Log Analytics**.
3. Selecteer de werkruimte in de lijst met Log Analytics-werkruimten, u hebt gekozen wanneer u Azure Monitor ingeschakeld voor virtuele machines.
4. Selecteer aan de linkerkant **oplossingen**.  
5. Selecteer in de lijst met oplossingen **InfrastructureInsights (Werkruimtenaam)** . Op de **overzicht** pagina voor de oplossing, selecteer **verwijderen**. Wanneer u hierom wordt gevraagd om te bevestigen, selecteert u **Ja**.  
6. Selecteer in de lijst met oplossingen **ServiceMap (Werkruimtenaam)** . Op de **overzicht** pagina voor de oplossing, selecteer **verwijderen**. Wanneer u hierom wordt gevraagd om te bevestigen, selecteert u **Ja**.  

Voordat u Azure Monitor ingeschakeld voor virtuele machines, als u niet hebt gedaan [verzamelen van prestatiemeteritems](vminsights-enable-overview.md#performance-counters-enabled) voor de Windows- of Linux-VM's in uw werkruimte [uitschakelen van deze regels](../platform/data-sources-performance-counters.md#configuring-performance-counters) voor Windows en Linux.

## <a name="disable-monitoring-and-keep-the-workspace"></a>Schakel bewaking en het behouden van de werkruimte  

Als uw Log Analytics-werkruimte moet ondersteuning bieden voor bewaking van andere bronnen, de volgende stappen om uit te schakelen op de virtuele machine die u gebruikt voor het evalueren van Azure Monitor voor virtuele machines bewaken. Voor Azure VM's verwijdert u de agent voor afhankelijkheden VM-extensie en de Log Analytics-agent VM-extensie voor Windows of Linux rechtstreeks vanuit de virtuele machine. 

>[!NOTE]
>De Log Analytics-agent niet worden verwijderd als: 
>
> * Azure Automation beheert de virtuele machine voor het indelen van processen of voor het beheren van configuratie- of -updates. 
> * Azure Security Center Hiermee beheert u de virtuele machine voor beveiliging en detectie van bedreigingen. 
>
> Als u de Log Analytics-agent verwijdert, u wordt voorkomen dat deze services en oplossingen proactief beheren van uw virtuele machine. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 
2. Selecteer in de Azure portal, **virtuele Machines**. 
3. Selecteer een VM in de lijst. 
4. Selecteer aan de linkerkant **extensies**. Op de **extensies** weergeeft, schakelt **DependencyAgent**.
5. Selecteer op de eigenschappenpagina van de extensie **verwijderen**.
6. Op de **extensies** weergeeft, schakelt **MicrosoftMonitoringAgent**. Selecteer op de eigenschappenpagina van de extensie **verwijderen**.  
