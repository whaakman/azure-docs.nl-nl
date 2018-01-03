---
title: Netwerk groep stroom beveiligingslogboeken met Azure-netwerk-Watcher beheren | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u voor het beheren van Netwerkbeveiligingsgroep stroom Logboeken in Azure-netwerk-Watcher
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 01606cbf-d70b-40ad-bc1d-f03bb642e0af
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 633543aba99f5c09b14a9e4b11adf59ca04d0fe5
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="manage-network-security-group-flow-logs-in-the-azure-portal"></a>Netwerk groep stroom beveiligingslogboeken in de Azure portal beheren

> [!div class="op_single_selector"]
> - [Azure Portal](network-watcher-nsg-flow-logging-portal.md)
> - [PowerShell](network-watcher-nsg-flow-logging-powershell.md)
> - [CLI 1.0](network-watcher-nsg-flow-logging-cli-nodejs.md)
> - [CLI 2.0](network-watcher-nsg-flow-logging-cli.md)
> - [REST API](network-watcher-nsg-flow-logging-rest.md)

Groep netwerk-stroom beveiligingslogboeken zijn een functie van netwerk-Watcher waarmee u informatie bekijken over inkomende en uitgaande IP-verkeer via een netwerkbeveiligingsgroep. Deze stroom logboeken zijn geschreven in JSON-indeling en belangrijke informatie bevatten met inbegrip van: 

- Binnenkomende en uitgaande stromen per regel op basis van een.
- De NIC die de stroom is van toepassing op.
- 5-tuple informatie over de stroom (bron/het doel-IP-poort van de bron/het doel, protocol).
- Informatie over of verkeer is toegestaan of geweigerd.

## <a name="before-you-begin"></a>Voordat u begint

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een exemplaar van de netwerk-Watcher](network-watcher-create.md). Het scenario wordt ervan uitgegaan dat u hebt een resourcegroep met een geldige virtuele machine.

## <a name="register-insights-provider"></a>Insights provider registreren

Voor verkeersstroom logboekregistratie om te werken, de **Microsoft.Insights** provider moet worden geregistreerd. Voor het registreren van de provider, moet u de volgende stappen uitvoeren: 

1. Ga naar **abonnementen**, en selecteer vervolgens het abonnement waarvoor u wilt inschakelen Logboeken van de stroom. 
2. Op de **abonnement** blade Selecteer **Resourceproviders**. 
3. Bekijk de lijst met providers en controleer of de **microsoft.insights** provider is geregistreerd. Zo niet, selecteer vervolgens **registreren**.

![Weergave-providers][providers]

## <a name="enable-flow-logs"></a>Stroom Logboeken inschakelen

Deze stappen gaat u door het proces van het inschakelen van Logboeken van de stroom op een netwerkbeveiligingsgroep.

### <a name="step-1"></a>Stap 1

Ga naar een netwerk-Watcher-exemplaar en selecteer vervolgens **NSG stromen logboeken**.

![Overzicht van de stroom-Logboeken][1]

### <a name="step-2"></a>Stap 2

Selecteer een netwerkbeveiligingsgroep in de lijst.

![Overzicht van de stroom-Logboeken][2]

### <a name="step-3"></a>Stap 3 

Op de **stroom logboeken instellingen** blade zet de status op **op**, en configureer vervolgens een opslagaccount.  Wanneer u bent klaar, selecteert u **OK**. Selecteer vervolgens **opslaan**.

![Overzicht van de stroom-Logboeken][3]

## <a name="download-flow-logs"></a>Stroom logboeken downloaden

Stroom logboeken worden opgeslagen in een opslagaccount. Download uw logboeken stroom om ze te bekijken.

### <a name="step-1"></a>Stap 1

Stroom logboeken downloaden, selecteer **kunt u stroom logboeken downloaden van de geconfigureerde opslagaccounts**. Deze stap gaat u naar de weergave van een storage-account waarin u kunt kiezen welke logboeken te downloaden.

![Instellingen voor stroomlogboeken][4]

### <a name="step-2"></a>Stap 2

Ga naar het juiste storage-account. Selecteer vervolgens **Containers** > **insights-log-networksecuritygroupflowevent**.

![Instellingen voor stroomlogboeken][5]

### <a name="step-3"></a>Stap 3

Ga naar de locatie van het logboek stroom, selecteert u deze en selecteer vervolgens **downloaden**.

![Instellingen voor stroomlogboeken][6]

Voor informatie over de structuur van het logboek, gaat u naar [netwerk groep stroom logboek beveiligingsoverzicht](network-watcher-nsg-flow-logging-overview.md).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe [visualiseren van uw NSG stroom logboeken met Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md).

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-portal/figure1.png
[2]: ./media/network-watcher-nsg-flow-logging-portal/figure2.png
[3]: ./media/network-watcher-nsg-flow-logging-portal/figure3.png
[4]: ./media/network-watcher-nsg-flow-logging-portal/figure4.png
[5]: ./media/network-watcher-nsg-flow-logging-portal/figure5.png
[6]: ./media/network-watcher-nsg-flow-logging-portal/figure6.png
[providers]: ./media/network-watcher-nsg-flow-logging-portal/providers.png
