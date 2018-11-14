---
title: Upgrade uitvoeren van Azure Monitor voor de agent containers (Preview) | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een upgrade van de Log Analytics-agent die wordt gebruikt door Azure Monitor voor containers uitvoeren.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2018
ms.author: magoedte
ms.openlocfilehash: fb45002d284bc28dfb7093f69cfac7aae0681e8d
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628986"
---
# <a name="how-to-upgrade-the-azure-monitor-for-containers-preview-agent"></a>Upgrade uitvoeren van de Azure-Monitor voor de agent containers (Preview)
Azure Monitor voor containers maakt gebruik van een beperkte versie van de Log Analytics-agent voor Linux. Wanneer een nieuwe versie van de agent wordt uitgebracht, wordt de agent niet automatisch bijgewerkt op uw beheerde Kubernetes-clusters die worden gehost in Azure Kubernetes Service (AKS).

In dit artikel beschrijft het proces voor het bijwerken van de agent.

## <a name="upgrading-agent-on-monitored-kubernetes-cluster"></a>Agent op de bewaakte Kubernetes-cluster upgraden
Het proces voor het bijwerken van de agent bestaat uit twee duidelijk stappen. De eerste stap is om uit te schakelen controleren met Azure Monitor voor containers met behulp van Azure CLI.  Volg de stappen de [uitschakelen bewaking](monitoring-container-insights-optout.md?toc=%2fazure%2fmonitoring%2ftoc.json#azure-cli) artikel. Met behulp van Azure CLI, kunnen we de agent verwijderen van de knooppunten in het cluster zonder gevolgen voor de oplossing en de bijbehorende gegevens die zijn opgeslagen in de werkruimte. 

>[!NOTE]
>Terwijl u deze activiteit onderhoud uitvoert, de knooppunten in het cluster niet van verzamelde gegevens doorsturen zijn en prestatieweergaven worden geen gegevens weergegeven tussen het moment dat u de agent verwijderen en de nieuwe versie installeert. 
>

Voor het installeren van de nieuwe versie van de agent, volg de stappen de [ingebouwde bewaking](monitoring-container-insights-onboard.md?toc=%2fazure%2fmonitoring%2ftoc.json#enable-monitoring-using-azure-cli) artikel met behulp van Azure CLI, om dit proces te voltooien.  

Wanneer u bewaking opnieuw inschakelt, is het duurt ongeveer 15 minuten voordat u de bijgewerkte status metrische gegevens voor het cluster kunt weergeven. 

## <a name="next-steps"></a>Volgende stappen
Als u problemen ondervindt tijdens het bijwerken van de agent, raadpleegt u de [problemen oplossen met](monitoring-container-insights-troubleshoot.md) voor ondersteuning.