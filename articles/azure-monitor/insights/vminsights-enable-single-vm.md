---
title: Azure Monitor voor virtuele machines (preview) inschakelen voor de evaluatie | Microsoft Docs
description: Dit artikel wordt beschreven hoe u Azure Monitor inschakelen voor virtuele machines voor een enkele virtuele Azure-machine of virtuele-machineschaalset voor evaluatiedoeleinden.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2019
ms.author: magoedte
ms.openlocfilehash: 673f153b551e4b0c89a564c96d6bd9819ca26f5d
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2019
ms.locfileid: "65524083"
---
# <a name="enable-azure-monitor-for-vms-preview-for-evaluation"></a>Azure Monitor voor virtuele machines (preview) inschakelen voor de evaluatie

Als u wilt evalueren Azure Monitor voor virtuele machines (preview) op een klein aantal virtuele machines van Azure of een enkele virtuele machine of virtuele-machineschaalset, is de eenvoudigste en meest directe methode bewaking wilt inschakelen vanuit de Azure-portal. Aan het einde van dit proces, u wordt is begonnen met het bewaken van deze en leer als problemen met prestaties of beschikbaarheid ondervinden. 

Voordat u aan de slag gaat, worden lees de [vereisten](vminsights-enable-overview.md) en controleer of uw abonnement en resources voldoen aan de vereisten.  

## <a name="enable-monitoring-for-a-single-azure-vm"></a>Schakel de bewaking voor een enkele Azure-VM
Voor bewaking van uw Azure-VM in Azure portal, het volgende doen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer **virtuele Machines**.

1. Selecteer een VM in de lijst.

1. Op de pagina virtuele machine in de **bewaking** sectie, selecteer **inzichten (preview)**.

1. Op de **inzichten (preview)** weergeeft, schakelt **Probeer nu**.

    ![Azure Monitor voor virtuele machines voor een virtuele machine inschakelen](./media/vminsights-enable-single-vm/enable-vminsights-vm-portal-01.png)

1. Op de **Azure Monitor Insights voorbereiden** pagina, hebt u een bestaande Log Analytics-werkruimte in hetzelfde abonnement, selecteert u deze in de vervolgkeuzelijst.  

    De lijst worden er de standaardwerkruimte en de locatie die de virtuele machine is geïmplementeerd op in het abonnement. 

    >[!NOTE]
    >Als u maken van een nieuwe werkruimte voor logboekanalyse wilt voor het opslaan van de bewakingsgegevens van de virtuele machine, volgt u de instructies in [een Log Analytics-werkruimte maken](../../azure-monitor/learn/quick-create-workspace.md) in een van de ondersteunde regio's vermeld [hier](vminsights-enable-overview.md#log-analytics).

Wanneer u bewaking inschakelt, is het duurt ongeveer 10 minuten voordat u de status van metrische gegevens voor de virtuele machine kunt weergeven.

![Azure Monitor inschakelen voor virtuele machines verwerking van de implementatie controleren](./media/vminsights-enable-single-vm/onboard-vminsights-vm-portal-status.png)

## <a name="enable-monitoring-for-a-single-virtual-machine-scale-set"></a>Schakel bewaking voor een enkele virtuele-machineschaalset

Als u wilt inschakelen voor bewaking van uw schaalset voor virtuele Azure-machine in Azure portal, het volgende doen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer **Virtual Machine Scale Sets**.

3. Selecteer een virtuele-machineschaalset in de lijst.

4. Op de virtuele machine schaalset pagina in de **bewaking** sectie, selecteer **inzichten (preview)**.

5. Op de **inzichten (preview)** pagina, hebt u een bestaande Log Analytics-werkruimte die u wilt gebruiken, selecteert u deze in de vervolgkeuzelijst.

    De lijst worden er de standaardwerkruimte en de locatie die de virtuele machine is geïmplementeerd op in het abonnement. 

    ![Azure Monitor voor virtuele machines inschakelen voor een virtuele-machineschaalset](./media/vminsights-enable-single-vm/enable-vminsights-vmss-portal-01.png)

    >[!NOTE]
    >Als u maken van een nieuwe werkruimte voor logboekanalyse wilt voor het opslaan van de bewakingsgegevens van de virtuele machine, volgt u de instructies in [een Log Analytics-werkruimte maken](../learn/quick-create-workspace.md) in een van de ondersteunde regio's vermeld [hier](vminsights-enable-overview.md#log-analytics).

Wanneer u bewaking inschakelt, is het duurt ongeveer 10 minuten voordat u de bewakingsgegevens voor de schaalset kunt weergeven.

>[!NOTE]
>Als u een handmatige upgrade model voor uw schaalset moet u upgraden van de instanties om de installatie te voltooien.  Dit kan worden uitgevoerd vanaf de pagina exemplaren onder de **instellingen** sectie.

![Azure Monitor inschakelen voor virtuele machines verwerking van de implementatie controleren](./media/vminsights-enable-single-vm/onboard-vminsights-vmss-portal-status-01.png)

## <a name="next-steps"></a>Volgende stappen

Nu dat de controle is ingeschakeld voor uw virtuele machine of virtuele-machineschaalset, is deze informatie is beschikbaar voor analyse met Azure Monitor voor virtuele machines. Zie voor meer informatie over het gebruik van de Health-functie, [weergave Azure Monitor voor virtuele machines Health](vminsights-health.md). Afhankelijkheden van gedetecteerde toepassingen, Zie [weergave Azure Monitor voor virtuele machines kaart](vminsights-maps.md). Voor het identificeren van knelpunten en het algehele gebruik met de prestaties van uw VM's, Zie [weergave Azure VM-prestaties](vminsights-performance.md), of als u afhankelijkheden van gedetecteerde toepassingen, Zie [weergave Azure Monitor voor virtuele machines kaart](vminsights-maps.md).