---
title: Problemen oplossen met Azure Virtual Network Gateway en verbindingen - Portal | Microsoft Docs
description: Deze pagina wordt uitgelegd hoe u de netwerk-Watcher van Azure Portal oplossen
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: f6f0a813-38b6-4a1f-8cfc-1dfdf979f595
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: jdial
ms.openlocfilehash: fb8f60eea0eaa24649f584ae4183aa8e552a433e
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="troubleshoot-virtual-network-gateway-and-connections-using-network-watcher-from-azure-portal"></a>Problemen met virtuele netwerkgateway en verbindingen met behulp van netwerk-Watcher vanuit Azure Portal

> [!div class="op_single_selector"]
> - [Portal](network-watcher-troubleshoot-manage-portal.md)
> - [PowerShell](network-watcher-troubleshoot-manage-powershell.md)
> - [CLI 1.0](network-watcher-troubleshoot-manage-cli-nodejs.md)
> - [CLI 2.0](network-watcher-troubleshoot-manage-cli.md)
> - [REST API](network-watcher-troubleshoot-manage-rest.md)

Netwerk-Watcher biedt veel mogelijkheden met betrekking tot het begrijpen van uw netwerkbronnen in Azure. Een van deze mogelijkheden resource is het oplossen van problemen. Het oplossen van bron kan worden aangeroepen via de portal, PowerShell, CLI of REST-API. Als deze wordt aangeroepen, netwerk-Watcher inspecteert de status van de Gateway van een virtueel netwerk of een verbinding en retourneert de bevindingen zijn.

## <a name="before-you-begin"></a>Voordat u begint

Dit scenario wordt ervan uitgegaan dat u de stappen in al hebt gevolgd [maken van een netwerk-Watcher](network-watcher-create.md) voor het maken van een netwerk-Watcher.

Voor een lijst met ondersteunde gateway typen bezoek, [ondersteund gatewaytypen](network-watcher-troubleshoot-overview.md#supported-gateway-types).

## <a name="overview"></a>Overzicht

Het oplossen van resource biedt de mogelijkheid oplossen van problemen die zich bij het virtuele netwerkgateways en verbindingen voordoen. Wanneer een aanvraag wordt gedaan bij resource probleemoplossing, logboeken worden opgevraagd en geïnspecteerd. Als controle voltooid is, worden de resultaten worden geretourneerd. Resources voor probleemoplossing aanvragen zijn langlopende aanvraagt, die meerdere minuten kan duren om een resultaat te retourneren. De logboeken van het oplossen van problemen worden opgeslagen in een container voor een opslagaccount die is opgegeven.

## <a name="troubleshoot-a-gateway-or-connection"></a>Problemen oplossen met een gateway of de verbinding

1. Navigeer naar de [Azure-portal](https://portal.azure.com) en klik op **Networking** > **netwerk-Watcher** > **VPN-diagnostische gegevens**
2. Selecteer een **abonnement**, **resourcegroep**, en **locatie**.
3. Het oplossen van resource retourneert gegevens over de status van de resource. Bovendien bespaart u relevante logboeken naar een opslagaccount moeten worden gecontroleerd. Klik op **Opslagaccount** een opslagaccount selecteren.
4. Op de **opslagaccounts** blade, selecteer een bestaand opslagaccount of klik op **+ opslagaccount** naar een nieuwe maken.
5. Op de **Containers** blade, kies een bestaande container of klik op **+ Container** voor het maken van een nieuwe container. Klik wanneer u klaar **selecteren**
6. Selecteer de Gateway en verbinding resources oplossen en klik op **probleemoplossing starten**

Als meerdere resources zijn geselecteerd, wordt het oplossen van problemen gelijktijdig op bronnen van de gateway uitgevoerd. Taak kan niet worden uitgevoerd op een verbinding en de bijbehorende gateway op hetzelfde moment. Het verdient aanbeveling om op te lossen gateways eerst verbinding probleemoplossing is meer tijd in beslag. Terwijl VPN-diagnostische gegevens van een resource wordt uitgevoerd de **probleemoplossing STATUS** kolom geeft de status van **uitgevoerd**

Na voltooiing wordt de statuskolom verandert in **goed** of **niet in orde**.

![volledige oplossen][2]

## <a name="understanding-the-results"></a>Inzicht in de resultaten

In de **Details** gedeelte van het venster de **Status** tabblad toont de status van de laatste troubleshooting uitvoeren op de geselecteerde bron. Resultaten van de meest recente diagnose zijn weergegeven xx minuten na de laatste uitvoering.

|Eigenschap  |Beschrijving  |
|---------|---------|
|Resource     | Een koppeling naar de resource.        |
|Pad voor opslag     |  Pad naar het opslagaccount en container die de logboeken bevatten (als een zijn gemaakt tijdens de uitvoering). Deze instelling niet bewaard is gebleven nadat u de portal verlaten.        |
|Samenvatting     | Samenvatting van de resourcestatus.        |
|Detail     | Gedetailleerde informatie over de resourcestatus.        |
|Laatst uitgevoerd     | De tijd die de laatste keer het oplossen van problemen is uitgevoerd.        |


De **actie** tabblad bevat algemene richtlijnen over het oplossen van het probleem. Als een actie kan worden uitgevoerd voor de uitgifte, wordt een koppeling geboden met aanvullende richtlijnen. In het geval wanneer er geen aanvullende richtlijnen, het antwoord geeft de url om een ondersteuningsaanvraag te openen.  Voor meer informatie over de eigenschappen van het antwoord en wat is opgenomen, gaat u naar [overzicht van netwerk-Watcher oplossen](network-watcher-troubleshoot-overview.md)


## <a name="next-steps"></a>Volgende stappen

Als de instellingen zijn gewijzigd die stop VPN-verbindingen, raadpleegt u [Netwerkbeveiligingsgroepen beheren](../virtual-network/virtual-network-manage-nsg-arm-portal.md) voor het opsporen van de groep en beveiliging netwerkbeveiligingsregels die betrokken zijn.


[2]: ./media/network-watcher-troubleshoot-manage-portal/2.png
