---
title: Azure Monitor voor virtuele machines (preview) inschakelen voor de evaluatie | Microsoft Docs
description: Informatie over het evalueren van Azure Monitor voor virtuele machines op een enkele Azure-machine of op een virtuele-machineschaalset.
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
ms.openlocfilehash: ec909bcd16f923bbd7036f6a69df2bbb07e561b8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67122533"
---
# <a name="enable-azure-monitor-for-vms-preview-for-evaluation"></a>Azure Monitor voor virtuele machines (preview) inschakelen voor de evaluatie

U kunt Azure Monitor evalueren voor virtuele machines (preview) op een klein aantal virtuele Azure-machines (VM's) of op een schaal van één virtuele machine of virtuele machine instellen. De eenvoudigste en meest directe manier om in te schakelen bewaking is van de Azure-portal. Het doel is om te controleren van uw virtuele machines en prestaties of beschikbaarheidsproblemen te detecteren. 

Voordat u begint, controleert u de [vereisten](vminsights-enable-overview.md) en zorg ervoor dat uw abonnement en resources voldoen aan de vereisten.  

## <a name="enable-monitoring-for-a-single-azure-vm"></a>Schakel de bewaking voor een enkele Azure-VM
Inschakelen van bewaking van uw Azure-VM:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer **virtuele Machines**.

1. Selecteer een VM in de lijst.

1. Op de pagina virtuele machine in de **bewaking** sectie, selecteer **inzichten (preview)** .

1. Op de **inzichten (preview)** weergeeft, schakelt **Probeer nu**.

    ![Azure Monitor voor virtuele machines voor een virtuele machine inschakelen](./media/vminsights-enable-single-vm/enable-vminsights-vm-portal-01.png)

1. Op de **Azure Monitor Insights voorbereiden** pagina, hebt u een bestaande Log Analytics-werkruimte in hetzelfde abonnement, selecteert u deze in de vervolgkeuzelijst.  

    De lijst worden er de standaardwerkruimte en de locatie waar de virtuele machine is geïmplementeerd in het abonnement. 

    >[!NOTE]
    >Zie voor het maken van een nieuwe werkruimte voor logboekanalyse voor het opslaan van de bewakingsgegevens van de virtuele machine, [een Log Analytics-werkruimte maken](../../azure-monitor/learn/quick-create-workspace.md). Uw Log Analytics-werkruimte moet behoren tot een van de [ondersteunde regio's](vminsights-enable-overview.md#log-analytics).

Wanneer u bewaking inschakelt, moet u mogelijk wacht tien minuten voordat u de status van metrische gegevens voor de virtuele machine weergeven kunt.

![Azure Monitor inschakelen voor virtuele machines verwerking van de implementatie controleren](./media/vminsights-enable-single-vm/onboard-vminsights-vm-portal-status.png)

## <a name="enable-monitoring-for-a-single-virtual-machine-scale-set"></a>Schakel bewaking voor een enkele virtuele-machineschaalset

Inschakelen van bewaking van uw schaalset voor virtuele Azure-machine:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer **Virtual Machine Scale Sets**.

3. Selecteer een virtuele-machineschaalset in de lijst.

4. Op de virtuele machine schaalset pagina in de **bewaking** sectie, selecteer **inzichten (preview)** .

5. Op de **inzichten (preview)** pagina als u wilt gebruiken van een bestaande Log Analytics-werkruimte, selecteert u deze in de vervolgkeuzelijst.

    De lijst worden er de standaardwerkruimte en de locatie die de virtuele machine is geïmplementeerd op in het abonnement. 

    ![Azure Monitor voor virtuele machines inschakelen voor een virtuele-machineschaalset](./media/vminsights-enable-single-vm/enable-vminsights-vmss-portal-01.png)

    >[!NOTE]
    >Zie voor het maken van een nieuwe werkruimte voor logboekanalyse voor het opslaan van de bewakingsgegevens van de virtuele-machineschaalset [een Log Analytics-werkruimte maken](../learn/quick-create-workspace.md). Uw Log Analytics-werkruimte moet behoren tot een van de [ondersteunde regio's](vminsights-enable-overview.md#log-analytics).

Wanneer u bewaking inschakelt, moet u mogelijk wacht tien minuten voordat u de bewakingsgegevens voor de schaalset kunt weergeven.

>[!NOTE]
>Als u een handmatige upgrade model voor uw schaalset, voer een upgrade instanties om de installatie te voltooien. U kunt beginnen met de upgrade van de **exemplaren** pagina, in de **instellingen** sectie.

![Azure Monitor inschakelen voor virtuele machines verwerking van de implementatie controleren](./media/vminsights-enable-single-vm/onboard-vminsights-vmss-portal-status-01.png)

Nu dat u de bewaking voor uw schaalset voor virtuele machine of virtuele machine hebt ingeschakeld, is de controlegegevens beschikbaar zijn voor analyse in Azure Monitor voor virtuele machines. 

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het gebruik van de Health-functie, [inzicht in de status van uw virtuele machines van Azure Monitor](vminsights-health.md). 
* Afhankelijkheden van gedetecteerde toepassingen, Zie [gebruikt Azure Monitor voor virtuele machines kaart](vminsights-maps.md). 
* Zie voor het identificeren van knelpunten, het algehele gebruik en prestaties van de virtuele machine, [weergave Azure VM-prestaties](vminsights-performance.md).